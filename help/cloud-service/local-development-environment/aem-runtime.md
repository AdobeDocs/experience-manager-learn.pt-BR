---
title: Configurar o SDK do AEM local para desenvolvimento no AEM as a Cloud Service
description: Configure o tempo de execução do SDK do AEM local usando o Quickstart Jar do SDK do AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 7%

---

# Configurar o SDK do AEM local {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="AEM Runtime local"
>abstract="O Adobe Experience Manager (AEM) pode ser executado localmente usando o Quickstart Jar do SDK do AEM as a Cloud Service. Isso permite que desenvolvedores implantem e testem códigos, configurações e conteúdo personalizados antes de enviá-los ao controle de origem e implantá-los em um ambiente do AEM as a Cloud Service."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=pt-BR" text="SDK do AEM as a Cloud Service"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Baixar SDK do AEM as a Cloud Service"

O Adobe Experience Manager (AEM) pode ser executado localmente usando o Quickstart Jar do SDK do AEM as a Cloud Service. Isso permite que desenvolvedores implantem e testem códigos, configurações e conteúdo personalizados antes de enviá-los ao controle de origem e implantá-los em um ambiente do AEM as a Cloud Service.

Observe que `~` é usado como abreviação para o Diretório do Usuário. No Windows, é equivalente a `%HOMEPATH%`.

## Instalar o Java™

O Experience Manager é um aplicativo Java™ e, portanto, requer o SDK Java™ do Oracle para oferecer suporte às ferramentas de desenvolvimento.

1. [Baixe e instale o SDK Java™ mais recente 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=11)
1. Verifique se o SDK do Java™ 11 do Oracle está instalado executando o comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## Baixar o SDK do AEM as a Cloud Service

O SDK do AEM as a Cloud Service, ou AEM SDK, contém o Quickstart Jar usado para executar o AEM Author e o Publish localmente para desenvolvimento, bem como a versão compatível das Ferramentas do Dispatcher.

1. Faça logon em [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) com sua Adobe ID
   + Observe que sua Organização Adobe __deve__ ser provisionada para que a AEM as a Cloud Service baixe o SDK da AEM as a Cloud Service.
1. Navegue até a guia __AEM as a Cloud Service__
1. Classificar por __Data de publicação__ em ordem __Decrescente__
1. Clique na linha de resultado mais recente __AEM SDK__
1. Revise e aceite o EULA e toque no botão __Baixar__

## Extraia o Quickstart Jar do zip do SDK do AEM

1. Descompacte o arquivo `aem-sdk-XXX.zip` baixado

## Configurar o serviço de autor local do AEM{#set-up-local-aem-author-service}

O serviço de autor local do AEM fornece aos desenvolvedores uma experiência local que os profissionais de marketing digital/autores de conteúdo compartilharão para criar e gerenciar conteúdo.  O serviço de autoria do AEM foi projetado como um ambiente de autoria e visualização, permitindo que a maioria das validações do desenvolvimento de recursos possa ser realizada em relação a ele, tornando-o um elemento vital do processo de desenvolvimento local.

