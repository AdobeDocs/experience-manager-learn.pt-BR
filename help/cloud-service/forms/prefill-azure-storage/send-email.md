---
title: Enviar email usando SendGrid
description: Acionar um email com um link para o formulário salvo
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-8474
duration: 29
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Enviar e-mail

Depois que os dados do formulário forem salvos no Armazenamento de blobs do Azure, um email com um link para o formulário salvo será enviado ao usuário. Esse email é enviado usando a API REST do SendGrid.

O arquivo Swagger, o modelo de dados de formulário e a configuração dos serviços em nuvem necessários para enviar emails são fornecidos como parte dos ativos do artigo.

Será necessário criar uma conta SendGrid, modelo dinâmico que pode assimilar dados capturados no formulário adaptável.


Veja a seguir o trecho de código html usado no modelo dinâmico. O valor do parâmetro blobID é transmitido usando o serviço de chamada do modelo de dados de formulário.

```html
You can  <a href='https://forms.enablementadobe.com/content/dam/formsanddocuments/azureportalstorage/creditcardapplication/jcr:content?wcmmode=disabled&ampguid={{blobID}}'>access your application here</a> and complete it.
```


