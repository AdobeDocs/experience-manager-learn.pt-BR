---
title: Configure as variáveis de ambiente para extensibilidade de Asset compute
description: As variáveis de ambiente são mantidas no arquivo .env para desenvolvimento local e são usadas para fornecer credenciais do Adobe I/O e credenciais de armazenamento na nuvem necessárias para o desenvolvimento local.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '588'
ht-degree: 0%

---


# Configure as variáveis de ambiente

![arquivo dot env](assets/environment-variables/dot-env-file.png)

Antes de iniciar o desenvolvimento de funcionários do Asset compute, verifique se o projeto está configurado com informações do Adobe I/O e do armazenamento em nuvem. Essas informações são armazenadas no `.env` do projeto, que é usado apenas para desenvolvimento local, e não é salvo no Git. O arquivo `.env` oferece uma maneira conveniente de expor pares de valores/chaves ao ambiente de desenvolvimento local do Asset compute. Quando [implantar](../deploy/runtime.md) trabalhadores de Asset compute no Adobe I/O Runtime, o arquivo `.env` não será usado, mas um subconjunto de valores será passado pelas variáveis de ambiente. Outros parâmetros e segredos personalizados também podem ser armazenados no arquivo `.env`, como credenciais de desenvolvimento para serviços da Web de terceiros.

## Consulte `private.key`

![chave privada](assets/environment-variables/private-key.png)

Abra o arquivo `.env`, exclua as barras de comentário `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e forneça o caminho absoluto do sistema de arquivos para `private.key` que emparelhe com o certificado público adicionado ao seu projeto Adobe I/O FireFly.

+ Se o par de chaves foi gerado pela Adobe I/O, ele foi baixado automaticamente como parte do `config.zip`.
+ Se você forneceu a chave pública para a Adobe I/O, você também deve estar na posse da chave privada correspondente.
+ Se você não tiver esses pares de chaves, poderá gerar novos pares de chaves ou fazer upload de novas chaves públicas na parte inferior de:
   [https://console.adobe.com](https://console.adobe.io) > Projeto Firefly do seu Asset compute > Espaços de trabalho em Desenvolvimento > Conta de serviço (JWT).

Lembre-se de que o arquivo `private.key` não deve ser verificado no Git, pois ele contém segredos, mas deve ser armazenado em um local seguro fora do projeto.

Por exemplo, no macOS, isso pode parecer com:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurar credenciais de Armazenamento da Cloud

O desenvolvimento local dos trabalhadores do Asset compute requer acesso a [armazenamento em nuvem](../set-up/accounts-and-services.md#cloud-storage). As credenciais de armazenamento de nuvem usadas para desenvolvimento local são fornecidas no arquivo `.env`.

Este tutorial prefere o uso do Armazenamento Blob do Azure, no entanto, o Amazon S3 e suas chaves correspondentes no arquivo `.env` podem ser usadas.

### Usando o Armazenamento Blob do Azure

Exclua os comentários e preencha as seguintes chaves no arquivo `.env` e preencha-as com os valores do armazenamento em nuvem provisionado encontrado no Portal do Azure.

![Armazenamento Blob do Azure](./assets/environment-variables/azure-portal-credentials.png)

1. Valor para a chave `AZURE_STORAGE_CONTAINER_NAME`
1. Valor para a chave `AZURE_STORAGE_ACCOUNT`
1. Valor para a chave `AZURE_STORAGE_KEY`

Por exemplo, isso pode parecer com (valores somente para ilustração):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

O arquivo resultante `.env` é exibido da seguinte maneira:

![Credenciais do Armazenamento Blob do Azure](assets/environment-variables/cloud-storage-credentials.png)

Se NÃO estiver a utilizar o Armazenamento Blob do Microsoft Azure, remova ou deixe estes comentários fora (prefixando com `#`).

### Uso do armazenamento em nuvem Amazon S3{#amazon-s3}

Se você estiver usando o armazenamento em nuvem do Amazon S3, exclua o comentário e preencha as seguintes chaves no arquivo `.env`.

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

Depois que o projeto de Asset compute gerado tiver sido configurado, valide a configuração antes de fazer alterações de código para garantir que os serviços de suporte sejam provisionados, nos arquivos `.env`.

Para a ferramenta de desenvolvimento de Asset computes de start para o projeto de Asset compute:

1. Abra uma linha de comando na raiz do projeto do Asset compute (no Código VS, isso pode ser aberto diretamente no IDE via Terminal > Novo terminal) e execute o comando:

   ```
   $ aio app run
   ```

1. A Ferramenta de Desenvolvimento de Asset computes local será aberta no navegador da Web padrão em __http://localhost:9000__.

   ![execução do aplicativo no rádio](assets/environment-variables/aio-app-run.png)

1. Observe a saída da linha de comando e o navegador da Web em busca de mensagens de erro quando a ferramenta de desenvolvimento for inicializada.
1. Para interromper a ferramenta de desenvolvimento de Asset computes, toque em `Ctrl-C` na janela que executou `aio app run` para encerrar o processo.

## Resolução de problemas

+ [A Ferramenta de Desenvolvimento não pode start devido à falta de private.key](../troubleshooting.md#missing-private-key)
