---
title: Configurar tempo de execução local AEM para AEM como um desenvolvimento Cloud Service
description: Configure o Tempo de execução local AEM usando o AEM como um Jar de início rápido do Cloud Service SDK.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: 4cfbf975919eb38413be8446b70b107bbfebb845
workflow-type: tm+mt
source-wordcount: '1406'
ht-degree: 1%

---


# Configurar tempo de execução AEM local

O Adobe Experience Manager (AEM) pode ser executado localmente usando o AEM como um Jar de Início Rápido do SDK do Cloud Service. Isso permite que os desenvolvedores implantem e testem código, configuração e conteúdo personalizados antes de confirmá-los no controle de origem e implantá-los em um AEM como ambiente Cloud Service.

Observe que `~` é usado como abreviação para o Diretório do usuário. No Windows, isso é o equivalente a `%HOMEPATH%`.

## Instalar Java

O Experience Manager é um aplicativo Java e, portanto, requer que o Java SDK suporte as ferramentas de desenvolvimento.

1. [Baixe e instale o Java SDK 11 mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14)
1. Verifique se o Java 11 SDK está instalado executando o comando:
   + Windows:`java -version`
   + macOS / Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Baixe o AEM como um SDK Cloud Service

O AEM como SDK do Cloud Service, ou SDK do AEM, contém o Jar do Quickstart usado para executar o Autor do AEM e publicar localmente para desenvolvimento, bem como a versão compatível das Ferramentas do Dispatcher.

1. Faça logon em [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) com seu Adobe ID
   + Observe que sua Organização Adobe __deve__ ser provisionada para AEM como Cloud Service para baixar o AEM como um SDK Cloud Service.
1. Navegue até a __AEM como uma guia Cloud Service__
1. Classificar por data __de__ publicação em ordem __decrescente__
1. Clique na linha de resultado mais recente do __AEM SDK__
1. Revise e aceite o EULA e toque no botão __Download__

## Extraia o Jar do Quickstart do zip do SDK AEM

1. Descompacte o arquivo baixado `aem-sdk-XXX.zip`

## Configurar o serviço de autor de AEM local{#set-up-local-aem-author-service}

O serviço de autor de AEM local fornece aos desenvolvedores uma experiência local com os profissionais de marketing/conteúdo digital que os autores compartilharão para criar e gerenciar conteúdo.  O serviço de autor de AEM foi criado como um ambiente de criação e pré-visualização, permitindo que a maioria das validações do desenvolvimento de recursos seja executada contra ele, tornando-o um elemento vital do processo de desenvolvimento local.

