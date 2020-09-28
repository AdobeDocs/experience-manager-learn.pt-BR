---
title: Implantar funcionários da Asset Compute na Adobe I/O Runtime para uso com AEM como Cloud Service
description: 'Os projetos de Computação de ativos e os trabalhadores que eles contêm devem ser implantados na Adobe I/O Runtime para serem usados pelo AEM como Cloud Service. '
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
translation-type: tm+mt
source-git-commit: 50519b9526182b528047069f211498099e3a4c88
workflow-type: tm+mt
source-wordcount: '649'
ht-degree: 0%

---


# Implantar no Adobe I/O Runtime

Os projetos de Computação de ativos e os trabalhadores que eles contêm devem ser implantados na Adobe I/O Runtime via CLI de E/S do Adobe para serem usados pelo AEM como Cloud Service.

Ao implantar no Adobe I/O Runtime para uso por AEM como um serviço de Cloud Service Author, somente duas variáveis de ambiente são necessárias:

+ `AIO_runtime_namespace` aponta o Adobe Project Firefly Workspace para implantação
+ `AIO_runtime_auth` são as credenciais de autenticação da área de trabalho do Adobe Project Firefly

As outras variáveis padrão definidas no `.env` arquivo são implicitamente fornecidas pelo AEM como um Cloud Service quando ele chama o trabalhador da Computação de ativos.

## Espaço de trabalho de desenvolvimento

Como este projeto foi gerado usando `aio app init` a `Development` área de trabalho, `AIO_runtime_namespace` é automaticamente definido como `81368-wkndaemassetcompute-development` a correspondência `AIO_runtime_auth` no `.env` arquivo local.  Se existir um `.env` arquivo no diretório usado para emitir o comando de implantação, seus valores serão usados, a menos que sejam substituídos por uma exportação variável de nível do SO, que é a forma como os espaços de trabalho de [estágio e produção](#stage-and-production) são direcionados.

![implantação do aplicativo aio usando variáveis .env](./assets/runtime/development__aio.png)

Para implantar no espaço de trabalho, defina no arquivo de projetos `.env` :

1. Abra a linha de comando na raiz do projeto do aplicativo Asset Compute
1. Executar o comando `aio app deploy`
1. Execute o comando `aio app get-url` para obter o URL do trabalhador para uso no AEM como um Perfil de processamento de Cloud Service para fazer referência a esse funcionário personalizado do Asset Compute. Se o projeto contiver vários trabalhadores, URLs separados para cada trabalhador serão listados.

Se o desenvolvimento local e o AEM como ambientes de Desenvolvimento de Cloud Service, usar implantações separadas de Computação de ativos, as implantações para AEM como um Desenvolvedor de Cloud Service podem ser gerenciadas da mesma maneira que as implantações [de](#stage-and-production)Estágio e Produção.

## Espaços de trabalho de estágio e produção{#stage-and-production}

A implantação para o estágio e os espaços de trabalho de produção geralmente são feitos pelo sistema de CI/CD de sua escolha. O projeto de Computação de ativos deve ser implantado em cada Workspace (Stage e, em seguida, Production) de forma discreta.

A definição de variáveis de ambiente verdadeiras substitui os valores das variáveis com o mesmo nome em `.env`.

![implantação de aplicativo do rádio usando variáveis de exportação](./assets/runtime/stage__export-and-aio.png)

A abordagem geral, normalmente automatizada por um sistema de CI/CD, para implantação em ambientes de estágio e produção é:

1. Verifique se o módulo [Adobe I/O CLI npm e o plug-in](../set-up/development-environment.md#aio) Asset Compute estão instalados
1. Confira o aplicativo Asset Compute a ser implantado do Git
1. Defina as variáveis do ambiente com os valores que correspondem à área de trabalho do público alvo (Palco ou Produção)
   + As duas variáveis necessárias são `AIO_runtime_namespace` e são obtidas por espaço de trabalho no Console do desenvolvedor de E/S do Adobe por meio do recurso `AIO_runtime_auth` Download de tudo ____ da Workspace.

![Console do desenvolvedor do Adobe - Namespace e autenticação do tempo de execução AIO](./assets/runtime/stage-auth-namespace.png)

Os valores dessas chaves podem ser definidos emitindo comandos de exportação da linha de comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se os funcionários da Computação de ativos exigirem outras variáveis, como no armazenamento em nuvem, elas também devem ser exportadas como variáveis de ambiente.

1. Depois que todas as variáveis de ambiente forem definidas para o espaço de trabalho do público alvo a ser implantado, execute o comando deployment:
   + `aio app deploy`
1. Os URLs de trabalho referenciados pelo AEM como Perfil de processamento de Cloud Service também estão disponíveis via:
   + `aio app get-url`.

Se a versão do aplicativo Asset Compute alterar o(s) URL(s) do trabalhador também é alterado para refletir a nova versão, e o URL precisará ser atualizado nos Perfis de processamento.

## Provisionamento da API do espaço de trabalho{#workspace-api-provisioning}

Ao [configurar o projeto Adobe Project Firefly em E/S](../set-up/firefly.md) de Adobe para suportar o desenvolvimento local, uma nova área de trabalho de Desenvolvimento foi criada e as APIs __de gerenciamento de Eventos de E/S, Eventos__ de E/S e computação de ____ ativos.

Os APIS __Asset Compute, Eventos__ de E/S e APIs __de gerenciamento de Eventos de__ E/S só são explicitamente adicionados aos espaços de trabalho usados para o desenvolvimento local. Os espaços de trabalho que se integram (exclusivamente) a AEM como ambientes Cloud Service __não__ precisam dessas APIs explicitamente adicionadas, pois as APIs são disponibilizadas naturalmente para AEM como Cloud Service.
