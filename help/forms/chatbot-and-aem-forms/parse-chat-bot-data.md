---
title: Uso do AEM Forms com Chatbot
description: Analisar dados de ChatBot
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# Analisar dados de ChatBot

Um [webhook ChatBot](https://www.chatbot.com/help/webhooks/what-are-webhooks/) foi usado para enviar os dados do ChatBot para um servlet AEM.
Os dados capturados no ChatBot estão no formato JSON com os dados inseridos pelo usuário no objeto de atributos, conforme mostrado abaixo
![dados de chatbot](assets/chat-bot-data.png)

Para mesclar os dados com o modelo XDP, precisamos criar o seguinte XML. Observe o elemento raiz do xml, que deve corresponder ao elemento raiz do modelo XDP para que os dados sejam mesclados com sucesso.


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![modelo-xdp](assets/xdp-template.png)

## Próximas etapas

[Mesclar dados com modelo XDP](./merge-data-with-template.md)
