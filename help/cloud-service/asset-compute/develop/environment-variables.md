---
title: Configurar as variáveis de ambiente para extensibilidade do Asset Compute
description: As variáveis de ambiente são mantidas no arquivo .env para desenvolvimento local e são usadas para fornecer credenciais do Adobe I/O e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6270
thumbnail: KT-6270.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: c63c5c75-1deb-4c16-ba33-e2c338ef6251
duration: 121
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Configurar as variáveis de ambiente

![arquivo env. dot](assets/environment-variables/dot-env-file.png)

Antes de iniciar o desenvolvimento de trabalhadores do Asset Compute, verifique se o projeto está configurado com informações de armazenamento do Adobe I/O e da nuvem. Essas informações são armazenadas no `.env` do projeto, que é usado apenas para desenvolvimento local, e não são salvas no Git. O arquivo `.env` fornece uma maneira conveniente de expor pares de chave/valores ao ambiente de desenvolvimento local do Asset Compute. Ao [implantar](../deploy/runtime.md) trabalhadores do Asset Compute para o Adobe I/O Runtime, o arquivo `.env` não é usado, mas um subconjunto de valores é transmitido por meio de variáveis de ambiente. Outros segredos e parâmetros personalizados também podem ser armazenados no arquivo `.env`, como credenciais de desenvolvimento para serviços Web de terceiros.

## Referenciar o `private.key`

![chave privada](assets/environment-variables/private-key.png)

Abra o arquivo `.env`, remova o comentário da chave `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e forneça o caminho absoluto em seu sistema de arquivos para o `private.key` que é emparelhado com o certificado público adicionado ao seu projeto do Adobe I/O App Builder.

+ Se o par de chaves foi gerado pelo Adobe I/O, ele foi baixado automaticamente como parte do `config.zip`.
+ Se você forneceu a chave pública para o Adobe I/O, também deverá estar de posse da chave privada correspondente.
+ Se você não tiver esses pares de chaves, poderá gerar novos pares de chaves ou fazer upload de novas chaves públicas na parte inferior do:
  [https://console.adobe.com](https://console.adobe.io) > Seu projeto do Asset Compute App Builder > Workspaces @ Development > Conta de Serviço (JWT).

Lembre-se de que não é necessário fazer check-in do arquivo `private.key` no Git, pois ele contém segredos. Em vez disso, ele deve ser armazenado em um local seguro fora do projeto.

Por exemplo, no macOS, pode ser semelhante a:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurar credenciais do Armazenamento na nuvem

O desenvolvimento local de trabalhadores do Asset Compute requer acesso ao [armazenamento na nuvem](../set-up/accounts-and-services.md#cloud-storage). As credenciais de armazenamento na nuvem usadas para desenvolvimento local são fornecidas no arquivo `.env`.

Este tutorial prefere o uso do Armazenamento Azure Blob, no entanto, o Amazon S3 e suas chaves correspondentes no arquivo `.env` podem ser usados.

### Usando o armazenamento Azure Blob

Remova o comentário e preencha as seguintes chaves no arquivo `.env` e preencha-as com os valores do armazenamento na nuvem provisionado encontrado no Portal do Azure.

![Armazenamento Azure Blob](./assets/environment-variables/azure-portal-credentials.png)

1. Valor da chave `AZURE_STORAGE_CONTAINER_NAME`
1. Valor da chave `AZURE_STORAGE_ACCOUNT`
1. Valor da chave `AZURE_STORAGE_KEY`

Por exemplo, isso pode parecer com (valores somente para ilustração):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

O arquivo resultante `.env` tem a seguinte aparência:

![Credenciais do Armazenamento Azure Blob](assets/environment-variables/cloud-storage-credentials.png)

Se você NÃO estiver usando o Armazenamento de blobs do Microsoft Azure, remova-os ou deixe-os comentados (prefixando com `#`).

### Uso do armazenamento em nuvem Amazon S3{#amazon-s3}

Se estiver usando o armazenamento na nuvem do Amazon S3, remova o comentário e preencha as seguintes chaves no arquivo `.env`.

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

Após configurar o projeto Asset Compute gerado, valide a configuração antes de fazer alterações no código para garantir que os serviços de suporte sejam provisionados nos arquivos `.env`.

Para iniciar a Ferramenta de desenvolvimento do Asset Compute para o projeto Asset Compute:

1. Abra uma linha de comando na raiz do projeto Asset Compute (no Código VS, isso pode ser aberto diretamente no IDE via Terminal > Novo terminal) e execute o comando:

   ```
   $ aio app run
   ```

1. A Ferramenta de Desenvolvimento do Asset Compute local será aberta no navegador da Web padrão em __http://localhost:9000__.

   ![aio aplicativo executado](assets/environment-variables/aio-app-run.png)

1. Observe a saída da linha de comando e o navegador da Web em busca de mensagens de erro enquanto a Ferramenta de desenvolvimento é inicializada.
1. Para interromper a Ferramenta de Desenvolvimento do Asset Compute, toque em `Ctrl-C` na janela que executou `aio app run` para encerrar o processo.

## Resolução de problemas

+ [A Ferramenta de desenvolvimento não pode ser iniciada devido à falta de private.key](../troubleshooting.md#missing-private-key)
