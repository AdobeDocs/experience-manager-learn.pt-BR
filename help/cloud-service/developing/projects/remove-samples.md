---
title: Remoção de amostras de um projeto AEM Maven
description: Saiba como limpar e remover o código de amostra de um projeto do AEM gerado pelo Arquétipo de projeto do AEM.
version: Experience Manager as a Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
jira: KT-9092
thumbnail: 337263.jpeg
exl-id: 4e10c2b7-41b6-41a0-b8d4-9207a9d3f9c8
duration: 341
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 3%

---

# Remoção de amostras de um projeto AEM Maven

Saiba como limpar e remover o código de amostra gerado de um projeto do AEM gerado pelo Arquétipo de projeto do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/337263?quality=12&learn=on)


## Recursos

+ [Arquétipo de projeto do AEM Maven](https://github.com/adobe/aem-project-archetype)

## Comandos

Os seguintes comandos podem ser executados para remover os arquivos de amostra gerados do projeto Maven do AEM:

```
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/filters \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/listeners \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/models \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/schedulers \
rm -rf core/src/main/java/com/adobe/aem/wknd/examples/core/servlets \
rm -rf core/src/test/java/com/adobe/aem/wknd/examples/core/* \
rm -rf ui.apps/src/main/content/jcr_root/apps/wknd-examples/components/helloworld \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.js \
rm -rf ui.frontend/src/main/webpack/components/_helloworld.css
```

## Edições

Remover o `<div class="helloworld" ...></div>` de:

```
ui.frontend/src/main/webpack/static/index.html
```

Remover a definição da instância do componente `<helloworld>` de:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
