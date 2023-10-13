---
title: Extrair resposta do envio personalizado
description: Extrair resposta personalizada no envio bem-sucedido do formulário
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 0%

---

# Extrair o objeto json da resposta

Ao enviar um formulário adaptável headless para um manipulador de envio personalizado, os dados retornados pelo manipulador de envio podem ser extraídos e exibidos no aplicativo react. O trecho de código a seguir mostra

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
