---
title: Configurar as variáveis de ambiente para a extensibilidade do Asset compute
description: As variáveis de ambiente são mantidas no arquivo .env para desenvolvimento local e são usadas para fornecer credenciais do Adobe I/O e credenciais de armazenamento de nuvem necessárias para o desenvolvimento local.
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

![arquivo env de ponto](assets/environment-variables/dot-env-file.png)

Antes de iniciar o desenvolvimento dos trabalhadores do Asset compute, verifique se o projeto está configurado com informações de armazenamento do Adobe I/O e da nuvem. Essas informações são armazenadas no `.env`  que é usada apenas para desenvolvimento local, e não salva no Git. O `.env` O arquivo fornece uma maneira conveniente de expor pares de chave/valor ao ambiente de desenvolvimento local do Asset compute. When [implantação](../deploy/runtime.md) Trabalhadores do Asset compute para o Adobe I/O Runtime, a `.env` não é usado, mas um subconjunto de valores é passado por variáveis de ambiente. Outros parâmetros e segredos personalizados podem ser armazenados no `.env` também, como credenciais de desenvolvimento para serviços da Web de terceiros.

## Faça referência ao `private.key`

![chave privada](assets/environment-variables/private-key.png)

Abra o `.env` exclua o comentário do arquivo `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` e forneça o caminho absoluto do sistema de arquivos para a `private.key` que emparelha com o certificado público adicionado ao seu projeto do Adobe I/O App Builder.

+ Se o seu par de chaves foi gerado pelo Adobe I/O, ele foi baixado automaticamente como parte do  `config.zip`.
+ Se você forneceu a chave pública para o Adobe I/O, então também deve estar na posse da chave privada correspondente.
+ Se você não tiver esses pares de chaves, poderá gerar novos pares de chaves ou fazer upload de novas chaves públicas na parte inferior de:
   [https://console.adobe.com](https://console.adobe.io) > Seu projeto do Asset compute App Builder > Espaços de trabalho @ desenvolvimento > Conta de serviço (JWT).

Lembre-se do `private.key` O arquivo não deve ser verificado no Git, pois contém segredos, mas deve ser armazenado em um local seguro fora do projeto.

Por exemplo, no macOS, isso pode ter a seguinte aparência:

```
...
ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/private.key
...
```

## Configurar credenciais do Cloud Storage

O desenvolvimento local dos trabalhadores Assets compute requer o acesso a [armazenamento na nuvem](../set-up/accounts-and-services.md#cloud-storage). As credenciais de armazenamento de nuvem usadas para desenvolvimento local são fornecidas no `.env` arquivo.

Este tutorial prefere o uso do Armazenamento Azure Blob, no entanto, o Amazon S3 e suas chaves correspondentes no `.env` em vez disso, o arquivo pode ser usado.

### Usando o Armazenamento Azure Blob

Exclua o comentário e preencha as seguintes chaves na `.env` e preencha-os com os valores do armazenamento na nuvem provisionado encontrado no Portal do Azure.

![Armazenamento Azure Blob](./assets/environment-variables/azure-portal-credentials.png)

1. Valor para a variável `AZURE_STORAGE_CONTAINER_NAME` key
1. Valor para a variável `AZURE_STORAGE_ACCOUNT` key
1. Valor para a variável `AZURE_STORAGE_KEY` key

Por exemplo, pode ser semelhante a (valores somente para ilustração):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Va9CnisgdbdsNJEJBqXDyNbYppbGbZ2V...OUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

O resultante `.env` O arquivo tem a seguinte aparência:

![Credenciais de Armazenamento do Azure Blob](assets/environment-variables/cloud-storage-credentials.png)

Se NÃO estiver a utilizar o Armazenamento de Blobs do Microsoft Azure, remova ou deixe estes comentários (prefixando com `#`).

### Uso do armazenamento em nuvem Amazon S3{#amazon-s3}

Se você estiver usando o armazenamento em nuvem do Amazon S3, exclua o comentário e preencha as seguintes chaves na `.env` arquivo.

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

Depois que o projeto do Asset compute gerado tiver sido configurado, valide a configuração antes de fazer alterações de código para garantir que os serviços de suporte sejam provisionados, no `.env` arquivos.

Para iniciar a Ferramenta de desenvolvimento de Assets compute para o projeto do Asset compute:

1. Abra uma linha de comando na raiz do projeto do Asset compute (no Código VS, isso pode ser aberto diretamente no IDE via Terminal > Novo terminal) e execute o comando:

   ```
   $ aio app run
   ```

1. A Ferramenta de desenvolvimento de Asset compute local será aberta no navegador da Web padrão em __http://localhost:9000__.

   ![execução do aplicativo aio](assets/environment-variables/aio-app-run.png)

1. Observe as mensagens de erro na saída da linha de comando e no navegador da Web, conforme a Ferramenta de desenvolvimento é inicializada.
1. Para interromper a ferramenta de desenvolvimento de Assets compute, toque em `Ctrl-C` na janela que foi executada `aio app run` para encerrar o processo.

## Resolução de problemas

+ [A Ferramenta de Desenvolvimento não pode ser iniciada devido à falta de private.key](../troubleshooting.md#missing-private-key)
