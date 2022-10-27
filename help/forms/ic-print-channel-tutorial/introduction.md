---
title: Criação da sua primeira comunicação interativa para o canal de impressão
seo-title: Creating your first interactive communication for the print channel
description: As Comunicações interativas são novas no AEM Forms 6.4. Este documento guiará você pelas etapas necessárias para criar uma comunicação interativa para o canal de impressão.
seo-description: Interactive Communications is new to AEM Forms 6.4. This document will walk you through the steps needed to create an interactive communication for the print channel.
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 1949aeff-ae56-4abd-8e63-23c2fb4859f2
last-substantial-update: 2019-08-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 2%

---

# Criação da sua primeira comunicação interativa para o canal de impressão

As Comunicações interativas são novas no AEM Forms 6.4. Este documento guiará você pelas etapas necessárias para criar uma comunicação interativa para o canal de impressão.

## Pré-requisitos {#prerequistes}

[Baixe e importe o ativo relacionado a este tutorial no AEM usando o gerenciador de pacotes.](assets/gettingstartedassets.zip)Este arquivo zip contém imagens, fragmentos de documento, configuração de pasta assistida e arquivo de layout (xdp) como parte do pacote de ativos

[Baixe e descompacte este arquivo.](assets/warfileandswaggerfile.zip) Este arquivo contém o arquivo SampleRest.war que precisa ser implantado no arquivo Tomcat e swagger, que precisa ser usado para configurar sua fonte de dados.

Ao concluir este tutorial, você aprenderá o seguinte:

* Criar fonte de dados
* Criar modelo de dados do formulário
* Criar fragmentos de documento
* Configurar tabelas e gráficos
* Usar pastas vigiadas para gerar documentos no modo de lote