1. Criar a pasta `~/aem-sdk/author`
1. Copie o arquivo JAR __do__ Quickstart para `~/aem-sdk/author` e renomeie-o para `aem-author-p4502.jar`
1. Start o serviço de autor de AEM local executando o seguinte na linha de comando:
   + `java -jar aem-author-p4502.jar`
      + Forneça a senha do administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para reduzir a necessidade de reconfiguração.

   Você *não pode* start o AEM como Cloud Service Quickstart Jar [clicando](#troubleshooting-double-click)no duplo.
1. Acesse o serviço de autor de AEM local em [http://localhost:4502](http://localhost:4502) em um navegador da Web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## Configurar o serviço local de publicação de AEM

O serviço local de publicação de AEM fornece aos desenvolvedores a experiência local que os usuários finais do AEM terão, como navegar no site hospedado no AEM. Um serviço de publicação de AEM local é importante, pois integra-se às ferramentas [de](./dispatcher-tools.md) Dispatcher do SDK AEM e permite que os desenvolvedores façam um teste de fumaça e ajustem a experiência final com o usuário final.

1. Criar a pasta `~/aem-sdk/publish`
1. Copie o arquivo JAR __do__ Quickstart para `~/aem-sdk/publish` e renomeie-o para `aem-publish-p4503.jar`
1. Start o serviço de publicação de AEM local executando o seguinte na linha de comando:
   + `java -jar aem-publish-p4503.jar`
      + Forneça a senha do administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para reduzir a necessidade de reconfiguração.

   Você *não pode* start o AEM como Cloud Service Quickstart Jar [clicando](#troubleshooting-double-click)no duplo.
1. Acesse o serviço local de publicação de AEM em [http://localhost:4503](http://localhost:4503) em um navegador da Web

Windows:

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS / Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Modos de start do Jar de início rápido

O nome do Jar de Início Rápido, `aem-<tier>_<environment>-p<port number>.jar` especifica como ele será start. Depois de AEM como iniciado em uma camada específica, autor ou publicação, ele não pode ser alterado para a camada alternativa. Para fazer isso, a `crx-Quickstart` pasta gerada durante a primeira execução deve ser excluída e o Jar de Início Rápido deve ser executado novamente. Ambiente e portas podem ser alteradas, no entanto, exigem a interrupção/start da instância de AEM local.

A alteração de ambientes, `dev`e `stage` `prod`, pode ser útil para os desenvolvedores para garantir que as configurações específicas do ambiente sejam definidas e resolvidas corretamente pelo AEM. É recomendável que o desenvolvimento local seja feito principalmente em relação ao modo de execução padrão do `dev` ambiente.

As permutações disponíveis são as seguintes:

+ `aem-author-p4502.jar`
   + Como Autor no modo de execução Dev na porta 4502
+ `aem-author_dev-p4502.jar`
   + Como Autor no modo de execução Dev na porta 4502 (igual a `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + Como autor no modo de execução de preparo na porta 4502
+ `aem-author_prod-p4502.jar`
   + Como Autor no modo de execução Produção na porta 4502
+ `aem-publish-p4503.jar`
   + Como Autor no modo de execução Dev na porta 4503
+ `aem-publish_dev-p4503.jar`
   + Como Autor no modo de execução Dev na porta 4503 (igual a `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + Como autor no modo de execução de preparo na porta 4503
+ `aem-publish_prod-p4503.jar`
   + Como Autor no modo de execução Produção na porta 4503

Observe que o número da porta pode ser qualquer porta disponível na máquina de desenvolvimento local, no entanto, por convenção:

+ A porta __4502__ é usada para o serviço de autor de AEM __local__
+ A porta __4503__ é usada para o serviço de publicação de AEM __local__

A alteração dessas configurações pode exigir ajustes AEM configurações do SDK

## Interrompendo um tempo de execução de AEM local

Para interromper um tempo de execução local AEM, seja o AEM Author ou o serviço Publish, abra a janela de linha de comando usada para start do tempo de execução AEM e toque em `Ctrl-C`. Aguarde AEM desligar. Quando o processo de desligamento estiver concluído, o prompt da linha de comando estará disponível.

## Quando atualizar o Jar de Início Rápido

Atualize o SDK AEM pelo menos mensalmente na última quinta-feira de cada mês, ou logo depois dela, que é a cadência de lançamento para AEM como um &quot;lançamento de recursos&quot; de Cloud Service.

>[!WARNING]
>
> A atualização do Jar do Quickstart para uma nova versão requer a substituição de todo o ambiente de desenvolvimento local, resultando na perda de todo o código, configuração e conteúdo nos repositórios de AEM locais. Certifique-se de que qualquer código, configuração ou conteúdo que não deve ser destruído seja atribuído com segurança ao Git ou exportado da instância de AEM local como AEM Packages.

### Como evitar perda de conteúdo ao atualizar o SDK AEM

A atualização do SDK AEM está criando efetivamente um novo tempo de execução AEM, incluindo um novo repositório, o que significa que todas as alterações feitas em um repositório anterior AEM SDK serão perdidas. As estratégias a seguir são viáveis para auxiliar no conteúdo persistente entre atualizações AEM SDK e podem ser usadas discretamente ou em conjunto:

1. Crie um pacote de conteúdo dedicado a conter conteúdo de &quot;amostra&quot; para auxiliar no desenvolvimento e mantê-lo no Git. Qualquer conteúdo que deva ser mantido por meio de atualizações AEM SDK seria mantido neste pacote e reimplantado após a atualização do SDK AEM.
1. Use [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) com a `includepaths` diretiva para copiar o conteúdo do repositório anterior do SDK AEM para o novo repositório AEM do SDK.
1. Faça backup de qualquer conteúdo usando AEM Package Manager e pacotes de conteúdo no SDK AEM anterior e reinstale-os no novo SDK AEM.

Lembre-se, usar as abordagens acima para manter o código entre AEM atualizações do SDK indica um antipadrão de desenvolvimento. O código não descartável deve se originar no seu IDE de desenvolvimento e fluir para AEM SDK por meio de implantações.

## Resolução de problemas

## Ao clicar no duplo no arquivo Jar do Quickstart, ocorre um erro{#troubleshooting-double-click}

Ao clicar com o duplo na barra de início rápido para o start, um modal de erro é exibido, impedindo o AEM de iniciar localmente.

![Solução de problemas - clique em Duplo no arquivo Jar do Quickstart](./assets/aem-runtime/troubleshooting__double-click.png)

Isso ocorre porque AEM como um Jar de Início Rápido Cloud Service não suporta clique em duplo do Jar de Início Rápido para start AEM localmente. Em vez disso, você deve executar o arquivo Jar a partir dessa linha de comando.

Para start do serviço de autor de AEM, entre `cd` no diretório que contém o Jar de Início Rápido e execute o comando:

`$ java -jar aem-author-p4502.jar`

ou, para start do serviço de publicação de AEM, `cd` no diretório que contém o Jar de Início Rápido e execute o comando:

`$ java -jar aem-author-p4503.jar`

## Iniciar o Jar de Início Rápido na linha de comando cancela imediatamente{#troubleshooting-java-8}

Ao iniciar o Jar de Início Rápido na linha de comando, o processo é abortado imediatamente e o serviço AEM não é start, com o seguinte erro:

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

Isso ocorre porque AEM como Cloud Service requer o Java SDK 11 e você está executando uma versão diferente, provavelmente o Java 8. Para resolver esse problema, baixe e instale o [Oracle Java SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr jcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14).
Depois que o Java SDK 11 tiver sido instalado, verifique se é a versão ativa executando o seguinte na linha de comando.

Depois que o Java 11 SDK estiver instalado, verifique se é a versão ativa executando o comando da linha de comando:

+ Windows: `java -version`
+ macOS / Linux: `java --version`

## Recursos adicionais

+ [Baixar AEM SDK](https://experience.adobe.com/#/downloads)
+ [Gerenciador da Adobe Cloud](https://my.cloudmanager.adobe.com/)
+ [Baixar o Docker](https://www.docker.com/)
+ [Documentação do Dispatcher Experience Manager](https://docs.adobe.com/content/help/pt-BR/experience-manager-dispatcher/using/dispatcher.html)
