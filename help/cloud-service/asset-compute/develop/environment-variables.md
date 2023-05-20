---
title: Configurar as variáveis de ambiente para extensibilidade do Asset compute
description: As variáveis de ambiente são mantidas no arquivo .env para desenvolvimento local e são usadas para fornecer credenciais de Adobe I/O e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '590'
ht-degree: 0%

---

# Configurar as variáveis de ambiente

![arquivo dot env](assets/environment-variables/dot-env-file.png)

Antes de iniciar o desenvolvimento de trabalhadores do Asset compute, verifique se o projeto está configurado com informações de armazenamento em Adobe I/O e nuvem. Essas informações são armazenadas no `.env`  que é usado apenas para desenvolvimento local e não é salvo no Git. A variável `.env` O arquivo fornece uma maneira conveniente de expor pares de chave/valores ao ambiente de desenvolvimento local do Asset compute. Quando [implantação](../deploy/runtime.md) Os trabalhadores do Asset compute para o Adobe I/O Runtime, a `.env` O arquivo não é usado, mas um subconjunto de valores é transmitido por meio de variáveis de ambiente. Outros parâmetros e segredos personalizados podem ser armazenados no `.env` arquivo, como credenciais de desenvolvimento para serviços Web de terceiros.

## Referencie a `private.key`

![chave privada](assets/environment-variables/private-key.png)

Abra o `.env` arquivo, remova o comentário do `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e forneça o caminho absoluto no sistema de arquivos para a variável `private.key` que corresponde ao certificado público adicionado ao projeto do Adobe I/O App Builder.

+ Se o par de chaves foi gerado pelo Adobe I/O, ele foi baixado automaticamente como parte do  `config.zip`.
+ Se você forneceu a chave pública para o Adobe I/O, então você também deve estar de posse da chave privada correspondente.
+ Se você não tiver esses pares de chaves, poderá gerar novos pares de chaves ou fazer upload de novas chaves públicas na parte inferior do:
   [https://console.adobe.com](https://console.adobe.io) > Seu projeto do Asset compute App Builder > Workspaces @ Development > Conta de serviço (JWT).

Lembre-se do `private.key` O arquivo não deve ser verificado no Git porque contém segredos, mas deve ser armazenado em um local seguro fora do projeto.

Por exemplo, no macOS, pode ser semelhante a:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurar credenciais do Armazenamento na nuvem

O desenvolvimento local dos trabalhadores Assets compute exige o [armazenamento na nuvem](../set-up/accounts-and-services.md#cloud-storage). As credenciais de armazenamento na nuvem usadas para o desenvolvimento local são fornecidas na `.env` arquivo.

Este tutorial prefere o uso do Armazenamento Azure Blob, no entanto, o Amazon S3 e suas chaves correspondentes no `.env` arquivo pode ser usado em seu lugar.

### Usando o armazenamento Azure Blob

Remova o comentário e preencha as seguintes chaves na `.env` e preencha-os com os valores do armazenamento em nuvem provisionado encontrado no Portal do Azure.

![Armazenamento Azure Blob](./assets/environment-variables/azure-portal-credentials.png)

1. Valor para o `AZURE_STORAGE_CONTAINER_NAME` key
1. Valor para o `AZURE_STORAGE_ACCOUNT` key
1. Valor para o `AZURE_STORAGE_KEY` key

Por exemplo, isso pode parecer com (valores somente para ilustração):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

O resultado `.env` O arquivo tem a seguinte aparência:

![Credenciais do Armazenamento Azure Blob](assets/environment-variables/cloud-storage-credentials.png)

Se você NÃO estiver usando o Armazenamento de blobs do Microsoft Azure, remova-os ou deixe-os comentados (prefixando com `#`).

### Uso do armazenamento em nuvem Amazon S3{#amazon-s3}

Se estiver usando o armazenamento na nuvem do Amazon S3, remova o comentário e preencha as seguintes chaves na `.env` arquivo.

Por exemplo, isso pode parecer com (valores somente para ilustração):

```
...
S3_BUCKET=aemguideswkndassetcompute
AWS_ACCESS_KEY_ID=KKIXZLZYNLXJLV24PLO6
AWS_SECRET_ACCESS_KEY=Ba898CnisgabdsNJEJBqCYyVrYttbGbZ2...OiNYExll0vwoLsPtOv
AWS_REGION=us-east-1
...
```

## Validar a configuração do projeto

Após configurar o projeto de Asset compute gerado, valide a configuração antes de fazer alterações no código para garantir que os serviços de suporte sejam provisionados, no `.env` arquivos.

Para iniciar a Ferramenta de desenvolvimento do Asset compute para o projeto do Asset compute:

1. Abra uma linha de comando na raiz do projeto Asset compute (no Código VS, isso pode ser aberto diretamente no IDE via Terminal > Novo terminal) e execute o comando:

   ```
   $ aio app run
   ```

1. A Ferramenta de desenvolvimento de Asset compute local será aberta no navegador da Web padrão em __http://localhost:9000__.

   ![execução do aplicativo aio](assets/environment-variables/aio-app-run.png)

1. Observe a saída da linha de comando e o navegador da Web em busca de mensagens de erro enquanto a Ferramenta de desenvolvimento é inicializada.
1. Para interromper a Ferramenta de desenvolvimento do Asset compute, toque em `Ctrl-C` na janela que executou `aio app run` para encerrar o processo.

## Resolução de problemas

+ [A Ferramenta de desenvolvimento não pode ser iniciada devido à falta de private.key](../troubleshooting.md#missing-private-key)
