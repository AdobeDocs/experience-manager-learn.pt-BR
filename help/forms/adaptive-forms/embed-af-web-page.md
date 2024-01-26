---
title: Incorporação de formulários Forms/HTML5 adaptáveis na página da Web
description: Etapas de configuração necessárias para incorporar formulários do Adaptive Forms ou HTML5 em uma página da Web que não seja do AEM.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 404
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Incorporação do formulário adaptável ou do formulário HTML5 na página da Web

O formulário adaptável incorporado é totalmente funcional e os usuários podem preencher e enviar o formulário sem sair da página. Ele ajuda o usuário a permanecer no contexto de outros elementos na página da Web e interagir simultaneamente com o formulário.

O vídeo a seguir explica as etapas necessárias para incorporar um formulário adaptável ou HTML5 em uma página da Web.
Consulte a [documentação](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html) para obter os pré-requisitos, as práticas recomendadas etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

Você pode baixar os arquivos de amostra usados no vídeo [daqui](assets/embedding-af-web-page.zip)

Este é o código usado para buscar o formulário adaptável e incorporar o formulário no contêiner identificado pelo nome da classe **direita**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
