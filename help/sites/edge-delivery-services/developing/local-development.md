---
title: Configurar um ambiente de desenvolvimento local para o Edge Delivery Services
description: Como configurar um ambiente de desenvolvimento local para Edge Delivery Services.
version: 6.5, Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
last-substantial-update: 2023-11-15T00:00:00Z
jira: KT-14483
thumbnail: 3425717.jpeg
duration: 196
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '87'
ht-degree: 0%

---


# Configurar um ambiente de desenvolvimento do local

Como configurar um ambiente de desenvolvimento local para desenvolvimento do Edge Delivery Services.

>[!VIDEO](https://video.tv.adobe.com/v/3425717/?learn=on)


## Etapas descritas em vídeo

1. Instalar a CLI do AEM

   ```
   $ sudo npm install -g @adobe/aem-cli
   ```

1. Altere o diretório para o diretório do projeto que é um repositório Git feito a partir do [Placa perfurada AEM](https://github.com/adobe/aem-boilerplate) modelo.

   ```
   $ git clone git@github.com:my-org/my-project.git
   $ cd my-project
   ```

1. Execute a CLI do AEM para iniciar a instância de AEM local.

   ```
   $ pwd
     /Users/my-user/my-project
   
   $ aem up
       ___    ________  ___                          __      __ 
      /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
     / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
    / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
   /_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/
   
   info: Starting AEM dev server vx.x.x
   info: Local AEM dev server up and running: http://localhost:3000/
   info: Enabled reverse proxy to https://main--my-project--my-org.hlx.page
   opening default browser: http://localhost:3000/
   ```

1. Abra http://localhost:3000/ no navegador da Web para ver o site do AEM.

