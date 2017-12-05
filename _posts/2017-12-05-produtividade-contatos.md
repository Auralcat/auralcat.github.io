Testando um sistema de organização de atividades por contatos

Há umas semanas, conversando no Telegram, apareceu esta mensagem do @mr_octopus:

```
Produtividade deveria ser baseada em contatos. Ao meu ver, gerenciamento de contatos é algo essencial.

O finado PalmOS, quando o Rubinstein apresentou o webOS, tinha algo ali de sensacional sendo apresentado: o Synergy.

O conceito de organizar tudo centralizado no contato é excelente. Independente da origem do contato (WhatsApp, LinkedIn,
SMS, ligação, e-mail, etc), deveria ser padrão vermos um contato na agenda e ali ver todo o histórico de comunicação com
aquela pessoa. Ou grupos de pessoas, classificados por qualquer que seja o método (empresa, assunto, nível de
relacionamento, etc).

Pois o que quer que você produza, qualquer trabalho, é feito para alguém e no fim tanto faz o meio, o que vale é o
público alvo. De um emoticon para sua companheira a uma proposta de trabalho, ou artigo publicado: são pessoas, sempre.
```

Pra testar o conceito, utilizei o org-mode, do Emacs. As atividades foram organizadas por arquivos .org com o nome de
cada contato. Dentro de cada arquivo, é possível colocar lista de tarefas, informação de contato, links para arquivos e
outros protocolos (http, ftp, link para emails dos clientes do Emacs, itens do Gnus, IRC e mais), tabelas... tudo é
organizado em texto puro, sem formatos proprietários ou que necessitam de um programa específico para trabalhar com o
arquivo. É possível usar outros programas para processar o texto do arquivo .org da maneira que o usuário quiser.

A ideia do org-mode é trazer as tarefas à tona a partir de anotações estruturadas. Ele permite mover cada cabeçalho pra
onde você quiser e marcar os itens com atividades como tarefas colocando a palavra TODO na frente deles.

O org-mode também possui uma agenda embutida, onde é possível ver os itens da lista de tarefas em um painel só. Dá pra
separar por dia, por semana, da maneira que a pessoa preferir. No caso, os itens apareciam de acordo com cada arquivo
que foi adicionado à agenda.

Escolhi testar no Emacs por essa flexibilidade para montar o sistema da maneira que ficar mais confortável e também por
já passar a maior parte do tempo dentro dele. Foi o terreno mais fértil pra provar o conceito.  Não é o sistema mais
amigável por aí. Todoist e Wunderlist são exemplos mais prontos pra usar imediatamente após a instalação no seu
dispositivo. Outro ponto fraco do Org mode por enquanto é que não há um aplicativo para dispositivos móveis pronto pra
acoplar aos arquivos do desktop, e também há a limitação do próprio Emacs. Ele foi feito pra ser usado com um teclado
físico, não há muita conveniência no teclado de celular. Dá pra aliviar um pouco esse desconforto com o evil-mode, mas
mesmo assim, não é o meio nativo dele.

Impressões

O fluxo de trabalho se deu do seguinte modo: as tarefas estavam listadas na agenda com datas de entrega e datas
agendadas, dependendo do item. As tarefas foram marcadas com datas de entrega (C-c C-d) e os compromissos não tinham o
prefixo TODO, eles foram marcados pelo comando de agendamento do org-mode (C-c C-s).

(coloque algumas fotos)

Para marcar um item como terminado (DONE), o comando era C-c C-t d (coloquei alguns rótulos a mais, os dois padrões são
TODO e DONE). No caso, d estava associado a DONE.
Em termos de organização, a visão dos itens ficou muito mais limpa do que se tivessem sido colocados em um único
arquivo. Não tanto na visão de agenda, mas na hora do planejamento e de registrar a atividade. O nome de cada contato
ajudava a lembrar o contexto da atividade. Todas as informações para entrega de cada item também estavam anexadas ao
arquivo .org do contato.

A ideia do experimento foi implementar o sistema independente da plataforma. Dá pra reproduzir o que foi feito aqui em
qualquer aplicativo de listas de tarefas. Pode ser feito no Evernote, Todoist, Wunderlist, Habitica, onde a pessoa preferir.
O ganho se deu na organização das informações referentes à entrega da tarefa. Em outras palavras, todo o contexto da
tarefa, materiais necessários para execução, onde falar com o contato quando houver algum imprevisto.

Um dos pontos levantados no mesmo dia da conversa foi como registrar as tarefas para a própria pessoa. Itens como perder
peso, atividades pessoais. Criei um arquivo chamado eu.org e registrei tudo lá. Por sinal, foi o que continha mais tarefas.

Essa foi a primeira maneira que deu certo pra organizar minhas tarefas e executar. Um dos problemas mais
frequentes com outros sistemas era não ter à mão tudo o que era necessário pra executar algum item e pra quem
entregar. Antigamente jogava tudo em uma lista só e ficava lá. Outro problema era olhar o que eu deixava lá. Na maior
parte do tempo ficava esquecido.

* Observações
- Dependendo da quantidade de contatos, o número de listas pode ficar bem grande. Achar o contato específico pode ser
  complicado dependendo da forma de busca do aplicativo. No caso do Emacs, isso não chegou a ser problema por causa do
  ido-mode, que permite uma busca mais rápida.
- Alguns contextos podem se entrelaçar. Por exemplo, uma organização marcada como contato e pessoas da organização.
  Um exemplo mais prático: organização de um evento e outras tarefas que surgem em relação aos participantes.
  Os itens referentes à execução do evento foram marcados dentro do arquivo do evento e as tarefas referentes aos
  participantes sem relação com o evento em si foram marcadas no arquivo do participante.
  Vou ministrar dois workshops, em um vou fazer sozinha e em outro vou ficar de ajudante. As tarefas referentes ao meu
  workshop foram registradas no arquivo do evento. As tarefas referentes ao workshop onde vou ficar de ajudante foram
  registradas no arquivo da moça que vai ministrar porque eu precisava coordenar as atividades com ela.
- É necessário uma maneira rápida de ver todos os arquivos como uma lista. No Emacs eles ficam armazenados como "buffers", dá pra
  olhar cada arquivo pelo nome e listar eles com o comando C-x C-b. Fica mais conveniente fazer isso com o pacote
  ibuffer. Pra visualizar as tarefas do dia.
- Pra visualização de atividades para o dia, também é necessário uma maneira rápida. A ideia é trabalhar com os arquivos
  direto da visão do dia, só marcando os itens como feitos ou outro rótulo pertinente. O org-mode cuida disso com a
  função org-agenda.

* Ideias para melhorias
- Seria legal ter uma maneira de falar com o contato direto do arquivo de tarefas. Na motivação pra esse experimento era
  possível fazer isso. Ao escrever esse item falei "ah, você tá pensando muito em termos do Emacs!", porque o programa
  te incentiva a fazer isso, a deixar tudo dentro dele... e pra montar uma solução assim não é tão difícil.
  Os meios universais pra isso são email e telefone... mas aí depende da implementação do programa específico. Ou a
  pessoa pode ligar do próprio celular.
- Um aplicativo mobile sincronizado com a instância principal ajuda bastante quando a pessoa estiver em movimento.
