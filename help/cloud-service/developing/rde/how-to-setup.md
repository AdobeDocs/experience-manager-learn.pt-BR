---
title: Como configurar um ambiente de desenvolvimento rápido
description: Saiba como configurar o Ambiente de desenvolvimento rápido para o AEM as a Cloud Service.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '668'
ht-degree: 2%

---

# Como configurar um ambiente de desenvolvimento rápido

Saiba **como configurar** o RDE (Rapid Development Environment, ambiente de desenvolvimento rápido) no AEM as a Cloud Service.

Este vídeo mostra:

- Adicionar um RDE ao seu programa usando o Cloud Manager
- Fluxo de logon RDE usando o Adobe IMS, como ele é semelhante a qualquer outro ambiente do AEM as a Cloud Service
- A instalação da [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) também conhecida como `aio CLI`
- Instalação e configuração do plug-in do AEM RDE e do Cloud Manager `aio CLI` usando o modo não interativo. Para o modo interativo, consulte as [instruções de configuração](#setup-the-aem-rde-plugin)

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## Pré-requisitos

Devem ser instalados:

- [Node.js](https://nodejs.org/en/) (LTS - Suporte a Longo Prazo)
- [npm 8+](https://docs.npmjs.com/)

## Configuração local

Para implantar o código e o conteúdo do [WKND Sites Project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) no RDE a partir da máquina local, conclua as etapas a seguir.

### CLI extensível do Adobe I/O Runtime

Instale o Adobe I/O Runtime Extensible CLI, também conhecido como `aio CLI`, executando o seguinte comando na linha de comando.

```shell
$ npm install -g @adobe/aio-cli
```

### Instalar e configurar plug-ins da CLI do aio

A CLI aio deve ter plug-ins instalados e configurados com a ID de ambiente de organização, programa e RDE para interagir com seu RDE. A configuração pode ser executada por meio da interface de linha de comando aio usando o modo interativo mais simples ou o modo não interativo.

>[!BEGINTABS]

>[!TAB Modo interativo]

Instale e configure os plug-ins do AEM RDE usando o comando `plugins:install` do `aio cli`.

1. Instale o plug-in RDE AEM da interface aio usando o comando `plugins:install` de `aio cli`.

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   O plug-in RDE do AEM permite que os desenvolvedores implantem código e conteúdo da máquina local.

2. Faça logon na Adobe I/O Runtime Extensible CLI executando o seguinte comando para obter o token de acesso. Certifique-se de fazer logon na mesma organização da Adobe que o Cloud Manager.

   ```shell
   $ aio login
   ```

3. Execute o seguinte comando para configurar o RDE usando o modo interativo.

   ```shell
   $ aio aem:rde:setup
   ```

4. A CLI solicita que você insira a ID da organização, a ID do programa e a ID do ambiente.

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - Escolha __Não__ se estiver trabalhando apenas com um único RDE e quiser armazenar a configuração do RDE globalmente no computador local.

   - Escolha __Sim__ se estiver trabalhando com vários RDEs ou quiser armazenar a configuração de RDE localmente, no arquivo `.aio` da pasta atual, para cada projeto.

5. Selecione a ID da organização, a ID do programa e a ID de ambiente RDE na lista de opções disponíveis.

6. Verifique se a Organização, o Programa e o Ambiente corretos estão configurados executando o comando a seguir.

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB Modo não interativo]

Instale e configure os plug-ins do Cloud Manager e do AEM RDE usando o comando `plugins:install` do `aio cli`.

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

O plug-in do Cloud Manager permite que os desenvolvedores interajam com o Cloud Manager a partir da linha de comando.

O plug-in RDE do AEM permite que os desenvolvedores implantem código e conteúdo da máquina local.

Os plug-ins da CLI aio devem ser configurados para interagir com o RDE.

1. Primeiro, usando o Cloud Manager, copie os valores da Organização, do Programa e da ID de ambiente.

   - ID da organização: copie o valor de **Imagem do perfil > Informações da conta (internas) > Janela modal > ID da organização atual**

   ![ID da Organização](./assets/Org-ID.png)

   - ID do Programa: Copie o valor de **Visão Geral do Programa > Ambientes > {ProgramName}-rde > URI do Navegador > números entre `program/` e`/environment`**

   ![ID do Programa e do Ambiente](./assets/Program-Environment-Id.png)

   - ID de Ambiente: Copie o valor de **Visão Geral do Programa > Ambientes > {ProgramName}-rde > URI do Navegador > números após`environment/`**

   ![ID do Programa e do Ambiente](./assets/Program-Environment-Id.png)

1. Use o comando `config:set` de `aio cli` para definir esses valores executando o comando a seguir.

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. Verifique os valores de configuração atuais executando o comando a seguir.

   ```shell
   $ aio config:list
   ```

1. Alternar ou revisar em qual organização você está conectado no momento:

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## Verificar acesso ao RDE

Verifique a instalação e a configuração do plug-in RDE do AEM executando o seguinte comando.

```shell
$ aio aem:rde:status
```

As informações de status do RDE são exibidas como status do ambiente, a lista de _seus projetos do AEM_ agrupados e configurações nos serviços de criação e publicação.

## Próxima etapa

Saiba [como usar](./how-to-use.md) um RDE para implantar código e conteúdo de seu ambiente de desenvolvimento integrado (IDE) favorito para ciclos de desenvolvimento mais rápidos.


## Recursos adicionais

[Habilitando RDE na documentação de um programa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

A instalação da [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) também conhecida como `aio CLI`

[uso e comandos da interface de usuário da aio](https://github.com/adobe/aio-cli#usage)

[Plug-in da CLI do Adobe I/O Runtime para interações com os ambientes de desenvolvimento AEM Rapid](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Plug-in da CLI do Cloud Manager aio](https://github.com/adobe/aio-cli-plugin-cloudmanager)
