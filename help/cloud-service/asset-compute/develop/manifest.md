---
title: Configurar o manifest.yml de um projeto do Asset Compute
description: O arquivo manifest.yml do projeto do Asset Compute descreve todos os trabalhadores neste projeto que serão implantados.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 0%

---


# Configurar o manifest.yml

O `manifest.yml`, localizado na raiz do projeto Asset Compute, descreve todos os trabalhadores neste projeto a serem implantados.

![manifest.yml](./assets/manifest/manifest.png)

## Definição de trabalhador padrão

Os trabalhadores são definidos como entradas de ação do Adobe I/O Runtime em `actions` e são compostos de um conjunto de configurações.

Os funcionários que acessam outras integrações do Adobe I/O devem definir a propriedade `annotations -> require-adobe-auth` como `true`, pois essa opção [expõe as credenciais do Adobe I/O do trabalhador](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) por meio do objeto `params.auth` . Normalmente, isso é necessário quando o trabalhador chama APIs do Adobe I/O, como as APIs do Adobe Photoshop, Lightroom ou Sensei, e pode ser alternado por trabalhador.

1. Abra e revise o trabalhador gerado automaticamente `manifest.yml`. Os projetos que contêm vários trabalhadores do Asset Compute devem definir uma entrada para cada trabalhador na matriz `actions`.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: # the array of workers, since we have a single worker there is only one entry beneath actions
      worker: # the auto-generated worker definition
        function: actions/worker/index.js # the entry point to the worker 
        web: 'yes'  # as our worker is invoked over HTTP from AEM Author service
        runtime: 'nodejs:12' # the target nodejs runtime (only 10 and 12 are supported)
        limits:
          concurrency: 10
        annotations:
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, Lightroom or Sensei APIs.
```

## Definir limites

Cada trabalhador pode configurar os [limites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) para seu contexto de execução no Adobe I/O Runtime. Esses valores devem ser ajustados para fornecer o dimensionamento ideal para o trabalhador, com base no volume, na taxa e no tipo de ativos que ele calculará, bem como no tipo de trabalho que ele executa.

Revise [Orientação sobre dimensionamento da Adobe](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) antes de definir limites. Os trabalhadores do Asset Compute podem ficar sem memória ao processar ativos, resultando no cancelamento da execução do Adobe I/O Runtime, portanto, verifique se o trabalhador está dimensionado adequadamente para lidar com todos os ativos candidatos.

1. Adicione uma seção `inputs` à nova entrada de ações `wknd-asset-compute`. Isso permite ajustar o desempenho geral do trabalhador do Asset Compute e a alocação de recursos.

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits: # Allows for the tuning of the worker's performance
          timeout: 60000 # timeout in milliseconds (1 minute)
          memorySize: 512 # memory allocated in MB; if the worker offloads heavy computational work to other Web services this number can be reduced
          concurrency: 10 # adjust based on expected concurrent processing and timeout 
        annotations:
          require-adobe-auth: true
           
```

## O manifest.yml concluído

O `manifest.yml` final tem a seguinte aparência:

```yml
packages:
  __APP_PACKAGE__:
    license: Apache-2.0
    actions: 
      worker:
        function: actions/worker/index.js 
        web: 'yes' 
        runtime: 'nodejs:12'
        limits:
          timeout: 60000 # in ms
          memorySize: 512 # in MB
          concurrency: 10 
        annotations:
          require-adobe-auth: true
```

## manifest.yml no Github

O `.manifest.yml` final está disponível no Github em:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validando o manifest.yml

Depois que o Asset Compute gerado `manifest.yml` for atualizado, execute a Ferramenta de Desenvolvimento local e garanta que seja iniciado com êxito com as configurações atualizadas `manifest.yml`.

Para iniciar a Ferramenta de desenvolvimento Asset Compute para o projeto do Asset Compute:

1. Abra uma linha de comando na raiz do projeto do Asset Compute (no Código VS, isso pode ser aberto diretamente no IDE via Terminal > Novo terminal) e execute o comando:

   ```
   $ aio app run
   ```

1. A Ferramenta de desenvolvimento Asset Compute local será aberta em seu navegador da Web padrão em __http://localhost:9000__.

   ![execução do aplicativo aio](assets/environment-variables/aio-app-run.png)

1. Observe as mensagens de erro na saída da linha de comando e no navegador da Web, conforme a Ferramenta de desenvolvimento é inicializada.
1. Para interromper a Ferramenta de desenvolvimento do Asset Compute, toque em `Ctrl-C` na janela que executou `aio app run` para encerrar o processo.

## Resolução de problemas

+ [Recuo YAML incorreto](../troubleshooting.md#incorrect-yaml-indentation)
+ [o limite memorySize está definido como muito baixo](../troubleshooting.md#memorysize-limit-is-set-too-low)
