---
title: Algumas dicas e truques úteis da interface do usuário
description: Documento para demonstrar algumas dicas úteis da interface do usuário
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: 280ea1ec8fc5da644320753958361488872359cc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 4%

---

# Alternar visibilidade do campo de senha

Um caso de uso comum é permitir que os usuários alternem para a visibilidade do texto inserido no campo de senha.
Para realizar esse caso de uso, usei o ícone de olho do [Biblioteca do Font Awesome](https://fontawesome.com/). O CSS necessário e o eye.svg são incluídos na biblioteca do cliente criada para esta demonstração.

## Amostra ao vivo

[Esse recurso está disponível para teste aqui](https://forms.enablementadobe.com/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)

## Código de exemplo

O formulário adaptável tem um campo do tipo PasswordBox chamado **ssnField**.

O código a seguir é executado quando o formulário é carregado

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

O CSS a seguir foi usado para posicionar a variável **olho** ícone dentro do campo de senha

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## Implantar a amostra de senha de alternância

* Baixe o [biblioteca do cliente](assets/simple-ui-tips.zip)
* Baixe o [formulário de amostra](assets/simple-ui-tricks-form.zip)
* Importe a biblioteca do cliente usando o [interface do usuário do gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe o formulário de amostra usando o [Forms e Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


