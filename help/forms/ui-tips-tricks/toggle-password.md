---
title: Algumas dicas e truques úteis sobre a interface do usuário
description: Documento para demonstrar algumas dicas úteis sobre a interface do usuário
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
exl-id: 13b9cd28-2797-4da9-a300-218e208cd21b
last-substantial-update: 2019-07-07T00:00:00Z
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---

# Alternar visibilidade do campo de senha

Um caso de uso comum é permitir que os preenchimentos de formulário alternem para a visibilidade do texto inserido no campo de senha.
Para concluir este caso de uso, usei o ícone de olho da [Biblioteca impressionante de fontes](https://fontawesome.com/). O CSS necessário e o eye.svg estão incluídos na biblioteca do cliente criada para esta demonstração.



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

O CSS a seguir foi usado para posicionar o ícone **olho** dentro do campo de senha

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

* Baixe a [biblioteca do cliente](assets/simple-ui-tips.zip)
* Baixe o [formulário de exemplo](assets/simple-ui-tricks-form.zip)
* Importar a biblioteca do cliente usando a [interface do usuário do gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Importar o formulário de exemplo usando o [Forms e o Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)
