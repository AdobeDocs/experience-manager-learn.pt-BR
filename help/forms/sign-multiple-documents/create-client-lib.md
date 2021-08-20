---
title: Criar biblioteca do cliente
description: Código da biblioteca do cliente para buscar o próximo formulário para assinar
feature: Formulários adaptáveis
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Desenvolvimento
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 4%

---

# Criar uma biblioteca do cliente

Crie uma Biblioteca do cliente personalizada, clientlib para resumir, para extrair os parâmetros de url que passam na chamada do GET. A chamada de GET é feita em um servlet montado em /bin/getnextformtosign que retorna o url do próximo formulário para fazer logon no pacote.

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

## Ativos

[A clientlib pode ser baixada aqui](assets/get-next-form-client-lib.zip)
