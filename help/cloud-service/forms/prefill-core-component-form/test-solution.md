---
title: Preencher previamente o formulário adaptável baseado no componente principal
description: Saiba como preencher previamente o formulário adaptável com dados
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
duration: 25
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Testar a solução

Depois que o código for implantado, crie um formulário adaptável com base nos componentes principais. Associe o formulário adaptável ao serviço de preenchimento prévio, como mostrado na captura de tela abaixo.
![serviço de preenchimento](assets/pre-fill-service.png)

Toda vez que o formulário é renderizado, o serviço de preenchimento prévio associado será executado e o formulário será preenchido com os dados retornados pelo serviço de preenchimento prévio.

Por exemplo, para preencher previamente o formulário com os dados associados ao guid **d815a2b3-5f4c-4422-8197-d0b73479bf0e**, o seguinte url é usado.
O código no serviço de preenchimento prévio extrairá o valor do parâmetro guid e buscará os dados associados ao guid da fonte de dados.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
