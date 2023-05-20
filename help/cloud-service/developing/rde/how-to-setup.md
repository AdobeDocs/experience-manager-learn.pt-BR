---
title: Como configurar um ambiente de desenvolvimento rápido
description: Saiba como configurar o Ambiente de desenvolvimento rápido para o AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 4%

---

# Como configurar um ambiente de desenvolvimento rápido

Saiba mais **como configurar** Ambiente de desenvolvimento rápido (RDE) no AEM as a Cloud Service.

Este vídeo mostra:

- Adicionar um RDE ao seu programa usando o Cloud Manager
- Fluxo de logon RDE usando o Adobe IMS, como ele é semelhante a qualquer outro ambiente AEM as a Cloud Service
- Configuração de [CLI extensível do Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) também conhecido como `aio CLI`
- Instalação e configuração do AEM RDE e Cloud Manager `aio CLI` plug-in

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Pré-requisitos

Devem ser instalados:

- [Node.js](https://nodejs.org/en/) (LTS - Suporte a longo prazo)
- [npm 8+](https://docs.npmjs.com/)

## Configuração local

Para implantar o [do projeto do WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) código e conteúdo no RDE de sua máquina local, conclua as etapas a seguir.

### CLI extensível do Adobe I/O Runtime

Instale a Adobe I/O Runtime Extensible CLI, também conhecida como a `aio CLI` executando o comando a seguir na linha de comando.

```shell
$ npm install -g @adobe/aio-cli
```

### Plug-ins do AEM

Instale os plug-ins do Cloud Manager e do AEM RDE usando o `aio cli`do `plugins:install` comando.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

O plug-in do Cloud Manager permite que os desenvolvedores interajam com o Cloud Manager a partir da linha de comando.

O plugin RDE do AEM permite que os desenvolvedores implantem código e conteúdo da máquina local.

Além disso, para atualizar os plug-ins, use o `aio plugins:update` comando.

## Configurar os plug-ins do AEM

Os plugins para AEM devem ser configurados para interagir com o RDE. Primeiro, usando a interface do Cloud Manager, copie os valores da Organização, do Programa e da ID de ambiente.

1. ID da organização: copiar o valor de **Imagem do perfil > Informações da conta (internas) > Janela modal > ID da organização atual**

   ![ID da organização](./assets/Org-ID.png)

1. ID do programa: Copiar o valor de **Visão geral do programa > Ambientes > vermelho-{ProgramName} > URI do navegador > números entre `program/` e`/environment`**

1. ID do ambiente: copiar o valor de **Visão geral do programa > Ambientes > vermelho-{ProgramName} > URI do navegador > números depois`environment/`**

   ![ID de Programa e Ambiente](./assets/Program-Environment-Id.png)

1. Em seguida, usando o `aio cli`do `config:set` defina esses valores executando o comando a seguir.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

Você pode verificar os valores de configuração atuais executando o comando a seguir.

```shell
$ aio config:list
```

Além disso, para alternar ou saber em qual organização você está conectado no momento, use o comando abaixo.

```shell
$ aio where
```

## Verificar acesso ao RDE

Verifique a instalação e a configuração do plug-in RDE do AEM executando o seguinte comando.

```shell
$ aio aem:rde:status
```

As informações de status do RDE são exibidas como status do ambiente, a lista de _seu projeto AEM_ pacotes e configurações nos serviços de autoria e publicação.

## Próxima etapa

Saiba mais [como usar](./how-to-use.md) um RDE para implantar código e conteúdo de seu ambiente de desenvolvimento integrado (IDE) favorito para ciclos de desenvolvimento mais rápidos.


## Recursos adicionais

[Habilitar RDE na documentação de um programa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Configuração de [CLI extensível do Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) também conhecido como `aio CLI`

[Uso e comandos da CLI AIO](https://github.com/adobe/aio-cli#usage)

[Plug-in da CLI da Adobe I/O Runtime para interações com ambientes de desenvolvimento AEM Rapid](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Plug-in da CLI AIO do Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)
