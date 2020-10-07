---
title: Configurar o manifest.yml de um projeto do Asset Compute
description: O arquivo manifest.yml do projeto Asset Compute descreve todos os trabalhadores neste projeto a serem implantados.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6281
thumbnail: KT-6281.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 0%

---


# Configurar o manifest.yml

O `manifest.yml`, localizado na raiz do projeto Computação de ativos, descreve todos os trabalhadores neste projeto a serem implantados.

![manifest.yml](./assets/manifest/manifest.png)

## Definição de trabalhador padrão

Os funcionários são definidos como entradas de ação do Adobe I/O Runtime em `actions`e são compostos por um conjunto de configurações.

Os funcionários que acessam outras integrações de E/S de Adobe devem definir a `annotations -> require-adobe-auth` propriedade como `true` expõe as credenciais [de E/S de Adobe do trabalhador por meio do](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#access-adobe-apis) `params.auth` objeto. Normalmente, isso é necessário quando o trabalhador chama APIs de E/S de Adobe, como as APIs Adobe Photoshop, Lightroom ou Sensei, e pode ser alternado por funcionário.

1. Abra e reveja o trabalhador gerado automaticamente `manifest.yml`. Os projetos que contêm vários funcionários da Computação de ativos devem definir uma entrada para cada trabalhador sob o `actions` storage.

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

Cada trabalhador pode configurar os [limites](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/guides/system_settings.md) para seu contexto de execução no Adobe I/O Runtime. Esses valores devem ser ajustados para fornecer o dimensionamento ideal para o trabalhador, com base no volume, taxa e tipo de ativos que ele calculará, bem como no tipo de trabalho que ele executa.

Analise as orientações [de dimensionamento de](https://docs.adobe.com/content/help/en/asset-compute/using/extend/develop-custom-application.html#sizing-workers) Adobe antes de definir os limites. Os funcionários da Asset Compute podem ficar sem memória ao processar ativos, resultando na execução do Adobe I/O Runtime sendo morta, portanto, verifique se o trabalhador foi dimensionado adequadamente para lidar com todos os ativos candidatos.

1. Adicione uma `inputs` seção à entrada de novas `wknd-asset-compute` ações. Isso permite ajustar o desempenho geral do trabalhador da Computação de ativos e a alocação de recursos.

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

## O manifest.yml finalizado

A versão final `manifest.yml` é:

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

A versão final `.manifest.yml` está disponível no site Github:

+ [aem-guides-wknd-asset-compute/manifest.yml](https://github.com/adobe/aem-guides-wknd-asset-compute/blob/master/manifest.yml)


## Validando o manifest.yml

Depois que a Computação de ativos gerada `manifest.yml` for atualizada, execute a Ferramenta de Desenvolvimento local e verifique se os start foram atualizados com êxito com as `manifest.yml` configurações atualizadas.

Para start Asset Compute Development Tool para o projeto Asset Compute:

1. Abra uma linha de comando na raiz do projeto Computação de ativos (no Código VS, isso pode ser aberto diretamente no IDE via Terminal > Novo terminal) e execute o comando:

   ```
   $ aio app run
   ```

1. A ferramenta local Asset Compute Development Tool será aberta em seu navegador padrão em __http://localhost:9000__.

   ![execução do aplicativo no rádio](assets/environment-variables/aio-app-run.png)

1. Observe a saída da linha de comando e o navegador da Web em busca de mensagens de erro quando a ferramenta de desenvolvimento for inicializada.
1. Para interromper a Ferramenta de desenvolvimento de computação de ativos, toque `Ctrl-C` na janela executada para encerrar `aio app run` o processo.

## Resolução de problemas

### Recuo YAML incorreto

+ __Erro:__ YAMLException: recuo incorreto de uma entrada de mapeamento na linha X, coluna Y:(via padrão fora do `aio app run` comando)
+ __Causa:__ Os arquivos Yaml são sensíveis ao espaço em branco, provavelmente seu recuo está incorreto.
+ __Resolução:__ Revise seu `manifest.yml` recuo e verifique se ele está correto.

### o limite memorySize está definido como muito baixo

+ __Erro:__  OpenWhiskError do Servidor de Desenvolvimento Local: PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true Retornou HTTP 400 (Solicitação incorreta) —> &quot;O conteúdo da solicitação foi malformado:falha no requisito: memória 64 MB abaixo do limite permitido de 134217728 B&quot;
+ __Causa:__ Um `memorySize` limite no manifesto foi definido abaixo do limite mínimo permitido, conforme relatado pela mensagem de erro em bytes.
+ __Resolução:__  Revise os `memorySize` limites no `manifest.yml` e verifique se eles são todos maiores que o limite mínimo permitido.