1. Criar a pasta `~/aem-sdk/author`
1. Copiar o arquivo __Quickstart JAR__ para `~/aem-sdk/author` e renomeá-lo para `aem-author-p4502.jar`
1. Inicie o serviço de autor local do AEM executando o seguinte na linha de comando:
   + `java -jar aem-author-p4502.jar`
      + Forneça a senha do administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para reduzir a necessidade de reconfigurar.

   Você *não pode* iniciar o AEM como Cloud Service Quickstart Jar [clicando duas vezes](#troubleshooting-double-click).
1. Acesse o Serviço de Autor do AEM local em [http://localhost:4502](http://localhost:4502) em um navegador da Web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## Configurar o serviço AEM Publish local

O Serviço AEM Publish local fornece aos desenvolvedores a experiência local que os usuários finais do AEM terão, como navegar pelo site hospedado no AEM. Um Serviço AEM Publish local é importante, pois se integra às [ferramentas Dispatcher](./dispatcher-tools.md) do SDK do AEM e permite que os desenvolvedores façam testes simulados e ajustem a experiência final voltada para o usuário final.

1. Criar a pasta `~/aem-sdk/publish`
1. Copiar o arquivo __Quickstart JAR__ para `~/aem-sdk/publish` e renomeá-lo para `aem-publish-p4503.jar`
1. Inicie o serviço AEM Publish local executando o seguinte na linha de comando:
   + `java -jar aem-publish-p4503.jar`
      + Forneça a senha do administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para reduzir a necessidade de reconfigurar.

   Você *não pode* iniciar o AEM como Cloud Service Quickstart Jar [clicando duas vezes](#troubleshooting-double-click).
1. Acesse o serviço AEM Publish local em [http://localhost:4503](http://localhost:4503) em um navegador da Web

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## Configurar serviços locais do AEM no modo de pré-lançamento

O tempo de execução local do AEM pode ser iniciado no [modo de pré-lançamento](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=pt-BR), permitindo que um desenvolvedor compile os recursos da próxima versão do AEM as a Cloud Service. O pré-lançamento é ativado passando o argumento `-r prerelease` na primeira inicialização do tempo de execução do AEM local. Isso pode ser usado com os serviços locais AEM Author e AEM Publish.


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux®]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## Simular distribuição de conteúdo {#content-distribution}

Em um ambiente de Cloud Service verdadeiro, o conteúdo é distribuído do Serviço do Autor para o Serviço do Publish usando o [Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) e o pipeline de Adobe. O [Pipeline de Adobe](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) é um microsserviço isolado disponível somente no ambiente de nuvem.

Durante o desenvolvimento, pode ser desejável simular a distribuição de conteúdo usando o serviço local Author e Publish. Isso pode ser feito ativando os agentes de replicação herdados.

>[!NOTE]
>
> Os agentes de replicação só estão disponíveis para uso no JAR do Quickstart local e fornecem apenas uma simulação da distribuição de conteúdo.

1. Faça logon no serviço **Author** e navegue até [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. Clique em **Agente Padrão (publicação)** para abrir o agente de Replicação padrão.
1. Clique em **Editar** para abrir a configuração do agente.
1. Na guia **Configurações**, atualize os seguintes campos:

   + **Habilitado** - verificar verdadeiro
   + **Id de usuário agente** - Deixe este campo vazio

   ![Configuração do Agente de Replicação - Configurações](assets/aem-runtime/settings-config.png)

1. Na guia **Transporte**, atualize os seguintes campos:

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **Usuário** - `admin`
   + **Senha** - `admin`

   ![Configuração do Agente de Replicação - Transporte](assets/aem-runtime/transport-config.png)

1. Clique em **Ok** para salvar a configuração e habilitar o Agente de Replicação **Padrão**.
1. Agora é possível fazer alterações no conteúdo no serviço do Autor e publicá-las no serviço do Publish.

![Página do Publish](assets/aem-runtime/publish-page-changes.png)

## Modos de inicialização do Quickstart Jar

O nome do Quickstart Jar, `aem-<tier>_<environment>-p<port number>.jar` especifica como ele será iniciado. Depois que o AEM é iniciado em um nível, criação ou publicação específica, ele não pode ser alterado para o nível alternativo. Para fazer isso, a pasta `crx-Quickstart` gerada durante a primeira execução deve ser excluída e o Quickstart Jar deve ser executado novamente. O ambiente e as portas podem ser alterados, mas exigem a interrupção/início da instância local do AEM.

Alterar os ambientes, `dev`, `stage` e `prod`, pode ser útil para os desenvolvedores garantirem que as configurações específicas do ambiente sejam corretamente definidas e resolvidas pelo AEM. Recomenda-se que o desenvolvimento local seja feito principalmente em relação ao modo de execução do ambiente `dev` padrão.

As permutações disponíveis são as seguintes:

| Nome de arquivo Jar de início rápido | Descrição do modo |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | Como autor no modo de execução de desenvolvimento na porta 4502 |
| `aem-author_dev-p4502.jar` | Como Autor no modo de execução de Desenvolvimento na porta 4502 (igual a `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | Como autor no modo de execução de preparo na porta 4502 |
| `aem-author_prod-p4502.jar` | Como autor no modo de execução de produção na porta 4502 |
| `aem-publish-p4503.jar` | Como Publish no modo de execução de desenvolvimento na porta 4503 |
| `aem-publish_dev-p4503.jar` | Como Publish no modo de execução Dev na porta 4503 (igual a `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | Como Publish no modo de execução de preparo na porta 4503 |
| `aem-publish_prod-p4503.jar` | Como Publish no modo de execução de produção na porta 4503 |

Observe que o número da porta pode ser qualquer porta disponível na máquina de desenvolvimento local, no entanto, por convenção:

+ A porta __4502__ é usada para o __serviço de Autor do AEM local__
+ A porta __4503__ é usada para o __serviço AEM Publish local__

Alterar esses parâmetros pode exigir ajustes nas configurações do SDK do AEM

## Interrupção de um tempo de execução local do AEM

Para interromper um tempo de execução de AEM local, o serviço de Autor de AEM ou Publish, abra a janela de linha de comando usada para iniciar o Tempo de Execução de AEM e toque em `Ctrl-C`. Aguarde o desligamento do AEM. Quando o processo de desligamento estiver concluído, o prompt da linha de comando estará disponível.

## Tarefas opcionais de configuração do tempo de execução do AEM

+ As __variáveis de ambiente de configuração OSGi e as variáveis secretas__ são [especialmente definidas para o tempo de execução local do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#local-development), em vez de gerenciá-las usando a interface de linha de comando aio.

## Quando atualizar o Quickstart Jar

Atualize o SDK do AEM pelo menos mensalmente na última quinta-feira de cada mês ou logo após essa data, que é a cadência de lançamento para &quot;lançamentos de recursos&quot; do AEM as a Cloud Service.

>[!WARNING]
>
> Atualizar o Quickstart Jar para uma nova versão requer a substituição de todo o ambiente de desenvolvimento local, resultando na perda de todo o código, configuração e conteúdo nos repositórios AEM locais. Verifique se qualquer código, configuração ou conteúdo que não deve ser destruído foi confirmado com segurança no Git ou exportado da instância local do AEM como Pacotes de AEM.

### Como evitar perda de conteúdo ao atualizar o SDK do AEM

A atualização do SDK do AEM está criando efetivamente um novo tempo de execução do AEM, incluindo um novo repositório, o que significa que todas as alterações feitas em um repositório do SDK do AEM anterior são perdidas. As estratégias a seguir são viáveis para ajudar a criar conteúdo persistente entre as atualizações do SDK do AEM e podem ser usadas de forma discreta ou em conjunto:

1. Crie um pacote de conteúdo dedicado a conter conteúdo de &quot;amostra&quot; para auxiliar no desenvolvimento e mantenha-o no Git. Qualquer conteúdo que deve ser mantido por meio de atualizações do SDK do AEM será mantido nesse pacote e reimplantado após a atualização do SDK do AEM.
1. Use [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) com a diretiva `includepaths` para copiar o conteúdo do repositório do SDK do AEM anterior para o novo repositório do SDK do AEM.
1. Faça backup de qualquer conteúdo usando o Gerenciador de pacotes AEM e pacotes de conteúdo no SDK AEM anterior e reinstale-os no novo SDK do AEM.

Lembre-se de que usar as abordagens acima para manter o código entre as atualizações do SDK do AEM indica um antipadrão de desenvolvimento. O código não descartável deve se originar no IDE de desenvolvimento e fluir para o SDK do AEM por meio de implantações.

## Resolução de problemas

### Clicar duas vezes no arquivo Jar de início rápido resulta em um erro{#troubleshooting-double-click}

Ao clicar duas vezes no Quickstart Jar para iniciar, um modal de erro é exibido, impedindo que o AEM seja iniciado localmente.

![Solução de problemas - Clique duas vezes no arquivo Quickstart Jar](./assets/aem-runtime/troubleshooting__double-click.png)

Isso ocorre porque o AEM as a Cloud Service Quickstart Jar não oferece suporte ao clique duplo no Quickstart Jar para iniciar o AEM localmente. Em vez disso, você deve executar o arquivo Jar a partir dessa linha de comando.

Para iniciar o serviço de Autor do AEM, `cd` no diretório que contém o Quickstart Jar e execute o comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

ou, para iniciar o serviço AEM Publish, `cd` no diretório que contém o Quickstart Jar e execute o comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]

### Iniciar o Quickstart Jar a partir da linha de comando interrompe imediatamente{#troubleshooting-java-8}

Ao iniciar o Quickstart Jar na linha de comando, o processo é interrompido imediatamente e o serviço AEM não é iniciado, com o seguinte erro:

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

Isso ocorre porque o AEM as a Cloud Service requer o Java™ SDK 11 e você está executando uma versão diferente, provavelmente o Java™ 8. Para resolver esse problema, baixe e instale o [Oracle Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=11).

Depois que o SDK do Java™ 11 do Oracle estiver instalado, verifique se essa é a versão ativa executando o comando na linha de comando:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

## Recursos adicionais

+ [Baixar o SDK do AEM](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Baixar Docker](https://www.docker.com/)
+ [Documentação do Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=pt-BR)
