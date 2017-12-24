---
layout: post
title: Organizando as notas da biblioteca de músicas
---

Essa ideia começou quando migrei do Windows pro Ubuntu. Na época a coleção tava
começando a crescer, tava com uns 100 GB de arquivos (hoje são 150 GB)... e eu ouvia muita música
naquele ano, cerca de dois álbuns novos por dia (era o que eu conseguia ouvir
sem ficar enjoada de novidades no outro dia). Usava o MediaMonkey pra avaliar as
músicas, de 1 a 5 estrelas.

Ao migrar pro Ubuntu, precisei trocar o programa pra tocar músicas, já que não
havia uma versão do MediaMonkey pra Linux (usar com o Wine nem me passou pela
cabeça). Depois de procurar por vários programas que tivessem as mesmas
funcionalidades e não comessem muita memória do computador, instalei o
Guayadeque e é o que eu uso até hoje na maior parte do tempo.

Um dos problemas que apareceram foi que as avaliações guardadas no MediaMonkey
não eram mostradas no Guayadeque. Enquanto o MediaMonkey gravava a avaliação na
tag POPM com um valor de 0 a 255 direto no arquivo MP3, o Guayadeque guardava
essa informação localmente no programa. Se você trocar as músicas de lugar, a
avaliação some.

Aí surgiu a necessidade de fazer um backup disso tudo. Imagina se o HD pifa de
uma hora pra outra? Tudo que tava registrado nesses dois anos vai desaparecer.
No começo tentei fazer manualmente, mas... são mais de 100 mil arquivos de
música, 150 GB... é muita coisa!

Pra resolver isso, decidi extrair as avaliações que eu já fiz durante dois anos
de coleção de músicas e álbuns com um script pra um formato mais portável. A primeira ideia
foi usar arquivos CSV com os dados estruturados desta maneira (o título do
arquivo era o nome do álbum):

Número da faixa, Título, Nota

