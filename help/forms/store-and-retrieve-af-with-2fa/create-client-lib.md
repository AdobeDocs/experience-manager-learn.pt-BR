---
title: Criar bibliotecas de clientes
description: Crie uma biblioteca do cliente para lidar com o evento click do botão "Salvar e sair"
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6597
thumbnail: 6597.pg
topic: Development
role: Developer
level: Intermediate
exl-id: c90eea73-bd44-40af-aa98-d766aa572415
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 6%

---

# Criar biblioteca do cliente

Criar [biblioteca cliente](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=pt-BR) que incluirá o código para chamar o método `doAjaxSubmitWithFileAttachment` do `guideBridge` API no evento de clique do botão identificado pela classe CSS **savebutton**.  Passamos os dados do formulário adaptável, `fileMap`, e o `mobileNumber` para o endpoint que está escutando em `**/bin/storeafdatawithattachments`

Depois que os dados do formulário são salvos, uma ID de aplicativo exclusiva é gerada e apresentada ao usuário em uma caixa de diálogo. Ao descartar a caixa de diálogo, o usuário é levado ao formulário, o que permite recuperar o formulário adaptável salvo usando a ID exclusiva do aplicativo.

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
> Nós usamos [biblioteca javascript de bootbox](http://bootboxjs.com/examples.html) para exibir a caixa de diálogo

As bibliotecas de clientes usadas nesta amostra podem ser [baixado aqui](assets/client-libraries.zip)

## Próximas etapas

[Verificar usuários com serviço OTP](./verify-users-with-otp.md)