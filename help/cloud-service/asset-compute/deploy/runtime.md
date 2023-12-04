---
title: Implantar trabalhadores do Asset compute no Adobe I/O Runtime para uso com AEM as a Cloud Service
description: Os projetos do Asset compute e os funcionários que eles contêm devem ser implantados no Adobe I/O Runtime para serem usados pelo AEM as a Cloud Service.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6286
thumbnail: KT-6286.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 0327cf61-fd51-4fa7-856d-3febd49c01a0
duration: 178
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---

# Implantar no Adobe I/O Runtime

Os projetos do Asset compute e os funcionários que eles contêm devem ser implantados no Adobe I/O Runtime por meio da CLI do Adobe I/O AEM a ser usada pelo as a Cloud Service.

Ao implantar no Adobe I/O Runtime AEM para uso pelos serviços do as a Cloud Service Author, somente duas variáveis de ambiente são necessárias:

+ `AIO_runtime_namespace` aponta para o Espaço de trabalho do App Builder para implantação
+ `AIO_runtime_auth` são as credenciais de autenticação do espaço de trabalho do App Builder

As outras variáveis padrão definidas no `.env` arquivo são fornecidos implicitamente pelo AEM as a Cloud Service quando ele invoca o trabalhador do Asset compute.

## Workspace de desenvolvimento

Porque este projeto foi gerado usando `aio app init` usando o `Development` espaço de trabalho, `AIO_runtime_namespace` é automaticamente definido como `81368-wkndaemassetcompute-development` com o correspondente `AIO_runtime_auth` em nosso local `.env` arquivo.  Se um `.env` existir no diretório usado para emitir o comando deploy, seus valores serão usados, a menos que sejam substituídos por uma exportação de variável de nível de SO, que é a forma [estágio e produção](#stage-and-production) os espaços de trabalho são direcionados.

![implantação de aplicativo aio usando variáveis .env](./assets/runtime/development__aio.png)

Para implantar no espaço de trabalho definido nos projetos `.env` arquivo:

1. Abra a linha de comando na raiz do projeto do Asset compute
1. Executar o comando `aio app deploy`
1. Executar o comando `aio app get-url` para obter o URL do trabalhador para uso no Perfil de processamento as a Cloud Service AEM para fazer referência a este trabalhador de Asset compute personalizado. Se o projeto contiver vários workers, os URLs discretos para cada worker serão listados.

Se os ambientes de desenvolvimento local e desenvolvimento as a Cloud Service do AEM usarem implantações separadas do Asset compute, as implantações no desenvolvimento as a Cloud Service do AEM poderão ser gerenciadas da mesma maneira que [Implantações de preparo e produção](#stage-and-production).

## Espaços de trabalho de preparo e produção{#stage-and-production}

A implantação em espaços de trabalho de preparo e produção geralmente é feita pelo sistema de CI/CD de sua escolha. O projeto do Asset compute deve ser implantado em cada espaço de trabalho (Preparo e, em seguida, Produção) de forma separada.

A configuração de variáveis de ambiente verdadeiras substitui valores para as variáveis de mesmo nome em `.env`.

![implantação do aplicativo aio usando variáveis de exportação](./assets/runtime/stage__export-and-aio.png)

A abordagem geral, normalmente automatizada por um sistema CI/CD, para implantação em ambientes de Preparo e Produção é:

1. Assegure a [Módulo npm da CLI do Adobe I/O e plug-in do Asset compute](../set-up/development-environment.md#aio) estão instalados
1. Confira o projeto do Asset compute para implantar do Git
1. Defina as variáveis de ambiente com os valores que correspondem ao espaço de trabalho de destino (Preparo ou Produção)
   + As duas variáveis necessárias são `AIO_runtime_namespace` e `AIO_runtime_auth` e são obtidos por espaço de trabalho no Console do desenvolvedor do Adobe I/O por meio da __Baixar tudo__ recurso.

![Console Adobe Developer - Namespace e autenticação de tempo de execução AIO](./assets/runtime/stage-auth-namespace.png)

Os valores dessas chaves podem ser definidos emitindo comandos export a partir da linha de comando:

```
$ export AIO_runtime_namespace=81368-wkndaemassetcompute-stage
$ export AIO_runtime_auth=27100f9f-2676-4cce-b73d-b3fb6bac47d1:0tDu307W6MboQf5VWB1BAK0RHp8xWqSy1CQc3lKe7f63o3aNtAu0Y3nAmN56502W
```

Se os funcionários do Asset compute exigirem outras variáveis, como o armazenamento em nuvem, elas também deverão ser exportadas como variáveis de ambiente.

1. Depois que todas as variáveis de ambiente forem definidas para o espaço de trabalho de destino no qual implantar, execute o comando deploy:
   + `aio app deploy`
1. O(s) URL(s) do trabalhador referenciado(s) pelo Perfil de processamento do AEM as a Cloud Service também está(ão) disponível(is) por meio de:
   + `aio app get-url`.

Se a versão do projeto do Asset compute alterar, os URLs do trabalhador também serão alterados para refletir a nova versão, e o URL precisará ser atualizado nos Perfis de processamento.

## Provisionamento da API do Workspace{#workspace-api-provisioning}

Quando [configuração do projeto App Builder no Adobe I/O](../set-up/app-builder.md) para apoiar o desenvolvimento local, foi criado um novo espaço de __Asset compute, Eventos de E/S__ e __APIs de gerenciamento de eventos de E/S__ foram adicionados a ele.

A variável __Asset compute, Eventos de E/S__ e __APIs de gerenciamento de eventos de E/S__ As APIS são adicionadas explicitamente apenas aos espaços de trabalho usados para desenvolvimento local. Espaços de trabalho que se integram (exclusivamente) a ambientes as a Cloud Service AEM fazem __não__ Essas APIs precisam ser adicionadas explicitamente, pois as APIs são disponibilizadas naturalmente para o AEM as a Cloud Service.