Na primeira tentativa pra fazer esse script (cerca de Julho, Agosto de 2016,
quando eu tinha acabado de migrar), não consegui avançar muito. Atacando o
problema de volta este ano com mais conhecimentos em Ruby, consegui montar uma
versão funcional. O arquivo completo está [aqui](https://github.com/Auralcat/100-days-of-code/blob/master/scripts/songsToCSV.rb).

```
# You can use Find.find to get the list of directories to crawl:
def get_album_dir_list(music_library_dir)
  dir_list = []
  Find.find(music_library_dir).each do |filepath|
    sanitize = filepath.split("/")
    dir_list << sanitize.slice(0, sanitize.length - 1).join("/")
  end

  # Removing duplicates and sorting
  dir_list.uniq!.sort!

  # Crawl the deepest directories found:
  longest_dir_length = dir_list.map{|x| x.split("/").length}.max - 3
  dir_list.select{|x| x.split("/").length == longest_dir_length}
end
```

O primeiro passo do script é achar onde está a biblioteca de músicas. No meu
caso, organizo assim:

Gênero -> Artista -> Álbum -> Disco (quando há mais de um disco por álbum) ->
Faixas

Usando a gem `find`, é possível andar pelo diretório e pegar o nome dos
diretórios dos álbuns para passar para a criação do arquivo CSV, além de apontar o programa pra onde
deve buscar as notas, já que `Find.find` retorna strings.

Depois de fazer a triagem inicial, removemos os caminhos onde não há músicas
(exemplos: ~/Metal/Metallica e ~/Metal; essas pastas contém outras pastas e não
arquivos de música no mesmo nível).

Também é necessário determinar o caminho mais comprido do array pra filtrar o
resultado final. Queremos que o programa passe pelo nível mais profundo da biblioteca.

```
def extract(raw_tags, target, sep=target)
  # Extracts the ratings from the output of id3info
  raw_tags.select{|x| x.include?(target)}.map{|x| (x.split(sep).pop)}
end
```

O `id3info` é a única _hard-dependency_ do script, já que a gem `find` e a gem `csv`
são parte da biblioteca padrão do Ruby. A saída do programa geralmente é dessa
forma:

```
*** Tag information for 01. i am crazy.mp3
=== TALB (Album/Movie/Show title): 3 Knocks - Disk 1 (Well Being)
=== TPE1 (Lead performer(s)/Soloist(s)): Pendulum
=== TCON (Content type): Rock
=== TIT2 (Title/songname/content description): 01 -I Am Crazy
=== TRCK (Track number/Position in set): 1
=== POPM (Popularimeter): MusicBee, counter=0 rating=196
*** mp3 info
MPEG1/layer III
Bitrate: 192KBps
Frequency: 44KHz
```

O método `extract` serve pra tirar só o valor do campo que estamos procurando,
tanto pra POPM quanto pra TIT2. O número da faixa é obtido pelo nome do arquivo,
no meu caso todos os arquivos são nomeados na estrutura "<número_da_faixa> - <título>".

```
# Making sure we're inside the empty Ratings dir, and making sure
# it exists if it hasn't been created already
music_library_dir = Dir.home + "/Música"
Dir.chdir(music_library_dir)
Dir.mkdir("Ratings") if Dir.glob("Ratings") == []
puts "You're in #{Dir.pwd}"

dir_list = get_album_dir_list(music_library_dir)

```

Esse é o ponto inicial da execução do script. Primeiro precisamos saber onde
está a biblioteca de músicas. Uma sugestão de melhoria é fazer a pessoa usuária
especificar onde está a biblioteca dela.
Feito isso, vamos para o diretório base da biblioteca e criamos a pasta
"Ratings" (se ela já não existir), onde vamos colocar os arquivos CSV. Daí
pegamos a lista de álbuns dentro dessa biblioteca.

```
dir_list.each do |dirpath|
  # File name should be the album's name, which is in the end of the dir names.
  copy_dir_name = dirpath
  album_name = copy_dir_name.split("/").pop
    CSV.open("#{music_library_dir}/Ratings/#{album_name}", "wb") do |csv_file|
    csv_file << ["Track Number", "Track Title", "Track Rating"]
    count = 1
    Dir.chdir(dirpath)
    puts "Crawling #{dirpath}..."

    Dir.glob("*\.{mp3,flac,m4a}").sort do |file|
      puts "Checking file #{dirpath}/#{file}"
      # puts `id3info "#{dirpath}/#{file}"`
      raw_tags = `id3info "#{dirpath}/#{file}"`.encode!('UTF-8',
      'UTF-8', :invalid => :replace).split("\n")
      puts raw_tags
      # Extract the info you want here...
      rating = extract(raw_tags, "POPM", "rating=")
      track_title = file
      # After that...
      csv_file << [count, album_name, track_title, rating.pop.to_i/32]
      count += 1
    end
  end
  puts "CSV file for #{dirpath} was saved."
end
```

Com a lista de diretórios pra buscar, criamos o arquivo CSV com a estrutura
especificada anteriormente e começamos a extrair as notas de cada arquivo MP3.
Esse ponto é importante: arquivos FLAC ou M4A não não processados pelo `id3info`.
Todos os arquivos CSV são colocados dentro da mesma pasta sem nenhuma ordenação
anterior.

A ideia depois de conseguir os arquivos CSV pra cada álbum era colocar tudo em
uma planilha pra cada artista. Como as notas serviam apenas para referência, não
havia necessidade de olhar pra esses dados o tempo inteiro. A motivação do
script era mais pra arquivamento mesmo: as planilhas e arquivos CSV não ocupam
tanto espaço quanto um arquivo MP3 e com certeza menos que arquivos FLAC. A
tendência pra capacidade de armazenamento de dados em geral é aumentar, por
enquanto é o que eu consigo fazer com os recursos que eu tenho.

Outra vantagem de separar o armazenamento e registro das notas do programa
usado pra tocar as músicas é que isso deixa um requerimento a menos pra trocar
de programa caso apareça um melhor: não preciso mais escolher o próximo player
de música com base em uma maneira de mostrar e registrar notas pras músicas,
posso escolher com base em outros critérios.

Por fim, voltamos pra casa:

```
puts "Taking you home..."
Dir.chdir(Dir.home)
```

Alguns pontos onde dá pra melhorar:

- Perguntar onde está o diretório base da biblioteca de músicas
- Remover os "números mágicos" dos métodos
- Cuidar de casos onde há álbuns com o mesmo nome, de artistas diferentes
