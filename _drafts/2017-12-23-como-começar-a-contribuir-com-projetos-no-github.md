---
layout: post
title:  "Como começar a contribuir com projetos no GitHub"
date: 2017-12-23
---

A ideia deste post é compartilhar como foi a minha primeira contribuição com
código para um projeto hospedado no GitHub, desde a escolha do projeto até
mandar o pull request e ter a confirmação que foi incorporado ao ramo "master"
dele.

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

Quanto a escolher o projeto, tem [este texto do Eric Raymond](http://www.catb.org/~esr/faqs/hacking-howto.html#idm45418026893328) onde ele dá
umas dicas, como escolher um projeto que você utiliza com frequência ou um
programa que você quer saber como funciona.

O GitHub tem as issues pra
cada repositório. O que precisa ser feito está lá. Dá pra abrir outras issues
também.
