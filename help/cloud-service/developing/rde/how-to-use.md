---
title: Como usar o ambiente de desenvolvimento rápido
description: Saiba como usar o Ambiente de desenvolvimento rápido para implantar código e conteúdo do computador local.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 0%

---


# Como usar o ambiente de desenvolvimento rápido

Saiba mais **como usar** Ambiente de desenvolvimento rápido (RDE) em AEM as a Cloud Service. Implante código e conteúdo para ciclos de desenvolvimento mais rápidos do seu código quase final no RDE, do seu ambiente de desenvolvimento integrado (IDE) favorito.

Usando [AEM Projeto de Sites WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) você aprenderá a implantar vários artefatos AEM no RDE executando o AEM-RDE `install` do seu IDE favorito.

- Implantação do código AEM e do pacote de conteúdo (todos, ui.apps)
- Implantação de pacote e arquivo de configuração do OSGi
- O Apache e o Dispatcher configuram a implantação como um arquivo zip
- Arquivos individuais, como HTL, `.content.xml` Implantação (XML da caixa de diálogo)
- Revise outros comandos RDE como `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## Pré-requisitos

Clonar o [Sites WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) e abra-o no IDE favorito para implantar os artefatos de AEM no RDE.

    &quot;shell
    $ clone git git@github.com:adobe/aem-guides-wknd.git
    &quot;

Em seguida, crie e implante-o no AEM-SDK local executando o seguinte comando maven.

    &quot;
    $ cd aem-guides-wknd/
    $ mvn clean install -PautoInstallSinglePackage
    &quot;

## Implantar artefatos AEM usando o plug-in AEM-RDE

Usar o `aem:rde:install` , vamos implantar vários artefatos AEM.

### Implantar `all` e `dispatcher` pacotes

Um ponto de partida comum é implantar primeiro a variável `all` e `dispatcher` pacotes executando os seguintes comandos.

    &quot;shell
    # Instale o pacote &#39;all&#39;
    $ aio aem:rde:instalar all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip
    
    # Instalar o zip &#39;dispatcher&#39;
    $ aio aem:rde:instale o dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
    &quot;

Após implantações bem-sucedidas, verifique o site WKND nos serviços de autor e publicação. Você deve ser capaz de adicionar e editar o conteúdo nas páginas do site da WKND e publicá-lo.

### Aprimorar e implantar um componente

Vamos aprimorar o `Hello World Component` e implantá-lo no RDE.

1. Abra o XML da caixa de diálogo (`.content.xml`) arquivo de `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` pasta
1. Adicione o `Description` campo de texto após o `Text` campo de diálogo

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. Abra o `helloworld.html` arquivo de `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` pasta
1. Renderize o `Description` após a `<div>` do `Text` propriedade.

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Verifique as alterações no AEM-SDK local executando a build maven ou sincronizando arquivos individuais.

1. Implantar as alterações no RDE via `ui.apps` ou implantando os arquivos individuais Dialog e HTL.

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

### Revise o `install` opções de comando

No exemplo de comando de implantação de arquivo individual acima, a variável `-t` e `-p` os sinalizadores são usados para indicar o tipo e o destino do caminho JCR, respectivamente. Vamos revisar a `install` executando o seguinte comando.

    &quot;shell
    $ aio aem:rde:instalar —help
    &quot;

As bandeiras são autoexplicativas, as `-s` O sinalizador é útil para direcionar a implantação apenas para os serviços de criação ou publicação. Use o `-t` sinalizador ao implantar o **content-file ou content-xml** arquivos junto com a `-p` sinalizador para especificar o caminho JCR de destino no ambiente RDE AEM.

### Implantar pacote OSGi

Para saber como implantar o pacote OSGi, vamos aprimorar o `HelloWorldModel` Classe Java™ e implante-a no RDE.

1. Abra o `HelloWorldModel.java` arquivo de `core/src/main/java/com/adobe/aem/guides/wknd/core/models` pasta
1. Atualize o `init()` como abaixo:

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Verifique as alterações no AEM-SDK local ao implantar a variável `core` pacote via comando maven
1. Implante as alterações no RDE executando o seguinte comando

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. Verifique as alterações no RDE adicionando ou editando o `Hello World Component` em uma página do site WKND.

### Implantar configuração do OSGi

Você pode implantar os arquivos de configuração individuais ou concluir o pacote de configuração, por exemplo:

    &quot;shell
    # Implantar arquivo de configuração individual
    $ aio aem:rde:instalar ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.fatory.config~wknd.cfg.json
    
    # Ou implante o pacote de configuração completo
    $ cd ui.config
    $ mvn clean package
    $ aio aem:rde:instale o target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
    &quot;

>[!TIP]
>
>Para instalar uma configuração OSGi somente em uma instância de autor ou publicação, use o `-s` sinalizador.


### Implantar a configuração do Apache ou Dispatcher

Os arquivos de configuração do Apache ou Dispatcher **não pode ser implantado individualmente**, mas toda a estrutura de pastas do Dispatcher precisa ser implantada no formato de um arquivo ZIP.

1. Faça a alteração desejada no arquivo de configuração do `dispatcher` para fins de demonstração, atualize o `dispatcher/src/conf.d/available_vhosts/wknd.vhost` para armazenar em cache o `html` arquivos somente por 60 segundos.

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

1. Verifique as alterações localmente, consulte [Executar o Dispatcher localmente](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) para obter mais detalhes.
1. Implante as alterações no RDE executando o seguinte comando:

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. Verificar alterações no RDE

## Comandos adicionais do plug-in AEM RDE

Vamos analisar os comandos adicionais do plug-in AEM RDE para gerenciar e interagir com o RDE do computador local.

    &quot;shell
    $ aio aem:rde —help
    Interaja com ambientes de desenvolvimento Rapid.
    
    USO
    $ aio aem rde COMMAND
    
    COMANDOS
    aem rde delete Excluir pacotes e configurações do código atual.
    histórico do aem rde Obtenha uma lista das atualizações feitas no rde atual.
    pacotes de instalação/atualização do aem rde, configurações e pacotes de conteúdo.
    aem rde redefinir Redefinir o RDE
    reinicialização do aem rde Reinicie o autor e publique um RDE
    status do aem rde Obtenha uma lista dos pacotes e configurações implantados no rde atual.
    &quot;

Usando os comandos acima, seu RDE pode ser gerenciado no IDE favorito para um ciclo de vida de desenvolvimento/implantação mais rápido.

## Próxima etapa

Saiba mais sobre o [ciclo de vida de desenvolvimento/implantação usando RDE](./development-life-cycle.md) para fornecer recursos com velocidade.


## Recursos adicionais

[Documentação de comandos RDE](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Plug-in Adobe I/O Runtime CLI para interações com AEM ambientes de desenvolvimento rápido](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Configuração do projeto AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html)
