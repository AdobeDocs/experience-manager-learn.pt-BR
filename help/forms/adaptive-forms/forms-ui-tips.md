---
title: Algumas dicas e truques úteis sobre a interface do usuário
description: Documento para demonstrar algumas dicas úteis sobre a interface do usuário
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# Alternar visibilidade do campo de senha

Um caso de uso comum é permitir que os preenchimentos de formulário alternem para a visibilidade do texto inserido no campo de senha.
Para concluir esse caso de uso, usei o ícone de olho do [Biblioteca impressionante de fontes](https://fontawesome.com/). O CSS necessário e o eye.svg estão incluídos na biblioteca do cliente criada para esta demonstração.


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

O CSS a seguir foi usado para posicionar o **olho** ícone dentro do campo de senha

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

## Implantar a amostra de alternância de senha

* Baixe o [biblioteca do cliente](assets/simple-ui-tips.zip)
* Baixe o [exemplo de formulário](assets/simple-ui-tricks-form.zip)
* Importe a biblioteca do cliente usando o [IU do gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe o formulário de amostra usando o [Forms e Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


