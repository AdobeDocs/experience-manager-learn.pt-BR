---
title: Preencher formulário adaptável usando o método setData
description: Envie o arquivo pdf carregado para extração de dados e preencha o formulário adaptável com os dados extraídos
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 1%

---

# Fazer chamada de Ajax

Quando o usuário carrega o arquivo pdf, precisamos fazer uma chamada de POST para um servlet e passar o documento PDF carregado na solicitação POST. A solicitação POST retorna um caminho para os dados exportados no repositório crx

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

O servlet montado em **_/bin/ExtractDataFromPDF_** extrai os dados do arquivo PDF e retorna o caminho do nó crx onde os dados extraídos estão armazenados.
A variável [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) é usado para definir os dados do formulário adaptável.

## Próximas etapas

[Implantar os ativos de amostra](./test-the-solution.md)

