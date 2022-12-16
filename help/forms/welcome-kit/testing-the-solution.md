---
title: Teste da solução de kit de boas-vindas
description: Implantar os ativos da solução para testar a solução
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Implante e teste os ativos de amostra

[Instale o pacote de kit de boas-vindas](assets/welcomekit.zip). Este pacote contém o modelo de página, o componente personalizado para listar os ativos, o fluxo de trabalho de exemplo, o modelo de email e os documentos pdf de amostra que serão incluídos no kit de boas-vindas.
[Instale o pacote kit de boas-vindas](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Este pacote contém o código para criar a página e a classe java para retornar os ativos a serem exibidos na página da Web.
[Instalar o formulário adaptável de amostra](assets/account-openeing-form.zip)
Configure o Day CQ Mail Service. O workflow envia o link do kit de boas-vindas para o remetente do formulário, que precisa do SMTP configurado corretamente.
Configure o componente Enviar email do fluxo de trabalho de acordo com seus requisitos
[Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Insira seus detalhes e selecione um ou mais fundos mutualistas e envie o formulário Você deve receber um email com um link para o kit de boas-vindas


