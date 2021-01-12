---
title: Criar um projeto de Asset compute para extensibilidade do Asset compute
description: Os projetos de asset compute são projetos Node.js, gerados com o Adobe I/O CLI, que seguem uma estrutura específica, permitindo que eles sejam implantados na Adobe I/O Runtime e integrados com AEM como Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 676d4bfceaaec3ae8d4feb9f66294ec04e1ecd2b
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# Criar um projeto de Asset compute

Os projetos de asset compute são projetos Node.js, gerados usando a CLI da Adobe I/O, que seguem uma estrutura específica que permite a implantação deles na Adobe I/O Runtime e a integração com AEM como Cloud Service. Um único projeto de Asset compute pode conter um ou mais funcionários do Asset compute, cada um com um ponto final HTTP distinto referenciável de um AEM como um Perfil de Processamento Cloud Service.

## Gerar um projeto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through de geração de um projeto de Asset compute (Sem áudio)_

Use o [plugin do Asset compute Adobe I/O CLI](../set-up/development-environment.md#aio-cli) para gerar um novo projeto de Asset compute vazio.

1. Na linha de comando, navegue até a pasta para conter o projeto.
1. Na linha de comando, execute `aio app init` para iniciar a CLI de geração de projeto interativa.
   + Isso pode gerar um navegador da Web solicitando autenticação para a Adobe I/O. Se isso acontecer, forneça suas credenciais de Adobe associadas aos [serviços e produtos Adobe necessários](../set-up/accounts-and-services.md). Se não conseguir fazer logon, siga as [instruções a seguir sobre como gerar um projeto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Selecionar organização__
   + Selecione a Organização Adobe que tem AEM como Cloud Service, o Project Firefly é registrado para
1. __Selecionar projeto__
   + Localize e selecione o Projeto. Este é o [Título do projeto](../set-up/firefly.md) criado a partir do modelo de projeto do Firefly, neste caso `WKND AEM Asset Compute`
1. __Selecionar espaço de trabalho__
   + Selecione a área de trabalho `Development`
1. __Quais recursos do aplicativo Adobe I/O você deseja ativar para este projeto? Selecionar componentes para incluir__
   + Selecionar `Actions: Deploy runtime actions`
   + Use as teclas de setas para selecionar e o espaço para desmarcar/selecionar, e Enter para confirmar a seleção
1. __Selecione o tipo de ações a serem geradas__
   + Selecionar `Adobe Asset Compute Worker`
   + Use as teclas de setas para selecionar, o espaço para desmarcar/selecionar e Enter para confirmar a seleção
1. __Como deseja nomear esta ação?__
   + Use o nome padrão `worker`.
   + Se o seu projeto contém vários trabalhadores que executam cálculos de ativos diferentes, nomeie-os semanticamente

## Gerar console.json

Na raiz do projeto de Asset compute recém-criado, execute o seguinte comando para gerar um `console.json`.

```
$ aio app use
```

Verifique se os detalhes da área de trabalho atual estão corretos, pressione `Y` ou digite para gerar um `console.json`. Se `.env` e `.aio` forem detectados como já existentes, toque em `x` para ignorar a sua criação.

Se estiver criando uma nova opção ou substituindo `.env`, adicione novamente quaisquer chaves/valores ausentes à nova `.env`:

```
## please provide the following environment variables for the Asset Compute devtool. You can use AWS or Azure, not both:
#ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=
#S3_BUCKET=
#AWS_ACCESS_KEY_ID=
#AWS_SECRET_ACCESS_KEY=
#AWS_REGION=
#AZURE_STORAGE_ACCOUNT=
#AZURE_STORAGE_KEY=
#AZURE_STORAGE_CONTAINER_NAME=
```

## Rever a anatomia do projeto

O projeto de Asset compute gerado é um projeto Node.js para um projeto especializado do Adobe Project Firefly; os seguintes projetos são idiossincráticos para o projeto do Asset compute:

+ `/actions` contém subpastas e cada subpasta define um trabalhador de Asset compute.
   + `/actions/<worker-name>/index.js` define o JavaScript executado para executar o trabalho desse trabalhador.
      + O nome da pasta `worker` é um padrão e pode ser qualquer coisa, desde que esteja registrado em `manifest.yml`.
      + É possível definir mais de uma pasta de trabalho em `/actions`, conforme necessário, no entanto, eles devem estar registrados em `manifest.yml`.
+ `/test/asset-compute` contém os conjuntos de testes para cada trabalhador. Semelhante à pasta `/actions`, `/test/asset-compute` pode conter várias subpastas, cada uma correspondente ao trabalhador testado.
   + `/test/asset-compute/worker`, representando um conjunto de testes para um trabalhador específico, contém subpastas representando um caso de teste específico, juntamente com a entrada do teste, os parâmetros e a saída esperada.
+ `/build` contém a saída, os registros e os artefatos das execuções de caso de teste de Asset compute.
+ `/manifest.yml` define quais trabalhadores do Asset compute o projeto fornece. Cada implementação de trabalhador deve ser enumerada neste arquivo para disponibilizá-las para AEM como Cloud Service.
+ `/console.json` define configurações do Adobe I/O
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
+ `/.aio` contém configurações usadas pela ferramenta CLI do rádio.
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
+ `/.env` define variáveis de ambiente em uma  `key=value` sintaxe e contém segredos que não devem ser compartilhados. Isso pode ser gerado ou Para proteger esses segredos, este arquivo NÃO deve ser verificado no Git e é ignorado pelo arquivo padrão `.gitignore` do projeto.
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
   + As variáveis definidas neste arquivo podem ser substituídas por [exportando variáveis](../deploy/runtime.md) na linha de comando.

Para obter mais detalhes sobre a revisão da estrutura do projeto, consulte a [Anatomia de um projeto do Adobe Project Firefly](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application).

A maior parte do desenvolvimento ocorre na pasta `/actions` que desenvolve implementações de funcionários e em `/test/asset-compute` que grava testes para os trabalhadores de Asset computes personalizados.

## Projeto de asset compute no Github

O projeto final do Asset compute está disponível no site Github:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contém o estado final do projeto, totalmente preenchido com os casos de trabalhador e teste, mas não contém quaisquer credenciais, ou seja, `.env`,  `console.json` ou  `.aio`._

