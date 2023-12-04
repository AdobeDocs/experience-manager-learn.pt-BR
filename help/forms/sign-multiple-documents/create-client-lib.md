---
title: Criar biblioteca do cliente
description: Código da biblioteca do cliente para buscar o próximo formulário a ser assinado
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 3%

---

# Criar uma biblioteca do cliente

Crie uma Biblioteca do cliente personalizada, clientlib para resumir, para extrair os parâmetros de URL que passam esses parâmetros na chamada do GET. A chamada GET é feita para um servlet montado em /bin/getnextformtosign, que retorna o url do próximo formulário para entrar no pacote.

Este é o código usado na função javascript clientlib


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[A clientlib pode ser baixada aqui](assets/get-next-form-client-lib.zip)

## Próximas etapas

[Criar modelo de formulário personalizado para este caso de uso](./create-af-template.md)