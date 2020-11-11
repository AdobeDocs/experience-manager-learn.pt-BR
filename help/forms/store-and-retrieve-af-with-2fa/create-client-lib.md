---
title: Criar bibliotecas de clientes
description: Criar biblioteca de clientes para manipular o evento click do botão "Salvar e sair"
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 6%

---

# Criar biblioteca de cliente

Crie a biblioteca [do](https://docs.adobe.com/content/help/pt-BR/experience-manager-65/developing/introduction/clientlibs.translate.html) cliente que incluirá o código para chamar o método `doAjaxSubmitWithFileAttachment` da `guideBridge` API no evento click do botão identificado pelo **botão** Savebutton da classe CSS.  Enviamos os dados do formulário adaptável, `fileMap`e o `mobileNumber` para o terminal que escuta em `**/bin/storeafdatawithattachments`

Depois que os dados do formulário são salvos, uma ID de aplicativo exclusiva é gerada e apresentada ao usuário em uma caixa de diálogo. Ao descartar a caixa de diálogo, o usuário é direcionado para o formulário, o que permite recuperar o formulário adaptativo salvo usando a ID exclusiva do aplicativo.

```java
$(document).ready(function () {
  
  $(".savebutton").click(function () {
    var tel = guideBridge.resolveNode(
      "guide[0].guide1[0].guideRootPanel[0].contactInformation[0].basicContact[0].telephoneNumber[0]"
    );
    var telephoneNumber = tel.value;
    guideBridge.getFormDataString({
      success: function (data) {
        var map = guideBridge._getFileAttachmentMapForSubmit();
        guideBridge.doAjaxSubmitWithFileAttachment(
          "/bin/storeafdatawithattachments",
          {
            data: data.data,
            fileMap: map,
            mobileNumber: telephoneNumber,
          },
          {
            success: function (x) {
              bootbox.alert(
                "This is your reference number.<br>" +
                  x.data.path +
                  " <br>You will need this to retrieve your application",
                function () {
                  console.log(
                    "This was logged in the callback! After the ok button was pressed"
                  );
                  window.location.href =
                    "http://localhost:4502/content/dam/formsanddocuments/myaccountform/jcr:content?wcmmode=disabled";
                }
              );
              console.log(x.data.path);
            },
          },
          guideBridge._getFileAttachmentsList()
        );
      },
    });
  });
});
```

>[!NOTE]
> Usamos a biblioteca [javascript da](http://bootboxjs.com/examples.html) caixa de diálogo para exibir a caixa de diálogo

As bibliotecas de clientes usadas neste exemplo podem ser [baixadas aqui](assets/client-libraries.zip)
