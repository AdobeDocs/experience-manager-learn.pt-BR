---
title: Criar um projeto de Computação de ativos para extensibilidade de Computação de ativos
description: Os projetos de Computação de ativos são projetos Node.js, gerados usando a CLI de E/S do Adobe, que seguem uma estrutura específica, permitindo que eles sejam implantados na Adobe I/O Runtime e integrados ao AEM como um Cloud Service.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6269
thumbnail: 40197.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---


# Criar um projeto de Computação de ativos

Os projetos de Computação de ativos são projetos Node.js, gerados usando a CLI de E/S do Adobe, que seguem uma estrutura específica que permite que eles sejam implantados na Adobe I/O Runtime e integrados ao AEM como um Cloud Service. Um único projeto de Computação de ativos pode conter um ou mais funcionários da Computação de ativos, cada um com um ponto final HTTP distinto referenciável de um AEM como um Perfil de Processamento de Cloud Service.

## Gerar um projeto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through de geração de um projeto Asset Compute (Sem áudio)_


Use o plug-in [](../set-up/development-environment.md#aio-cli) Adobe I/O CLI Asset Compute para gerar um novo projeto vazio da Asset Compute.

1. Na linha de comando, navegue até a pasta para conter o projeto.
1. Na linha de comando, execute `aio app init` para iniciar a CLI de geração de projeto interativa.
   + Isso pode gerar um navegador da Web solicitando autenticação para E/S de Adobe. Se isso acontecer, forneça suas credenciais de Adobe associadas aos serviços e produtos [de Adobe](../set-up/accounts-and-services.md)necessários. Se não conseguir fazer logon, siga [estas instruções sobre como gerar um projeto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Selecionar organização__
   + Selecione a Organização Adobe que tem AEM como Cloud Service, o Project Firefly é registrado para
1. __Selecionar projeto__
   + Localize e selecione o Projeto. Este é o título [do](../set-up/firefly.md) Projeto criado a partir do modelo de projeto do Firefly, neste caso `WKND AEM Asset Compute`
1. __Selecionar espaço de trabalho__
   + Selecionar a `Development` área de trabalho
1. __Quais recursos do aplicativo de E/S de Adobe você deseja ativar para este projeto? Selecionar componentes a serem incluídos__
   + Selecionar `Actions: Deploy runtime actions`
   + Use as teclas de setas para selecionar e o espaço para desmarcar/selecionar, e Enter para confirmar a seleção
1. __Selecione o tipo de ações a serem geradas__
   + Selecionar `Adobe Asset Compute Worker`
   + Use as teclas de setas para selecionar, o espaço para desmarcar/selecionar e Enter para confirmar a seleção
1. __Como deseja nomear esta ação?__
   + Use o nome padrão `worker`.
   + Se o seu projeto contém vários trabalhadores que executam cálculos de ativos diferentes, nomeie-os semanticamente

## Gerar console.json

Na raiz do projeto recém-criado do Asset Compute, execute o seguinte comando para gerar um `console.json`.

```
$ aio app use
```

Verifique se os detalhes da área de trabalho atual estão corretos e se estão bonitos `Y` ou digitados para gerar um `console.json`. Se `.env` e `.aio` forem detectados como já existentes, toque em `x` para ignorar sua criação.

## Rever a anatomia do projeto

O projeto Asset Compute gerado é um projeto Node.js para um projeto especializado do Adobe Project Firefly; os seguintes projetos são idiossincráticos para o projeto Asset Compute:

+ `/actions` contém subpastas e cada subpasta define um funcionário do Asset Compute.
   + `/actions/<worker-name>/index.js` define o JavaScript executado para executar o trabalho desse trabalhador.
      + O nome da pasta `worker` é um padrão e pode ser qualquer coisa, desde que esteja registrado no `manifest.yml`.
      + Mais de uma pasta de trabalho pode ser definida conforme `/actions` necessário, no entanto, ela deve estar registrada no `manifest.yml`.
+ `/test/asset-compute` contém os conjuntos de testes para cada trabalhador. Semelhante à `/actions` pasta, `/test/asset-compute` pode conter várias subpastas, cada uma correspondente ao trabalho testado.
   + `/test/asset-compute/worker`, representando um conjunto de testes para um trabalhador específico, contém subpastas representando um caso de teste específico, juntamente com a entrada do teste, os parâmetros e a saída esperada.
+ `/build` contém a saída, os registros e os artefatos das execuções de caso de teste do Asset Compute.
+ `/manifest.yml` define o que a Computação de ativos fornece aos trabalhadores do projeto. Cada implementação de trabalhador deve ser enumerada neste arquivo para disponibilizá-las para AEM como Cloud Service.
+ `/console.json` define configurações de E/S de Adobe
   + Esse arquivo pode ser gerado/atualizado usando o `aio app use` comando.
+ `/.aio` contém configurações usadas pela ferramenta CLI do rádio.
   + Esse arquivo pode ser gerado/atualizado usando o `aio app use` comando.
+ `/.env` define variáveis de ambiente em uma `key=value` sintaxe e contém segredos que não devem ser compartilhados. Isso pode ser gerado ou para proteger esses segredos, este arquivo NÃO deve ser verificado no Git e é ignorado pelo arquivo padrão do `.gitignore` projeto.
   + Esse arquivo pode ser gerado/atualizado usando o `aio app use` comando.
   + As variáveis definidas neste arquivo podem ser substituídas pela [exportação de variáveis](../deploy/runtime.md) na linha de comando.

Para obter mais detalhes sobre a revisão da estrutura do projeto, reveja a [Anatomia de um projeto](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application)do Adobe Project Firefly.

A maior parte do desenvolvimento ocorre na `/actions` pasta que desenvolve implementações de funcionários e em testes `/test/asset-compute` escritos para os funcionários personalizados da Asset Compute.

## Projeto Asset Compute no Github

O projeto final Asset Compute está disponível no Github em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contém o estado final do projeto, totalmente preenchido com os casos de trabalhador e teste, mas não contém quaisquer credenciais, ou seja, `.env`, `console.json` ou `.aio`._

