---
title: Implantar os ativos localmente
description: Implantar os ativos tutoriais na instância local do AEM
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 3%

---

# Implantar no sistema

Siga as etapas listadas abaixo para que esse caso de uso funcione em sua instância local do AEM.

* [Implantar o pacote DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) contido no arquivo zip.

* Adicione a seguinte entrada no serviço Mapeador de usuários do Apache Sling Service **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** usando o [configMgr](http://localhost:4502/system/console/configMgr).

* [Implantar o conjunto de informativos](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Este pacote contém o código para listar o conteúdo da pasta e montar os informativos selecionados.

* [Importar o pacote usando o Gerenciador de pacotes](assets/newsletter.zip). Este pacote contém biblioteca do cliente e arquivos pdf de amostra para testar a solução.

* [Importar a amostra de formulário adaptável](assets/sample-adaptive-form.zip). Este formulário listará os informativos que podem ser selecionados.

* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selecione alguns informativos para baixar.Os informativos selecionados serão combinados em um pdf e retornados a você.
