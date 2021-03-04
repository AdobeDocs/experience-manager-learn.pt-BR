---
title: Implante os trabalhadores do Asset Compute no Adobe I/O Runtime para uso com o AEM as a Cloud Service
description: 'Os projetos do Asset Compute e os trabalhadores que eles contêm devem ser implantados no Adobe I/O Runtime para serem usados pelo AEM as a Cloud Service. '
feature: Microserviços do Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6286
thumbnail: KT-6286.jpg
topic: Integrações, desenvolvimento
role: Desenvolvedor
level: Intermediário, Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---


# Implantar no Adobe I/O Runtime

Os projetos do Asset Compute e os trabalhadores que eles contêm devem ser implantados no Adobe I/O Runtime por meio da CLI do Adobe I/O para serem usados pelo AEM as a Cloud Service.

Ao implantar no Adobe I/O Runtime para uso pelos serviços de autor do AEM as a Cloud Service, somente duas variáveis de ambiente são necessárias:

+ `AIO_runtime_namespace` aponta o Adobe Project Firefly Workspace para implantação do
+ `AIO_runtime_auth` são as credenciais de autenticação do espaço de trabalho do Adobe Project Firefly

As outras variáveis padrão definidas no arquivo `.env` são fornecidas implicitamente pelo AEM as a Cloud Service quando ele chama o trabalhador do Asset Compute.

## Espaço de trabalho de desenvolvimento

Como esse projeto foi gerado usando `aio app init` usando o espaço de trabalho `Development`, `AIO_runtime_namespace` é automaticamente definido como `81368-wkndaemassetcompute-development` com o `AIO_runtime_auth` correspondente no arquivo `.env` local.  Se um arquivo `.env` existir no diretório usado para emitir o comando deploy, seus valores serão usados, a menos que sejam substituídos por uma exportação de variável no nível do sistema operacional, que é a forma como os espaços de trabalho [stage e production](#stage-and-production) são direcionados.

![implantação do aplicativo aio usando variáveis .env](./assets/runtime/development__aio.png)

Para implantar no espaço de trabalho, defina no arquivo de projetos `.env` :

1. Abra a linha de comando na raiz do projeto Asset Compute
1. Execute o comando `aio app deploy`
1. Execute o comando `aio app get-url` para obter o URL do trabalhador para usar no Perfil de processamento do AEM as a Cloud Service para fazer referência a esse trabalhador do Asset Compute personalizado. Se o projeto contiver vários trabalhadores, os URLs separados de cada trabalhador serão listados.

Se os ambientes de desenvolvimento local e de desenvolvimento do AEM as a Cloud Service usarem implantações separadas do Asset Compute, as implantações no AEM as a Cloud Service Dev poderão ser gerenciadas da mesma maneira que as [implantações de Preparo e Produção](#stage-and-production).

## Espaços de trabalho de preparo e produção{#stage-and-production}

A implantação de espaços de trabalho de preparo e produção geralmente é feita pelo sistema de CI/CD de sua escolha. O projeto do Asset Compute deve ser implantado em cada Workspace (Preparo e Produção) de forma discreta.

A configuração de variáveis de ambiente verdadeiras substitui os valores para as variáveis com mesmo nome em `.env`.

![implantação do aplicativo aio usando variáveis de exportação](./assets/runtime/stage__export-and-aio.png)

A abordagem geral, normalmente automatizada por um sistema de CI/CD, para implantação em ambientes de Preparo e Produção é:

1. Verifique se o módulo [Adobe I/O CLI npm e o plug-in do Asset Compute](../set-up/development-environment.md#aio) estão instalados
1. Confira o projeto do Asset Compute para implantar do Git
1. Defina as variáveis de ambiente com os valores que correspondem ao espaço de trabalho de destino (Preparo ou Produção)
   + As duas variáveis necessárias são `AIO_runtime_namespace` e `AIO_runtime_auth` e são obtidas por espaço de trabalho no Console do desenvolvedor do Adobe I/O por meio do recurso __Baixar tudo__ do Workspace.

![Console do desenvolvedor da Adobe - Namespace e Auth do tempo de execução do AIO](./assets/runtime/stage-auth-namespace.png)

Os valores dessas chaves podem ser definidos emitindo comandos de exportação da linha de comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se os trabalhadores do Asset Compute exigirem outras variáveis, como no armazenamento em nuvem, elas também deverão ser exportadas como variáveis de ambiente.

1. Depois que todas as variáveis de ambiente forem definidas para o espaço de trabalho de destino para implantação, execute o comando deploy:
   + `aio app deploy`
1. Os URLs de trabalho referenciados pelo Perfil de processamento do AEM as a Cloud Service também estão disponíveis por meio de:
   + `aio app get-url`.

Se a versão do projeto do Asset Compute alterar os URLs do trabalhador também mudarem para refletir a nova versão, e o URL precisará ser atualizado nos Perfis de processamento.

## Provisionamento da API do Workspace{#workspace-api-provisioning}

Quando [configurar o projeto Adobe Project Firefly no Adobe I/O](../set-up/firefly.md) para suportar desenvolvimento local, um novo espaço de trabalho de desenvolvimento foi criado e __Asset Compute, Eventos de E/S__ e __APIs de gerenciamento de eventos de E/S__ foram adicionadas a ele.

As APIS __Asset Compute, I/O Events__ e __I/O Events Management APIs__ só são explicitamente adicionadas aos espaços de trabalho usados para desenvolvimento local. Os espaços de trabalho que se integram (exclusivamente) aos ambientes do AEM as a Cloud Service __not__ precisam dessas APIs explicitamente adicionadas, pois as APIs são disponibilizadas naturalmente para o AEM as a Cloud Service.
