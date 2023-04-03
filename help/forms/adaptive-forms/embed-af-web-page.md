---
title: Incorporação de formulários adaptáveis Forms/HTML5 na página da Web
description: Etapas de configuração necessárias para incorporar formulários adaptáveis Forms ou HTML5 em uma página da Web que não seja AEM.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%

---

# Incorporação do formulário adaptável ou HTML5 na página da Web

O formulário adaptável incorporado é totalmente funcional e os usuários podem preencher e enviar o formulário sem sair da página. Ajuda o usuário a permanecer no contexto de outros elementos na página da Web e interagir simultaneamente com o formulário.

O vídeo a seguir explica as etapas necessárias para incorporar um formulário adaptável ou HTML5 na página da Web.
Consulte a [documentação](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) para obter os melhores pré-requisitos, práticas recomendadas etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

Você pode baixar os arquivos de amostra usados no vídeo [daqui](assets/embedding-af-web-page.zip)

Este é o código usado para buscar o formulário adaptável e incorporar o formulário no container identificado pelo nome da classe **right**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
