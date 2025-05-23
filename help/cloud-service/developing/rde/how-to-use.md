---
title: Como usar o ambiente de desenvolvimento rápido
description: Saiba como usar o Ambiente de desenvolvimento rápido para implantar código e conteúdo de seu computador local.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 792
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 0%

---

# Como usar o ambiente de desenvolvimento rápido

Saiba **como usar** o Ambiente de desenvolvimento rápido (RDE) no AEM as a Cloud Service. Implante código e conteúdo para ciclos de desenvolvimento mais rápidos do seu código quase final no RDE, a partir do seu ambiente de desenvolvimento integrado (IDE) favorito.

Usando o [Projeto de Sites do AEM WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project), você aprenderá a implantar vários artefatos do AEM no RDE executando o comando `install` do AEM-RDE em seu IDE favorito.

- Implantação do pacote de código e conteúdo do AEM (all, ui.apps)
- Implantação do pacote OSGi e do arquivo de configuração
- Implantação das configurações do Apache e Dispatcher como um arquivo zip
- Arquivos individuais como HTL, implantação de `.content.xml` (caixa de diálogo XML)
- Revisar outros comandos RDE como `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## Pré-requisitos

Clona o projeto [WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) e o abre em seu IDE favorito para implantar os artefatos do AEM no RDE.

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

Em seguida, crie e implante-o no AEM-SDK local executando o seguinte comando maven.

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## Implantar artefatos do AEM usando o plug-in AEM-RDE

Primeiro, verifique se você tem o [último `aio` módulo CLI instalado](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools#aio-cli).

Em seguida, use o comando `aio aem:rde:install` para implantar vários artefatos do AEM. Agora que você deve

### Implantar pacotes `all` e `dispatcher`

Um ponto de partida comum é primeiro implantar os pacotes `all` e `dispatcher` executando os comandos a seguir.

```shell
# Install the 'all' content package (zip file)
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' deployment artifact (zip file)
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

Após implantações bem-sucedidas, verifique o site do WKND nos serviços de criação e publicação. Você deve ser capaz de adicionar, editar e publicar o conteúdo nas páginas do site WKND.

### Aprimorar e implantar um componente

Vamos aprimorar o `Hello World Component` e implantá-lo no RDE.

1. Abrir o arquivo XML da caixa de diálogo (`.content.xml`) da pasta `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/`
1. Adicionar o campo de texto `Description` após o campo de diálogo `Text` existente

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Abrir o arquivo `helloworld.html` da pasta `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld`
1. Renderize a propriedade `Description` após o elemento `<div>` existente da propriedade `Text`.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Verifique as alterações no AEM SDK local executando a compilação Maven ou sincronizando arquivos individuais.

1. Implante as alterações no RDE por meio do pacote `ui.apps` ou implantando a caixa de diálogo individual e os arquivos HTL:

   ```shell
   # Using 'ui.apps' package
   
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. Verifique as alterações no RDE adicionando ou editando o `Hello World Component` em uma página do site WKND.

### Examine as opções do comando `install`

No exemplo de comando de implantação de arquivo individual acima, os sinalizadores `-t` e `-p` são usados para indicar o tipo e o destino do caminho JCR, respectivamente. Vamos revisar as opções de comando `install` disponíveis executando o seguinte comando.

```shell
$ aio aem:rde:install --help
```

Os sinalizadores são autoexplicativos, o sinalizador `-s` é útil para direcionar a implantação apenas para os serviços de autoria ou publicação. Use o sinalizador `-t` ao implantar os arquivos **content-file ou content-xml** junto com o sinalizador `-p` para especificar o caminho JCR de destino no ambiente AEM RDE.

### Implantar pacote OSGi

Para saber como implantar o pacote OSGi, vamos aprimorar a classe Java™ `HelloWorldModel` e implantá-la no RDE.

1. Abrir o arquivo `HelloWorldModel.java` da pasta `core/src/main/java/com/adobe/aem/guides/wknd/core/models`
1. Atualize o método `init()` conforme abaixo:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Verifique as alterações no AEM-SDK local implantando o pacote `core` por meio do comando maven
1. Implante as alterações no RDE executando o seguinte comando

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Verifique as alterações no RDE adicionando ou editando o `Hello World Component` em uma página do site WKND.

### Implantar configuração OSGi

Você pode implantar os arquivos de configuração individuais ou concluir o pacote de configuração, por exemplo:

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>Para instalar uma configuração OSGi somente em uma instância de autor ou publicação, use o sinalizador `-s`.


### Implantar a configuração do Apache ou Dispatcher

Os arquivos de configuração do Apache ou Dispatcher **não podem ser implantados individualmente**, mas toda a estrutura de pastas do Dispatcher precisa ser implantada no formato de um arquivo ZIP.

1. Faça uma alteração desejada no arquivo de configuração do módulo `dispatcher`, para fins de demonstração, atualize o `dispatcher/src/conf.d/available_vhosts/wknd.vhost` para armazenar os arquivos `html` em cache somente por 60 segundos.

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. Verifique as alterações localmente. Consulte [Executar o Dispatcher localmente](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools) para obter mais detalhes.
1. Implante as alterações no RDE executando o seguinte comando:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verifique as alterações no RDE.

### Implantar arquivos de configuração (YAML)

Os arquivos de configuração de CDN, tarefas de manutenção, encaminhamento de logs e autenticação de API do AEM podem ser implantados no RDE usando o comando `install`. Essas configurações são gerenciadas como arquivos YAML na pasta `config` do projeto do AEM. Consulte [Configurações suportadas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/operations/config-pipeline#configurations) para obter mais detalhes.

Para saber como implantar os arquivos de configuração, vamos aprimorar o arquivo de configuração `cdn` e implantá-lo no RDE.

1. Abrir o arquivo `cdn.yaml` da pasta `config`
1. Atualizar a configuração desejada; por exemplo, atualizar o limite de taxa para 200 solicitações por segundo

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     trafficFilters:
       rules:
       #  Block client for 5m when it exceeds an average of 100 req/sec to origin on a time window of 10sec
       - name: limit-origin-requests-client-ip
         when:
           reqProperty: tier
           equals: 'publish'
         rateLimit:
           limit: 200 # updated rate limit
           window: 10
           count: fetches
           penalty: 300
           groupBy:
             - reqProperty: clientIp
         action: log
   ...
   ```

1. Implante as alterações no RDE executando o seguinte comando

   ```shell
   $ aio aem:rde:install -t env-config ./config
   ```

1. Verificar alterações no RDE


## Comandos adicionais do plug-in RDE do AEM

Vamos analisar os comandos adicionais do plug-in do AEM RDE para gerenciar e interagir com o RDE na máquina local.

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

Usando os comandos acima, seu RDE pode ser gerenciado a partir de seu IDE favorito para um ciclo de vida de desenvolvimento/implantação mais rápido.

## Próxima etapa

Saiba mais sobre o [ciclo de vida de desenvolvimento/implantação usando o RDE](./development-life-cycle.md) para fornecer recursos com velocidade.


## Recursos adicionais

[Documentação de comandos RDE](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments)

[Plug-in da CLI do Adobe I/O Runtime para interações com os ambientes de desenvolvimento AEM Rapid](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configuração do AEM Project](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup)
