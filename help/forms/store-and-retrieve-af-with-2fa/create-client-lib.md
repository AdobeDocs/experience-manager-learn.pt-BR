---
title: Criar bibliotecas de clientes
description: Criar biblioteca de clientes para manipular o evento click do botão "Salvar e sair"
feature: Formulários adaptáveis
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Desenvolvimento
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 2%

---

# Criar biblioteca do cliente

Crie [client lib](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html) que incluirá o código para chamar o método `doAjaxSubmitWithFileAttachment` da API `guideBridge` no evento click do botão identificado pela classe CSS **savebutton**.  Passamos os dados do formulário adaptável, `fileMap`, e `mobileNumber` para o ponto de extremidade que escuta em `**/bin/storeafdatawithattachments`

Depois que os dados do formulário são salvos, uma ID de aplicativo exclusiva é gerada e apresentada ao usuário em uma caixa de diálogo. Ao descartar a caixa de diálogo, o usuário é levado ao formulário, o que permite recuperar o formulário adaptável salvo usando a ID de aplicativo exclusiva.

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
> Usamos [biblioteca javascript de bootbox](http://bootboxjs.com/examples.html) para exibir a caixa de diálogo

As bibliotecas de clientes usadas neste exemplo podem ser [baixadas aqui](assets/client-libraries.zip)
