---
title: Criar um projeto do Asset Compute para a extensibilidade do Asset Compute
description: Os projetos do Asset Compute são projetos Node.js, gerados usando a CLI do Adobe I/O, que seguem uma estrutura específica, permitindo que eles sejam implantados no Adobe I/O Runtime e integrados ao AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '912'
ht-degree: 1%

---


# Criar um projeto do Asset Compute

Os projetos do Asset Compute são projetos Node.js, gerados usando a CLI do Adobe I/O, que seguem uma estrutura específica que permite a implantação deles na Adobe I/O Runtime e integrados ao AEM as a Cloud Service. Um único projeto do Asset Compute pode conter um ou mais trabalhadores do Asset Compute, cada um com um ponto final HTTP discreto referenciável de um Perfil de processamento do AEM as a Cloud Service.

## Gerar um projeto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through da geração de um projeto do Asset Compute (sem áudio)_

Use o plug-in [Adobe I/O CLI Asset Compute](../set-up/development-environment.md#aio-cli) para gerar um novo projeto vazio do Asset Compute.

1. Na linha de comando, navegue até a pasta para conter o projeto.
1. Na linha de comando, execute `aio app init` para iniciar a CLI de geração de projeto interativa.
   + Esse comando pode gerar um navegador da Web solicitando autenticação para o Adobe I/O. Se isso acontecer, forneça suas credenciais da Adobe associadas aos [serviços e produtos da Adobe necessários](../set-up/accounts-and-services.md). Se não conseguir fazer logon, siga [estas instruções sobre como gerar um projeto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Selecionar Org__
   + Selecione a Adobe Org que tem o AEM as a Cloud Service, o Project Firefly é registrado com
1. __Selecionar projeto__
   + Localize e selecione o Projeto. Este é o [Título do projeto](../set-up/firefly.md) criado a partir do modelo de projeto do Firefly, neste caso `WKND AEM Asset Compute`
1. __Selecionar espaço de trabalho__
   + Selecione o espaço de trabalho `Development`
1. __Quais recursos do aplicativo de E/S da Adobe você deseja ativar para este projeto? Selecionar componentes para incluir__
   + Selecionar `Actions: Deploy runtime actions`
   + Use as teclas de setas para selecionar e espaçar para desmarcar/selecionar, e Enter para confirmar a seleção
1. __Selecione o tipo de ações a serem geradas__
   + Selecionar `Adobe Asset Compute Worker`
   + Use as teclas de setas para selecionar, espace para desmarcar/selecionar e Enter para confirmar a seleção
1. __Como deseja nomear esta ação?__
   + Use o nome padrão `worker`.
   + Se seu projeto contém vários trabalhadores que executam diferentes cálculos de ativos, nomeie-os semanticamente

## Gerar console.json

A ferramenta de desenvolvedor requer um arquivo chamado `console.json` que contém as credenciais necessárias para se conectar ao Adobe I/O. Esse arquivo é baixado do console do Adobe I/O.

1. Abra o projeto do trabalhador do Asset Compute [Adobe I/O](https://console.adobe.io)
1. Selecione o espaço de trabalho do projeto para baixar as credenciais `console.json`, nesse caso, selecione `Development`
1. Vá para a raiz do projeto do Adobe I/O e toque em __Baixar tudo__ no canto superior direito.
1. Um arquivo é baixado como um arquivo `.json` com o prefixo do projeto e do espaço de trabalho, por exemplo: `wkndAemAssetCompute-81368-Development.json`
1. Você pode
   + Renomeie o arquivo como `config.json` e mova-o para a raiz do projeto de trabalho do Asset Compute. Esta é a abordagem neste tutorial.
   + Mova-a para uma pasta arbitrária E faça referência a essa pasta do arquivo `.env` com uma entrada de configuração `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. O caminho do arquivo pode ser absoluto ou relativo à raiz do seu projeto. Por exemplo:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Ou
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> OBSERVAÇÃO
> O arquivo contém credenciais. Se você armazenar o arquivo em seu projeto, adicione-o ao arquivo `.gitignore` para evitar que seja compartilhado. O mesmo se aplica ao arquivo `.env` — Esses arquivos de credenciais não devem ser compartilhados ou armazenados no Git.

## Revisar a anatomia do projeto

O projeto do Asset Compute gerado é um projeto Node.js para uso como um projeto especializado do Adobe Project Firefly. Os seguintes elementos estruturais são idiossincráticos para o projeto do Asset Compute:

+ `/actions` contém subpastas e cada subpasta define um trabalhador do Asset Compute.
   + `/actions/<worker-name>/index.js` define o JavaScript usado para executar o trabalho desse trabalhador.
      + O nome da pasta `worker` é um padrão e pode ser qualquer coisa, desde que esteja registrado no `manifest.yml`.
      + Mais de uma pasta de trabalho pode ser definida em `/actions` conforme necessário, no entanto, ela deve ser registrada no `manifest.yml`.
+ `/test/asset-compute` contém os conjuntos de teste para cada trabalhador. Semelhante à pasta `/actions`, `/test/asset-compute` pode conter várias subpastas, cada uma correspondente ao trabalhador testado.
   + `/test/asset-compute/worker`, representando um conjunto de testes para um trabalhador específico, contém subpastas que representam um caso de teste específico, juntamente com a entrada de teste, os parâmetros e a saída esperada.
+ `/build` contém a saída, os registros e os artefatos das execuções de caso de teste do Asset Compute.
+ `/manifest.yml` define quais trabalhadores do Asset Compute o projeto fornece. Cada implementação do trabalhador deve ser enumerada neste arquivo para torná-las disponíveis para o AEM as a Cloud Service.
+ `/console.json` define configurações do Adobe I/O
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
+ `/.aio` contém configurações usadas pela ferramenta aio CLI.
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
+ `/.env` O define variáveis de ambiente em uma  `key=value` sintaxe e contém segredos que não devem ser compartilhados. Para proteger esses segredos, esse arquivo NÃO deve ser verificado no Git e é ignorado por meio do arquivo `.gitignore` padrão do projeto.
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
   + As variáveis definidas neste arquivo podem ser substituídas por [exportando variáveis](../deploy/runtime.md) na linha de comando.

Para obter mais detalhes sobre a revisão da estrutura do projeto, consulte a [Anatomia de um projeto do Adobe Project Firefly](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

A maior parte do desenvolvimento ocorre na pasta `/actions` desenvolvendo implementações de funcionários e em `/test/asset-compute` gravando testes para os trabalhadores do Asset Compute personalizado.

## Projeto do Asset Compute no GitHub

O projeto final do Asset Compute está disponível no GitHub em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_O GitHub contém o estado final do projeto, totalmente preenchido com os casos de trabalho e teste, mas não contém credenciais, ou seja,  `.env`ou  `console.json`   `.aio`._

