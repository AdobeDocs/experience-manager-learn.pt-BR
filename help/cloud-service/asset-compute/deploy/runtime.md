---
title: Implante trabalhadores do Asset compute no Adobe I/O Runtime para uso com AEM as a Cloud Service
description: Os projetos do Asset compute e os trabalhadores que eles contêm devem ser implantados no Adobe I/O Runtime para serem usados AEM as a Cloud Service.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Implantar no Adobe I/O Runtime

Os projetos do Asset compute e os trabalhadores que eles contêm devem ser implantados no Adobe I/O Runtime por meio da CLI do Adobe I/O para serem usados AEM as a Cloud Service.

Ao implantar no Adobe I/O Runtime para uso AEM serviços de Autor as a Cloud Service, somente duas variáveis de ambiente são necessárias:

+ `AIO_runtime_namespace` aponta o App Builder Workspace para implantação do
+ `AIO_runtime_auth` são as credenciais de autenticação do espaço de trabalho do App Builder

As outras variáveis padrão definidas na variável `.env` são fornecidas implicitamente por AEM as a Cloud Service quando chama o Asset compute.

## Espaço de trabalho de desenvolvimento

Porque este projeto foi gerado usando `aio app init` usando o `Development` espaço de trabalho, `AIO_runtime_namespace` é automaticamente definido como `81368-wkndaemassetcompute-development` com a correspondência `AIO_runtime_auth` em nosso local `.env` arquivo.  Se uma `.env` existe no diretório usado para emitir o comando deploy, seus valores são usados, a menos que sejam substituídos por uma exportação de variável no nível do sistema operacional, que é a forma como [fase e produção](#stage-and-production) os espaços de trabalho são direcionados.

![implantação do aplicativo aio usando variáveis .env](./assets/runtime/development__aio.png)

Para implantar no espaço de trabalho, defina nos projetos `.env` arquivo:

1. Abra a linha de comando na raiz do projeto do Asset compute
1. Execute o comando `aio app deploy`
1. Execute o comando `aio app get-url` para obter o URL do trabalhador a ser usado no Perfil de processamento as a Cloud Service do AEM para fazer referência a este trabalhador de Asset compute personalizado. Se o projeto contiver vários trabalhadores, os URLs separados de cada trabalhador serão listados.

Se os ambientes de desenvolvimento local e AEM de desenvolvimento as a Cloud Service usarem implantações de Asset compute separadas, as implantações AEM desenvolvimento as a Cloud Service poderão ser gerenciadas da mesma maneira que [Implantações de preparo e produção](#stage-and-production).

## Espaços de trabalho de preparo e produção{#stage-and-production}

A implantação de espaços de trabalho de preparo e produção geralmente é feita pelo sistema de CI/CD de sua escolha. O projeto do Asset compute deve ser implantado em cada espaço de trabalho (Preparo e Produção) de forma discreta.

A configuração de variáveis de ambiente verdadeiras substitui os valores para as variáveis com o mesmo nome em `.env`.

![implantação do aplicativo aio usando variáveis de exportação](./assets/runtime/stage__export-and-aio.png)

A abordagem geral, normalmente automatizada por um sistema de CI/CD, para implantação em ambientes de Preparo e Produção é:

1. Verifique se a [Módulo Adobe I/O CLI npm e plug-in Asset compute](../set-up/development-environment.md#aio) estão instalados
1. Confira o projeto do Asset compute para implantar a partir do Git
1. Defina as variáveis de ambiente com os valores que correspondem ao espaço de trabalho de destino (Preparo ou Produção)
   + As duas variáveis necessárias são `AIO_runtime_namespace` e `AIO_runtime_auth` e são obtidas por espaço de trabalho no Console do desenvolvedor do Adobe I/O por meio do __Baixar tudo__ recurso.

![Console do desenvolvedor do Adobe - Namespace e Auth do tempo de execução do AIO](./assets/runtime/stage-auth-namespace.png)

Os valores dessas chaves podem ser definidos emitindo comandos de exportação da linha de comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se os funcionários do Asset compute exigirem outras variáveis, como no armazenamento em nuvem, elas também devem ser exportadas como variáveis de ambiente.

1. Depois que todas as variáveis de ambiente forem definidas para o espaço de trabalho de destino para implantação, execute o comando deploy:
   + `aio app deploy`
1. Os URLs de trabalho referenciados pelo Perfil de processamento as a Cloud Service AEM também estão disponíveis por meio de:
   + `aio app get-url`.

Se a versão do projeto do Asset compute alterar os URLs do trabalhador, também serão alterados para refletir a nova versão, e o URL precisará ser atualizado nos Perfis de processamento.

## Provisionamento da API do Workspace{#workspace-api-provisioning}

When [configuração do projeto do App Builder no Adobe I/O](../set-up/app-builder.md) para apoiar o desenvolvimento local, um novo espaço de trabalho de desenvolvimento foi criado e __asset compute, eventos de E/S__ e __APIs de gerenciamento de eventos de E/S__ foram adicionados a ele.

O __asset compute, eventos de E/S__ e __APIs de gerenciamento de eventos de E/S__ APIS são explicitamente adicionados aos espaços de trabalho usados para desenvolvimento local. Espaços de trabalho que integram (exclusivamente) AEM ambientes as a Cloud Service __not__ É necessário que essas APIs sejam explicitamente adicionadas, pois as APIs são disponibilizadas naturalmente para AEM as a Cloud Service.
