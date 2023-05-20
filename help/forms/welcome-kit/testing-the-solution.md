---
title: Teste da solução do kit de boas-vindas
description: Implantação dos ativos da solução para testar a solução
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Implantar e testar os ativos de amostra

[Instale o pacote do kit de boas-vindas](assets/welcomekit.zip). Este pacote contém o modelo de página, o componente personalizado para listar os ativos, fluxo de trabalho de amostra, modelo de email e documentos pdf de amostra para incluir no kit de boas-vindas.
[Instalar o pacote do kit de boas-vindas](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Esse pacote contém o código para criar a página e a classe java para retornar os ativos a serem exibidos na página da Web.
[Instale o exemplo de formulário adaptável](assets/account-openeing-form.zip)
Configure o Day CQ Mail Service. O fluxo de trabalho envia o link do kit de boas-vindas para o remetente do formulário que precisa do SMTP configurado corretamente.
Configure o componente de Envio de email do fluxo de trabalho de acordo com seus requisitos
[Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Insira seus detalhes, selecione um ou mais fundos mútuos e envie o formulário. Você deve receber um email com um link para o kit de boas-vindas
