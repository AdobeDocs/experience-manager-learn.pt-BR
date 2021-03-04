---
title: Criação da sua primeira comunicação interativa para o canal de impressão
seo-title: Criação da sua primeira comunicação interativa para o canal de impressão
description: As Comunicações interativas são novas no AEM Forms 6.4. Este documento guiará você pelas etapas necessárias para criar uma comunicação interativa para o canal de impressão.
seo-description: As Comunicações interativas são novas no AEM Forms 6.4. Este documento guiará você pelas etapas necessárias para criar uma comunicação interativa para o canal de impressão.
feature: Comunicação interativa
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 3%

---


# Criação da sua primeira comunicação interativa para o canal de impressão

As Comunicações interativas são novas no AEM Forms 6.4. Este documento guiará você pelas etapas necessárias para criar uma comunicação interativa para o canal de impressão.

Visite a página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para obter um link para uma demonstração ao vivo desse recurso.

## Pré-requisitos {#prerequistes}

[Baixe e importe o ativo relacionado a este tutorial para o AEM usando o gerenciador de pacotes.](assets/gettingstartedassets.zip)Este arquivo zip contém imagens, fragmentos de documento, configuração de pasta assistida e arquivo de layout (xdp) como parte do pacote de ativos

[Baixe e descompacte este arquivo.](assets/warfileandswaggerfile.zip) Este arquivo contém o arquivo SampleRest.war que precisa ser implantado no arquivo Tomcat e swagger, que precisa ser usado para configurar sua fonte de dados.

Ao concluir este tutorial, você aprenderá o seguinte:

* Criar fonte de dados
* Criar modelo de dados do formulário
* Criar fragmentos de documento
* Configurar tabelas e gráficos
* Usar pastas vigiadas para gerar documentos no modo de lote

