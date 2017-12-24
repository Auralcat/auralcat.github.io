---
layout: post
title:  "Como começar a contribuir com projetos no GitHub"
date: 2017-12-24
---

# O primeiro Pull Request

Pra mim a parte mais difícil foi escolher o projeto. Antes eu olhava e não
entendia direito como começar, qual tipo de contribuição precisava fazer, achava
que tinha que ser algo enorme e cheio de passos, coisas mirabolantes, resolver
um bug gigante e crítico... e não foi bem assim. O que eu fiz foi só propor uma
função de Emacs Lisp melhor pra compilar os arquivos .pug automaticamente depois
de salvar o buffer no Emacs.

Na época (Outubro de 2017), comecei a usar o Emacs com mais frequência e comecei
a fazer desenvolvimento web com ele. Instalei o pug-mode depois de conhecer o
pré-processador `pug.js` e um dos problemas que eu tive foi compilar o arquivo
.pug pra HTML toda vez que salvava algum buffer. Não queria fazer isso na mão. A
função original proposta no [repositório do pacote no
GitHub](https://github.com/hlissner/emacs-pug-mode) era esta:

```
(add-hook 'after-save-hook #'pug-compile)
```

O problema com este pedaço de código era que a função `pug-compile` era chamada
ao salvar qualquer buffer, não apenas aqueles trabalhando com arquivos .pug,
toda hora aparecia um erro no minibuffer e isso começou a me incomodar.

Depois de fazer uma pesquisa e chegar a [este post no
Reddit](https://www.reddit.com/r/emacs/comments/741tx6/how_to_change_default_findfile_action_for/),
fiz o meu fork do projeto, clonei o repositório pro meu computador e mudei a
função no README.md pra esta aqui, depois de testar localmente:

```
(defun pug-compile-saved-file()
  (when (and (stringp buffer-file-name)
     (string-match "\\.pug\\'" buffer-file-name))
     (pug-compile)))

(add-hook 'after-save-hook 'pug-compile-saved-file)
```

Aí foi a hora de mandar o pull request. Abri no GitHub e fiz a referência ao
post do Reddit de onde adaptei a função, expliquei bem certinho o que eu fiz
e... fiquei no aguardo. Levou umas semanas pro pull request ser aceito, e
quando soube, foi bem no meio de um workshop de Git pra ajudar os participantes
a completarem os pull requests pro Hacktoberfest! Olhei e falei "GENTE,
ACEITARAM O MEU PR!!! 🙌🙌🙌".

Esse dia mudou tudo. A segunda contribuição, mais extensa, foi pra [um bot do
grupo de Linux que eu participo no
Telegram](https://github.com/helioloureiro/homemadescripts).

# Dicas

Como comentei antes, o problema maior é escolher um projeto e dentro desse
projeto escolher um ponto pra começar a trabalhar.

Quanto a escolher o projeto, tem [este texto do Eric
Raymond](http://www.catb.org/~esr/faqs/hacking-howto.html#idm45418026893328)
onde ele dá
umas dicas, como escolher um projeto que você utiliza com frequência ou um
programa que você quer saber como funciona. Não precisa ser algo muito grande ou
mesmo envolvendo código: muitos projetos precisam de ajuda com documentação,
testes, relatar bugs ao testar o software e tradução da documentação do inglês
pra outras línguas.

Alguns critérios a mais para decidir:

- Persistência
  + Alguém trabalhou nesse projeto recentemente?
  + Quando o último PR foi incluído no projeto principal?
  + Há muitos PRs antigos esperando aprovação?

- Funcionalidade:
  + Qual é o objetivo do projeto? É um jogo? Lida com música, por exemplo?
  + O que ele faz?
  + Tem alguma funcionalidade que me interessa?

- Comunidade:
  + É amigável com iniciantes?
  + Há alguém pra tirar dúvidas?
  + Incentiva a participação de pessoas diversas?
  + Tem alguém que lida com esse projeto perto de mim fisicamente pra tirar
    dúvidas? _(essa é outra questão importante, tem como se comunicar online
    sim, compartilhar tela e outras coisas, mas ajuda presencial é mais
    eficiente)_.

- Organização:
  + As issues estão bem detalhadas?
  + Tem labels pra direcionar pessoas novas?
  + Tem documentação? Ela está bem detalhada? Está tudo bem explicado?
  + Há recursos e material para pessoas iniciantes no projeto?

Feita a escolha do projeto, os próximos passos são:

- Ajeitar o ambiente de desenvolvimento: instalar dependências, configurar
  programas, etc
- Reproduzir bugs/situações mencionadas na issue que você pegou pra trabalhar
- Montar a solução pro problema
  + Nessa parte você pode ir fazendo commit pro seu fork sem se preocupar em
    mandar tudo pronto de uma vez só. Uma boa prática é fazer commits atômicos,
    isto é, fazer uma alteração bem definida no código, fazer o commit e mandar
    pro seu fork.
    Exemplo: alterar configuração de framework | commit | push | criar nova
    função | commit | push | e assim vai.
- Testar a solução do problema: aparece algum bug novo? Está funcionando mesmo?

Aí é que chega a hora de abrir o pull request.
O que mudou tudo pra mim foi encarar o pull request não como "está aqui o que eu
fiz e quero botar no projeto de vocês", mas sim como "olha, tinha esse
bug/pediram essa funcionalidade/o que estava na issue e eu fiz uma solução dessa
maneira, o que posso melhorar nela?". Você está propondo uma solução para o
problema do projeto. Se for aceita de primeira, parabéns!! Você pode pegar mais
uma issue e trabalhar em outro problema.
Agora, caso o pull request não for aceito, a pessoa vai falar algo como "tem
esse ponto aqui que precisa mudar, esse outro ali"... aí você ajeita o código
com esse feedback e manda o PR de novo, e vai repetindo até ficar de acordo.

**AVISO**: Esse processo todo do PR pode demorar algumas semanas.

# Considerações finais

O primeiro pull request é o que dá mais trabalho. Depois disso, o segundo fica
mais fácil, aí vem o terceiro, o quarto... e assim vai. :)

# Links

- [Choosing Projects](https://github.com/collections/choosing-projects) <- Cheio
de outros links úteis
- [Your First PR](https://yourfirstpr.github.io/)
- [Awesome For-Beginners (lista de projetos com issues amigáveis para
iniciantes)](https://github.com/MunGell/awesome-for-beginners)
- [Contribuindo para projetos Open Source no Github mesmo sendo
  iniciante](https://woliveiras.com.br/posts/contribuindo-para-projetos-open-source-no-github-mesmo-sendo-iniciante/)
- [How To Contribute To Open
  Source](https://opensource.guide/how-to-contribute/)
- [Guia do FreeCodeCamp para contribuir com Open Source](https://github.com/freeCodeCamp/how-to-contribute-to-open-source)
