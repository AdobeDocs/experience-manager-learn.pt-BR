---
title: Implantar os ativos localmente
description: Implante os ativos do tutorial na instância local do AEM
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2023-01-04T00:00:00Z
exl-id: d23b51ba-1efb-4505-b5b3-44a02177e467
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '140'
ht-degree: 0%

---

# Implantar no sistema

Siga as etapas listadas abaixo para que esse caso de uso funcione em sua instância local do AEM.

* [Implante o Pacote DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=pt-BR) contido no arquivo zip.

* Adicione a seguinte entrada no Serviço de Mapeador de Usuários do Apache Sling Service **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** usando o [configMgr](http://localhost:4502/system/console/configMgr).

* [Implante o conjunto de boletins informativos](assets/Newsletters.core-1.0.0-SNAPSHOT.jar). Este pacote contém o código para listar o conteúdo da pasta e montar os informativos selecionados.

* [Importe o pacote usando o Gerenciador de Pacotes](assets/newsletter.zip). Este pacote contém biblioteca do cliente e arquivos pdf de amostra para testar a solução.

* [Importe o formulário adaptável de exemplo](assets/sample-adaptive-form.zip). Este formulário listará os informativos que podem ser selecionados.

* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/downloadarchivednewsletters/jcr:content?wcmmode=disabled).
Selecione alguns informativos para baixar.Os informativos selecionados serão combinados em um pdf e retornados a você.
