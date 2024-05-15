---
title: Criar um projeto do Asset compute para extensibilidade do Asset compute
description: Os projetos do Asset compute são projetos Node.js, gerados usando a CLI do Adobe I/O, que seguem uma estrutura específica, permitindo que eles sejam implantados no Adobe I/O Runtime AEM e integrados ao as a Cloud Service.
jira: KT-6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
exl-id: ebb11eab-1412-4af5-bc09-e965b9116ac9
duration: 177
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---

# Criar um projeto do Asset compute

Os projetos do Asset compute são projetos Node.js, gerados usando a CLI do Adobe I/O, que seguem uma determinada estrutura que permite que eles sejam implantados no Adobe I/O Runtime AEM e integrados ao as a Cloud Service. Um único projeto do Asset compute pode conter um ou mais workers do Asset compute AEM, com cada um tendo um ponto de extremidade HTTP distinto referenciável a partir de um Perfil de processamento as a Cloud Service.

## Gerar um projeto

>[!VIDEO](https://video.tv.adobe.com/v/40197?quality=12&learn=on)

_Click-through da geração de um projeto do Asset compute (sem áudio)_

Use o [Plug-in de Asset compute CLI do Adobe I/O](../set-up/development-environment.md#aio-cli) para gerar um novo projeto do Asset compute vazio.

1. Na linha de comando, navegue até a pasta que contém o projeto.
1. Na linha de comando, execute `aio app init` para iniciar a CLI de geração de projeto interativo.
   + Esse comando pode gerar um navegador da Web solicitando autenticação para Adobe I/O. Em caso afirmativo, forneça suas credenciais de Adobe associadas à [serviços e produtos Adobe necessários](../set-up/accounts-and-services.md). Se não conseguir fazer logon, siga [estas instruções sobre como gerar um projeto](https://developer.adobe.com/app-builder/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Selecionar organização__
   + Selecione a Organização Adobe que tem AEM as a Cloud Service, a qual o Construtor de aplicativos está registrado
1. __Selecionar projeto__
   + Localize e selecione o Projeto. Este é o [Título do projeto](../set-up/app-builder.md) criado a partir do modelo de projeto App Builder, neste caso `WKND AEM Asset Compute`
1. __Selecionar espaço de trabalho__
   + Selecione o `Development` espaço de trabalho
1. __Quais recursos do aplicativo Adobe I/O você deseja habilitar para este projeto? Selecionar componentes para incluir__
   + Selecionar `Actions: Deploy runtime actions`
   + Use as teclas de setas para selecionar e espaçar para desmarcar/selecionar, e Enter para confirmar a seleção
1. __Selecionar tipos de ações a serem geradas__
   + Selecionar `DX Asset Compute Worker v1`
   + Use as teclas de setas para selecionar, o espaço para desmarcar/selecionar e Enter para confirmar a seleção
1. __Como você deseja nomear esta ação?__
   + Usar o nome padrão `worker`.
   + Se o projeto contiver vários workers que executam diferentes cálculos de ativos, nomeie-os semanticamente

## Gerar console.json

A ferramenta de desenvolvedor requer um arquivo chamado `console.json` que contém as credenciais necessárias para se conectar ao Adobe I/O. Esse arquivo é baixado do console de Adobe I/O.

1. Abra o do trabalhador do Asset compute [Adobe I/O](https://console.adobe.io) projeto
1. Selecione o espaço de trabalho do projeto para baixar `console.json` credenciais para, nesse caso, selecione `Development`
1. Vá para a raiz do projeto Adobe I/O e toque em __Baixar tudo__ no canto superior direito.
1. Um arquivo é baixado como um `.json` arquivo prefixado com o projeto e espaço de trabalho, por exemplo: `wkndAemAssetCompute-81368-Development.json`
1. Você pode:
   + Renomear o arquivo como `console.json` e mova-o para a raiz do seu projeto do Asset compute Worker. Esta é a abordagem neste tutorial.
   + Mova-a para uma pasta arbitrária E faça referência a essa pasta a partir de sua `.env` arquivo com uma entrada de configuração `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. O caminho do arquivo pode ser absoluto ou relativo à raiz do projeto. Por exemplo:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

     Ou
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`

> OBSERVAÇÃO
> O arquivo contém credenciais. Se você armazenar o arquivo no seu projeto, adicione-o ao seu `.gitignore` arquivo para impedir que seja compartilhado. O mesmo se aplica à `.env` file — Esses arquivos de credenciais não devem ser compartilhados nem armazenados no Git.

## Asset compute projeto no GitHub

O projeto final do Asset compute está disponível no GitHub em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_O GitHub contém o estado final do projeto, totalmente preenchido com os casos de trabalhador e de teste, mas não contém credenciais, ou seja, `.env`, `console.json` ou `.aio`._
