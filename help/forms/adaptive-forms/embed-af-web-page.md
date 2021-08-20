---
title: Como incorporar formulários adaptáveis Forms/HTML5 na página da Web
description: Etapas de configuração necessárias para incorporar formulários adaptáveis Forms ou HTML5 em uma página da Web que não seja AEM.
feature: Formulários adaptáveis
type: Tutorial
version: 6.4,6.5
topic: Desenvolvimento
role: Developer
level: Beginner
kt: 8390
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---


# Como incorporar o formulário adaptável ou HTML5 na página da Web

O formulário adaptável incorporado é totalmente funcional e os usuários podem preencher e enviar o formulário sem sair da página. Ajuda o usuário a permanecer no contexto de outros elementos na página da Web e interagir simultaneamente com o formulário.

O vídeo a seguir explica as etapas necessárias para incorporar um formulário adaptável ou HTML5 na página da Web.
Consulte a [documentação](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) para obter os melhores pré-requisitos, práticas recomendadas etc.
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

Você pode baixar os arquivos de amostra usados no vídeo [daqui](assets/embedding-af-web-page.zip)

Este é o código usado para buscar o formulário adaptável e incorporar o formulário no container identificado pelo nome de classe **right**

```javascript
$(document).ready(function(){
  
	var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
	console.log(formName);
	$(".right").append(data);
      
    });
  
});
```














