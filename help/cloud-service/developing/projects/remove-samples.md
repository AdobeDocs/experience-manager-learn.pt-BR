---
title: Remoção de amostras de um projeto AEM Maven
description: Saiba como limpar e remover o código de amostra de um Projeto AEM gerado pelo Arquétipo de Projeto AEM.
version: Cloud Service
topic: Development
feature: AEM Project Archetype
role: Developer
level: Beginner
kt: 9092
thumbnail: 337263.jpeg
source-git-commit: e8b3bcaeee40b4bfd4f967f929ad664e8d168cb0
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 6%

---


# Remoção de amostras de um projeto AEM Maven

Saiba como limpar e remover o código de amostra gerado de um AEM Project gerado pelo Arquétipo de Projeto AEM.

>[!VIDEO](https://video.tv.adobe.com/v/337263/?quality=12&learn=on)


## Recursos

+ [Arquétipo de projeto AEM Maven](https://github.com/adobe/aem-project-archetype)

## Comandos

Os seguintes comandos podem ser executados para remover os arquivos de amostra gerados do Projeto Maven AEM:

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

Remova o `<div class="helloworld" ...></div>` de:

```
ui.frontend/src/main/webpack/static/index.html
```

Remova o `<helloworld>` definição de instância de componente de:

```
ui.content/src/main/content/jcr_root/content/wknd-examples/us/en/.content.xml
```
