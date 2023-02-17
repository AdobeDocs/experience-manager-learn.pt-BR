---
title: Como configurar o Ambiente de desenvolvimento rápido
description: Saiba como configurar o Ambiente de desenvolvimento rápido para AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 4%

---


# Como configurar o Ambiente de desenvolvimento rápido

Saiba mais **como configurar** Ambiente de desenvolvimento rápido (RDE) em AEM as a Cloud Service.

Este vídeo mostra:

- Adicionar um RDE ao seu programa usando o Cloud Manager
- Fluxo de logon RDE usando o Adobe IMS, como ele é semelhante a qualquer outro ambiente AEM as a Cloud Service
- Configuração de [CLI extensível do Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) também conhecido como `aio CLI`
- Configuração e configuração do AEM RDE e do Cloud Manager `aio CLI` plugin

>[!VIDEO](https://video.tv.adobe.com/v/3415490/?quality=12&learn=on)

## Pré-requisitos

Devem ser instalados:

- [Node.js](https://nodejs.org/en/) (LTS - Suporte a longo prazo)
- [npm 8+](https://docs.npmjs.com/)

## Configuração local

Para implantar o [Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) código e conteúdo no RDE da máquina local, conclua as etapas a seguir.

### CLI extensível do Adobe I/O Runtime

Instale a CLI extensível do Adobe I/O Runtime, também conhecida como `aio CLI` executando o seguinte comando a partir da linha de comando.

    &quot;shell
    $ npm install -g @adobe/aio-cli
    &quot;

### Plug-ins AEM

Instale o Cloud Manager e AEM plug-ins RDE usando o `aio cli`&#39;s `plugins:install` comando.

    &quot;shell
    $ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
    
    $ aio plugins:install @adobe/aio-cli-plugin-aem-rde
    &quot;

O plug-in do Cloud Manager permite que os desenvolvedores interajam com o Cloud Manager a partir da linha de comando.

O plug-in RDE AEM permite que os desenvolvedores implantem código e conteúdo do computador local.

Além disso, para atualizar os plug-ins, use o `aio plugins:update` comando.

## Configurar plug-ins AEM

Os plug-ins AEM devem ser configurados para interagir com seu RDE. Primeiro, usando a interface do usuário do Cloud Manager, copie os valores da Organização, Programa e ID do ambiente.

1. ID da organização: Copie o valor de **Imagem do perfil > Informações da conta (interna) > Janela modal > ID da organização atual**

   ![ID da organização](./assets/Org-ID.png)

1. ID do programa: Copie o valor de **Visão geral do programa > Ambientes > {ProgramName}-rde > URI do navegador > números entre `program/` e`/environment`**

1. ID do ambiente: Copie o valor de **Visão geral do programa > Ambientes > {ProgramName}-rde > URI do navegador > números após`environment/`**

   ![ID de Programa e Ambiente](./assets/Program-Environment-Id.png)

1. Em seguida, usando o `aio cli`&#39;s `config:set` defina esses valores executando o seguinte comando.

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

Você pode verificar os valores de configuração atuais executando o seguinte comando.

    &quot;shell
    $ aio config:list
    &quot;

Além disso, para alternar ou saber em qual organização você está conectado no momento, use o comando abaixo.

    &quot;shell
    $ aio onde
    &quot;

## Verificar acesso ao RDE

Verifique a instalação e a configuração AEM do plug-in RDE executando o seguinte comando.

    &quot;shell
    $ aio aem:rde:status
    &quot;

As informações de status do RDE são exibidas como status do ambiente, a lista de _seu projeto AEM_ pacotes e configurações no serviço de criação e publicação.

## Próxima etapa

Saiba mais [como usar](./how-to-use.md) um RDE para implantar código e conteúdo do IDE (Integrated Development Environment, ambiente de desenvolvimento integrado) favorito para ciclos de desenvolvimento mais rápidos.


## Recursos adicionais

[Ativar o RDE em uma documentação de programa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

Configuração de [CLI extensível do Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) também conhecido como `aio CLI`

[Uso e comandos da CLI do AIO](https://github.com/adobe/aio-cli#usage)

[Plug-in Adobe I/O Runtime CLI para interações com AEM ambientes de desenvolvimento rápido](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Plug-in da CLI AIO do Cloud Manager](https://github.com/adobe/aio-cli-plugin-cloudmanager)
