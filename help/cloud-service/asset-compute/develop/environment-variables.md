---
title: Configure as variáveis de ambiente para a extensibilidade do Asset Compute
description: As variáveis de ambiente são mantidas no arquivo .env para desenvolvimento local e são usadas para fornecer credenciais de E/S do Adobe e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '642'
ht-degree: 0%

---


# Configure as variáveis de ambiente

![arquivo dot env](assets/environment-variables/dot-env-file.png)

Antes de iniciar o desenvolvimento de funcionários da Asset Compute, verifique se o projeto está configurado com as informações de Adobe I/O e armazenamento em nuvem. Essas informações são armazenadas no projeto, `.env` que é usado apenas para desenvolvimento local, e não no Git. O `.env` arquivo fornece uma maneira conveniente de expor pares de valores/chaves ao ambiente de desenvolvimento local do Asset Compute. Ao [implantar](../deploy/runtime.md) os funcionários da Asset Compute na Adobe I/O Runtime, o `.env` arquivo não é usado, mas um subconjunto de valores é passado pelas variáveis do ambiente. Outros parâmetros e segredos personalizados também podem ser armazenados no `.env` arquivo, como credenciais de desenvolvimento para serviços da Web de terceiros.

## Consulte a `private.key`

![chave privada](assets/environment-variables/private-key.png)

Abra o `.env` arquivo, exclua o comentário da `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` chave e forneça o caminho absoluto do seu sistema de arquivos para o `private.key` que emparelha com o certificado público adicionado ao seu projeto de E/S FireFly do Adobe.

+ Se o seu par de chaves foi gerado pela E/S do Adobe, ele foi baixado automaticamente como parte do `config.zip`.
+ Se você forneceu a chave pública para E/S de Adobe, então você também deve estar na posse da chave privada correspondente.
+ Se você não tiver esses pares de chaves, poderá gerar novos pares de chaves ou fazer upload de novas chaves públicas na parte inferior de:
   [https://console.adobe.com](https://console.adobe.io) > Projeto Firefly do Asset Compute > Workspaces @ Desenvolvimento > Conta de Serviço (JWT).

Lembre-se de que o `private.key` arquivo não deve ser verificado no Git, pois ele contém segredos, mas deve ser armazenado em um local seguro fora do projeto.

Por exemplo, no macOS, isso pode parecer com:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurar credenciais de Armazenamento da Cloud

O desenvolvimento local dos funcionários da Asset Compute requer acesso ao armazenamento [em](../set-up/accounts-and-services.md#cloud-storage)nuvem. As credenciais de armazenamento de nuvem usadas para desenvolvimento local são fornecidas no `.env` arquivo.

Este tutorial prefere o uso do Armazenamento Blob do Azure, no entanto, o Amazon S3 e suas chaves correspondentes no `.env` arquivo podem ser usadas.

### Usando o Armazenamento Blob do Azure

Exclua os comentários e preencha as seguintes chaves no `.env` arquivo e preencha-as com os valores do armazenamento em nuvem provisionado encontrados no Portal do Azure.

![Armazenamento Blob do Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valor da `AZURE_STORAGE_CONTAINER_NAME` chave
1. Valor da `AZURE_STORAGE_ACCOUNT` chave
1. Valor da `AZURE_STORAGE_KEY` chave

Por exemplo, isso pode parecer com (valores somente para ilustração):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

O `.env` arquivo resultante é exibido da seguinte maneira:

![Credenciais do Armazenamento Blob do Azure](assets/environment-variables/cloud-storage-credentials.png)

Se NÃO estiver a utilizar o Armazenamento Blob do Microsoft Azure, remova ou deixe estes comentários fora (prefixando com `#`).

### Uso do armazenamento em nuvem Amazon S3{#amazon-s3}

Se você estiver usando o armazenamento em nuvem do Amazon S3, exclua o comentário e preencha as seguintes chaves no `.env` arquivo.

Por exemplo, isso pode parecer com (valores somente para ilustração):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validação da configuração do projeto

Depois que o projeto do Asset Compute gerado tiver sido configurado, valide a configuração antes de fazer alterações de código para garantir que os serviços de suporte sejam provisionados, nos `.env` arquivos.

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

### O Asset Compute Local Development Tools não pode ser start devido à falta de private.key

+ __Erro:__ Erro do Servidor de Desenvolvimento Local: Faltando arquivos necessários em validatePrivateKeyFile.... (por padrão fora do `aio app run` comando)
+ __Causa:__ O `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no `.env` arquivo não aponta para `private.key` ou não `private.key` pode ser lido pelo usuário atual.
+ __Resolução:__ Revise o `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` valor no `.env` arquivo e verifique se ele contém o caminho completo e absoluto para o `private.key` em seu sistema de arquivos.
