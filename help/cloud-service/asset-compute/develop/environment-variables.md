---
title: Configure as variáveis de ambiente para a extensibilidade do Asset Compute
description: As variáveis de ambiente são mantidas no arquivo .env para desenvolvimento local e são usadas para fornecer credenciais do Adobe I/O e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---


# Configurar as variáveis de ambiente

![arquivo env de ponto](assets/environment-variables/dot-env-file.png)

Antes de iniciar o desenvolvimento dos trabalhadores do Asset Compute, verifique se o projeto está configurado com as informações de armazenamento em nuvem e I/O da Adobe. Essas informações são armazenadas no `.env` do projeto, que é usado somente para desenvolvimento local, e não é salvo no Git. O arquivo `.env` fornece uma maneira conveniente de expor pares de chave/valores ao ambiente de desenvolvimento local do Asset Compute . Quando [implantar](../deploy/runtime.md) o Asset Compute trabalha no Adobe I/O Runtime, o arquivo `.env` não é usado, mas um subconjunto de valores é passado por variáveis de ambiente. Outros parâmetros e segredos personalizados também podem ser armazenados no arquivo `.env` , como credenciais de desenvolvimento para serviços da Web de terceiros.

## Faça referência a `private.key`

![chave privada](assets/environment-variables/private-key.png)

Abra o arquivo `.env`, exclua o comentário da tecla `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e forneça o caminho absoluto no sistema de arquivos para o `private.key` que emparelha com o certificado público adicionado ao seu projeto do Adobe I/O FireFly.

+ Se o seu par de chaves foi gerado pelo Adobe I/O, ele foi baixado automaticamente como parte do `config.zip`.
+ Se você forneceu a chave pública para o Adobe I/O, então também deve estar na posse da chave privada correspondente.
+ Se você não tiver esses pares de chaves, poderá gerar novos pares de chaves ou fazer upload de novas chaves públicas na parte inferior de:
   [https://console.adobe.com](https://console.adobe.io) > Seu projeto do Asset Compute Firefly > Espaços de trabalho @ Development > Conta de serviço (JWT).

Lembre-se de que o arquivo `private.key` não deve ser verificado no Git, pois ele contém segredos, mas deve ser armazenado em um local seguro fora do projeto.

Por exemplo, no macOS, pode ser semelhante a:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurar credenciais do Cloud Storage

O desenvolvimento local dos trabalhadores do Asset Compute requer acesso ao [armazenamento na nuvem](../set-up/accounts-and-services.md#cloud-storage). As credenciais de armazenamento de nuvem usadas para desenvolvimento local são fornecidas no arquivo `.env` .

Este tutorial prefere o uso do Armazenamento Azure Blob, no entanto, o Amazon S3 e suas chaves correspondentes no arquivo `.env` podem ser usadas.

### Usando o Armazenamento Azure Blob

Exclua o comentário e preencha as seguintes chaves no arquivo `.env` e as preencha com os valores do armazenamento na nuvem provisionado encontrado no Portal do Azure.

![Armazenamento Azure Blob](./assets/environment-variables/azure-portal-credentials.png)

1. Valor para a chave `AZURE_STORAGE_CONTAINER_NAME`
1. Valor para a chave `AZURE_STORAGE_ACCOUNT`
1. Valor para a chave `AZURE_STORAGE_KEY`

Por exemplo, pode ser semelhante a (valores somente para ilustração):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

O arquivo resultante `.env` tem a seguinte aparência:

![Credenciais de Armazenamento do Azure Blob](assets/environment-variables/cloud-storage-credentials.png)

Se NÃO estiver usando o Armazenamento de Blobs do Microsoft Azure, remova ou deixe-os comentados (com prefixo `#`).

### Uso do armazenamento em nuvem do Amazon S3{#amazon-s3}

Se você estiver usando o armazenamento em nuvem do Amazon S3, exclua o comentário e preencha as seguintes chaves no arquivo `.env`.

Por exemplo, pode ser semelhante a (valores somente para ilustração):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validar a configuração do projeto

Depois que o projeto do Asset Compute gerado tiver sido configurado, valide a configuração antes de fazer alterações de código para garantir que os serviços de suporte sejam provisionados, nos arquivos `.env`.

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

+ [A Ferramenta de Desenvolvimento não pode ser iniciada devido à falta de private.key](../troubleshooting.md#missing-private-key)
