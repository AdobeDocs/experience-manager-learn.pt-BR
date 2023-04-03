---
title: Criar um projeto do Asset compute para extensibilidade do Asset compute
description: Os projetos do Asset compute são projetos Node.js, gerados usando a Adobe I/O CLI, que seguem uma estrutura específica, permitindo que eles sejam implantados na Adobe I/O Runtime e integrados com AEM as a Cloud Service.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 2%

---

# Criar um projeto do Asset compute

Os projetos do Asset compute são projetos Node.js, gerados usando a Adobe I/O CLI, que seguem uma estrutura específica que permite que eles sejam implantados na Adobe I/O Runtime e integrados com AEM as a Cloud Service. Um único projeto do Asset compute pode conter um ou mais trabalhadores do Asset compute, cada um com um ponto final HTTP discreto referenciável de um Perfil de processamento as a Cloud Service AEM.

## Gerar um projeto

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Click-through da geração de um projeto de Asset compute (Sem áudio)_

Use o [Plug-in de Asset compute CLI do Adobe I/O](../set-up/development-environment.md#aio-cli) para gerar um novo projeto do Asset compute vazio.

1. Na linha de comando, navegue até a pasta para conter o projeto.
1. Na linha de comando, execute `aio app init` para iniciar a CLI de geração de projeto interativo.
   + Esse comando pode gerar um navegador da Web solicitando a autenticação para o Adobe I/O. Se isso acontecer, forneça suas credenciais do Adobe associadas ao [serviços e produtos necessários da Adobe](../set-up/accounts-and-services.md). Se não conseguir fazer logon, siga [estas instruções sobre como gerar um projeto](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Selecionar Org__
   + Selecione a Adobe Org que tem AEM as a Cloud Service, o App Builder está registrado com
1. __Selecionar projeto__
   + Localize e selecione o Projeto. Este é o [Título do projeto](../set-up/app-builder.md) criado a partir do modelo de projeto do App Builder, neste caso `WKND AEM Asset Compute`
1. __Selecionar espaço de trabalho__
   + Selecione o `Development` espaço de trabalho
1. __Quais recursos do Adobe I/O App você deseja ativar para este projeto? Selecionar componentes a serem incluídos__
   + Selecionar `Actions: Deploy runtime actions`
   + Use as teclas de setas para selecionar e espaçar para desmarcar/selecionar, e Enter para confirmar a seleção
1. __Selecione o tipo de ações a serem geradas__
   + Selecionar `DX Asset Compute Worker v1`
   + Use as teclas de setas para selecionar, espace para desmarcar/selecionar e Enter para confirmar a seleção
1. __Como deseja nomear esta ação?__
   + Usar o nome padrão `worker`.
   + Se seu projeto contém vários trabalhadores que executam diferentes cálculos de ativos, nomeie-os semanticamente

## Gerar console.json

A ferramenta de desenvolvedor requer um arquivo chamado `console.json` que contém as credenciais necessárias para se conectar ao Adobe I/O. Esse arquivo é baixado do console do Adobe I/O.

1. Abra o do trabalhador do Asset compute [Adobe I/O](https://console.adobe.io) projeto
1. Selecione o espaço de trabalho do projeto para baixar a variável `console.json` credenciais para, nesse caso, selecione `Development`
1. Vá para a raiz do projeto Adobe I/O e toque em __Baixar tudo__ no canto superior direito.
1. Um arquivo é baixado como um `.json` arquivo com o prefixo projeto e espaço de trabalho, por exemplo: `wkndAemAssetCompute-81368-Development.json`
1. Você pode
   + Renomeie o arquivo como `console.json` e mova-o para a raiz do seu projeto de trabalho do Asset compute. Esta é a abordagem neste tutorial.
   + Mova-a para uma pasta arbitrária E faça referência a essa pasta da sua `.env` arquivo com uma entrada de configuração `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. O caminho do arquivo pode ser absoluto ou relativo à raiz do seu projeto. Por exemplo:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Ou
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> OBSERVAÇÃO
> O arquivo contém credenciais. Se você armazenar o arquivo em seu projeto, adicione-o ao `.gitignore` para impedir o compartilhamento. O mesmo se aplica ao `.env` Arquivo — Esses arquivos de credenciais não devem ser compartilhados ou armazenados no Git.

## Projeto do Asset compute no GitHub

O projeto final do Asset compute está disponível no GitHub em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_O GitHub contém o estado final do projeto, totalmente preenchido com os casos de trabalho e teste, mas não contém credenciais, ou seja, `.env`, `console.json` ou `.aio`._
