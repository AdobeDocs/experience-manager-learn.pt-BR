---
title: Configurar o manifest.yml de um projeto do Asset Compute
description: O arquivo manifest.yml do projeto do Asset Compute descreve todos os funcionários neste projeto que serão implantados.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6281
thumbnail: KT-6281.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 766bfaff-ade0-41c8-a395-e79dfb4b3d76
duration: 115
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 0%

---

# Configurar o manifest.yml

O `manifest.yml`, localizado na raiz do projeto do Asset Compute, descreve todos os trabalhadores deste projeto que serão implantados.

![manifest.yml](./assets/manifest/manifest.png)

## Definição de trabalhador padrão

Os trabalhadores são definidos como entradas de ação do Adobe I/O Runtime em `actions` e são compostos de um conjunto de configurações.

Os trabalhadores que acessam outras integrações do Adobe I/O devem definir a propriedade `annotations -> require-adobe-auth` como `true`, pois este [expõe as credenciais do Adobe I/O do trabalhador](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=pt-BR#access-adobe-apis) por meio do objeto `params.auth`. Normalmente, isso é necessário quando o trabalhador chama APIs do Adobe I/O, como a Adobe Photoshop ou APIs do Lightroom, e pode ser alternado por trabalhador.

1. Abra e revise o trabalhador gerado automaticamente `manifest.yml`. Projetos que contêm vários trabalhadores do Asset Compute devem definir uma entrada para cada trabalhador na matriz `actions`.

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
          require-adobe-auth: true # set to true, to pass through Adobe I/O access token/client id via params.auth in the worker, typically required when the worker calls out to Adobe I/O APIs such as the Adobe Photoshop, or Lightroom.
```

## Definir limites

Cada trabalhador pode configurar os [limites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) para seu contexto de execução no Adobe I/O Runtime. Esses valores devem ser ajustados para fornecer o dimensionamento ideal para o trabalhador, com base no volume, na taxa e no tipo de ativos que ele calculará, bem como no tipo de trabalho que realizará.

Revise a [orientação de dimensionamento do Adobe](https://experienceleague.adobe.com/docs/asset-compute/using/extend/develop-custom-application.html?lang=pt-BR#sizing-workers) antes de definir limites. Os trabalhadores do Asset Compute podem ficar sem memória ao processar ativos, resultando na morte da execução do Adobe I/O Runtime. Portanto, verifique se o trabalhador está dimensionado adequadamente para lidar com todos os ativos candidatos.

1. Adicione uma seção `inputs` à nova entrada de ações `wknd-asset-compute`. Isso permite o ajuste do desempenho geral e da alocação de recursos do trabalhador do Asset Compute.

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


## Validar o manifest.yml

Depois que o Asset Compute gerado `manifest.yml` for atualizado, execute a Ferramenta de Desenvolvimento local e verifique se ele é iniciado com êxito com as configurações `manifest.yml` atualizadas.

Para iniciar a Ferramenta de desenvolvimento do Asset Compute para o projeto Asset Compute:

1. Abra uma linha de comando na raiz do projeto Asset Compute (no Código VS, isso pode ser aberto diretamente no IDE via Terminal > Novo terminal) e execute o comando:

   ```
   $ aio app run
   ```

1. A Ferramenta de Desenvolvimento do Asset Compute local será aberta no navegador da Web padrão em __http://localhost :9000__.

   ![aio aplicativo executado](assets/environment-variables/aio-app-run.png)

1. Observe a saída da linha de comando e o navegador da Web em busca de mensagens de erro enquanto a Ferramenta de desenvolvimento é inicializada.
1. Para interromper a Ferramenta de Desenvolvimento do Asset Compute, toque em `Ctrl-C` na janela que executou `aio app run` para encerrar o processo.

## Resolução de problemas

+ [Recuo YAML incorreto](../troubleshooting.md#incorrect-yaml-indentation)
+ [O limite memorySize está definido como muito baixo](../troubleshooting.md#memorysize-limit-is-set-too-low)
