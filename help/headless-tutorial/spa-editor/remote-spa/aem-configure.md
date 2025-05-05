---
title: Configurar o AEM para o Editor de SPA e o SPA remoto
description: Um projeto AEM é necessário para definir requisitos de configuração e conteúdo compatíveis com a instalação, a fim de permitir que o editor do SPA SPA seja autor de um AEM remoto.
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7631
thumbnail: kt-7631.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
recommendations: noDisplay, noCatalog
doc-type: Tutorial
exl-id: 0bdb93c9-5070-483c-a34c-f2b348bfe5ae
duration: 297
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1230'
ht-degree: 0%

---

# Configurar o AEM para o editor do SPA

Embora a base de código do SPA seja gerenciada fora do AEM, um projeto do AEM é necessário para configurar requisitos de configuração e conteúdo de suporte. Este capítulo aborda a criação de um projeto AEM que contém as configurações necessárias:

+ Proxies dos Componentes principais do WCM no AEM
+ Proxy da página SPA remota AEM
+ Modelos de página AEM SPA remoto
+ SPA Páginas AEM remotas da linha de base
+ Subprojeto para definir mapeamentos de URL de AEM para SPA
+ Pastas de configuração do OSGi

## Baixar o projeto base no GitHub

Baixe o projeto `aem-guides-wknd-graphql` de Github.com. Ele conterá alguns arquivos de linha de base usados neste projeto.

```
$ mkdir -p ~/Code
$ git clone https://github.com/adobe/aem-guides-wknd-graphql.git
$ cd remote-spa-tutorial
```

## Criar um projeto AEM

Crie um projeto AEM no qual as configurações e o conteúdo da linha de base sejam gerenciados. Este projeto será gerado dentro da pasta `remote-spa-tutorial` do projeto `aem-guides-wknd-graphql` clonado.

_Sempre usar a versão mais recente do [Arquétipo AEM](https://github.com/adobe/aem-project-archetype)._

```
$ cd ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial
$ mvn -B archetype:generate \
 -D archetypeGroupId=com.adobe.aem \
 -D archetypeArtifactId=aem-project-archetype \
 -D archetypeVersion=39 \
 -D aemVersion=cloud \
 -D appTitle="WKND App" \
 -D appId="wknd-app" \
 -D groupId="com.adobe.aem.guides.wkndapp" \
 -D frontendModule="react"
$ mv ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/wknd-app ~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app
```

_O último comando simplesmente renomeia a pasta do projeto AEM para que fique claro que é o projeto AEM e não deve ser confundido com SPA Remoto__

Enquanto `frontendModule="react"` for especificado, o projeto `ui.frontend` não será usado para o caso de uso de SPA Remoto. O SPA é desenvolvido e gerenciado externamente para AEM e só usa o AEM como uma API de conteúdo. O sinalizador `frontendModule="react"` é necessário para o projeto incluir as dependências do Java™ do AEM `spa-project` e configurar os Modelos de página do SPA remoto.

O Arquétipo de Projeto AEM gera os seguintes elementos que são usados para configurar o AEM para integração com o SPA.

+ __Proxies de Componentes Principais de WCM do AEM__ em `ui.apps/src/.../apps/wknd-app/components`
+ AEM __Proxy de página remota SPA__ em `ui.apps/src/.../apps/wknd-app/components/remotepage`
+ __Modelos de página AEM__ em `ui.content/src/.../conf/wknd-app/settings/wcm/templates`
+ __Subprojeto para definir mapeamentos de conteúdo__ em `ui.content/src/...`
+ SPA __Páginas AEM remotas da linha de base__ em `ui.content/src/.../content/wknd-app`
+ __Pastas de configuração OSGi__ em `ui.config/src/.../apps/wknd-app/osgiconfig`

Com a geração do projeto base AEM, alguns ajustes garantem a compatibilidade do Editor de SPA com o SPA remoto.

## Remover projeto ui.frontend

Como o SPA é um SPA remoto, suponha que ele seja desenvolvido e gerenciado fora do projeto AEM. Para evitar conflitos, remova a implantação do projeto `ui.frontend`. Se o projeto `ui.frontend` não for removido, dois SPA, o SPA padrão fornecido no projeto `ui.frontend` e o SPA AEM SPA remoto, serão carregados ao mesmo tempo no Editor de.

1. Abra o projeto AEM (`~/Code/aem-guides-wknd-graphql/remote-spa-tutorial/com.adobe.aem.guides.wknd-app`) no IDE
1. Abrir a raiz `pom.xml`
1. Comente o `<module>ui.frontend</module` fora da lista `<modules>`

   ```
   <modules>
       <module>all</module>
       <module>core</module>
   
       <!-- <module>ui.frontend</module> -->
   
       <module>ui.apps</module>
       <module>ui.apps.structure</module>
       <module>ui.config</module>
       <module>ui.content</module>
       <module>it.tests</module>
       <module>dispatcher</module>
       <module>ui.tests</module>
       <module>analyse</module>
   </modules>
   ```

   O arquivo `pom.xml` deve ser semelhante a:

   ![Remover o módulo ui.frontend do pom do reator](./assets/aem-project/uifrontend-reactor-pom.png)

1. Abrir o `ui.apps/pom.xml`
1. Comente o `<dependency>` em `<artifactId>wknd-app.ui.frontend</artifactId>`

   ```
   <dependencies>
   
       <!-- Remote SPA project will provide all frontend resources
       <dependency>
           <groupId>com.adobe.aem.guides.wkndapp</groupId>
           <artifactId>wknd-app.ui.frontend</artifactId>
           <version>${project.version}</version>
           <type>zip</type>
       </dependency>
       --> 
   </dependencies>
   ```

   O arquivo `ui.apps/pom.xml` deve ser semelhante a:

   ![Remover dependência ui.frontend de ui.apps](./assets/aem-project/uifrontend-uiapps-pom.png)

Se o projeto AEM foi compilado antes dessas alterações, exclua manualmente a Biblioteca do Cliente gerada por `ui.frontend` do projeto `ui.apps` em `ui.apps/src/main/content/jcr_root/apps/wknd-app/clientlibs/clientlib-react`.

## Mapeamento de conteúdo do AEM

Para que o AEM carregue o SPA remoto no Editor de SPA, os mapeamentos entre as rotas do SPA e as Páginas do AEM usadas para abrir e criar conteúdo devem ser estabelecidos.

A importância dessa configuração é explorada posteriormente.

O mapeamento pode ser feito com o [Mapeamento do Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html#root-level-mappings-1) definido em `/etc/map`.

1. No IDE, abra o subprojeto `ui.content`
1. Navegue até `src/main/content/jcr_root`
1. Criar uma pasta `etc`
1. Em `etc`, crie uma pasta `map`
1. Em `map`, crie uma pasta `http`
1. Em `http`, crie um arquivo `.content.xml` com o conteúdo:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping">
       <localhost_any/>
   </jcr:root>
   ```

1. Em `http` , crie uma pasta `localhost_any`
1. Em `localhost_any`, crie um arquivo `.content.xml` com o conteúdo:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="localhost\\.\\d+">
       <wknd-app-routes-adventure/>
   </jcr:root>
   ```

1. Em `localhost_any` , crie uma pasta `wknd-app-routes-adventure`
1. Em `wknd-app-routes-adventure`, crie um arquivo `.content.xml` com o conteúdo:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   
   <!--
   The 'wknd-app-routes-adventure' mapping, maps requests to the SPA's adventure route 
   to it's corresponding page in AEM at /content/wknd-app/us/en/home/adventure/xxx.
   
   Note the adventure AEM pages are created directly in AEM.
   -->
   
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
       jcr:primaryType="sling:Mapping"
       sling:match="adventure:.*/([^/]+)/?$"
       sling:internalRedirect="/content/wknd-app/us/en/home/adventure/$1"/>
   ```

1. Adicione os nós de mapeamento a `ui.content/src/main/content/META-INF/vault/filter.xml` para eles incluídos no pacote AEM.

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <workspaceFilter version="1.0">
       <filter root="/conf/wknd-app" mode="merge"/>
       <filter root="/content/wknd-app" mode="merge"/>
       <filter root="/content/dam/wknd-app/asset.jpg" mode="merge"/>
       <filter root="/content/experience-fragments/wknd-app" mode="merge"/>
   
       <!-- Add the Sling Mapping rules for the WKND App -->
       <filter root="/etc/map" mode="merge"/>
   </workspaceFilter>
   ```

A estrutura de pastas e os arquivos `.context.xml` devem ter a seguinte aparência:

![Mapeamento do Sling](./assets/aem-project/sling-mapping.png)

O arquivo `filter.xml` deve ser semelhante a:

![Mapeamento do Sling](./assets/aem-project/sling-mapping-filter.png)

Agora, quando o projeto AEM é implantado, essas configurações são incluídas automaticamente.

O AEM dos efeitos do Mapeamento do Sling em execução em `http` e `localhost`, portanto, somente oferece suporte ao desenvolvimento local. Ao implantar no AEM as a Cloud Service, mapeamentos Sling semelhantes devem ser adicionados para direcionar `https` e os domínios apropriados do AEM as a Cloud Service. Para obter mais informações, consulte a [documentação de Mapeamento do Sling](https://sling.apache.org/documentation/the-sling-engine/mappings-for-resource-resolution.html).

## Políticas de segurança do Compartilhamento de recursos entre origens

Em seguida, configure o AEM para proteger o conteúdo para que somente esse SPA possa acessar o conteúdo do AEM. Configurar o [Compartilhamento de Recursos entre Origens no AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/develop-for-cross-origin-resource-sharing.html?lang=pt-BR).

1. No IDE, abra o subprojeto Maven `ui.config`
1. Navegar `src/main/content/jcr_root/apps/wknd-app/osgiconfig/config`
1. Criar um arquivo chamado `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json`
1. Adicione o seguinte ao arquivo:

   ```
   {
       "supportscredentials":true,
       "exposedheaders":[
           ""
       ],
       "supportedmethods":[
           "GET",
           "HEAD",
           "POST",
           "OPTIONS"
       ],
       "alloworigin":[
           "https://external-hosted-app", "localhost:3000"
       ],
       "maxage:Integer":1800,
       "alloworiginregexp":[
           ".*"
       ],
       "allowedpaths":[
           ".*"
       ],
       "supportedheaders":[
           "Origin",
           "Accept",
           "X-Requested-With",
           "Content-Type",
           "Access-Control-Request-Method",
           "Access-Control-Request-Headers",
           "authorization"
       ]
   }
   ```

O arquivo `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-app_remote-spa.cfg.json` deve ser semelhante a:

![Configuração CORS do editor SPA](./assets/aem-project/cors-configuration.png)

Os principais elementos de configuração são:

+ `alloworigin` especifica quais hosts têm permissão para recuperar conteúdo do AEM.
   + `localhost:3000` é adicionado para oferecer suporte ao SPA executado localmente
   + `https://external-hosted-app` atua como um espaço reservado a ser substituído pelo domínio em que o SPA Remoto está hospedado.
+ `allowedpaths` especifique quais caminhos no AEM são cobertos por esta configuração do CORS. O padrão permite o acesso a todo o conteúdo no AEM, no entanto, ele pode ser controlado apenas para os caminhos específicos que o SPA pode acessar, por exemplo: `/content/wknd-app`.

## Definir página AEM como modelo de página SPA remota

O Arquétipo de Projeto AEM gera um projeto preparado para a integração do AEM com um SPA remoto, mas requer um pequeno, mas importante ajuste na estrutura da página do AEM gerada automaticamente. A página AEM gerada automaticamente deve ter seu tipo alterado para __página SPA remota__, em vez de uma __página SPA__.

1. No IDE, abra o subprojeto `ui.content`
1. Abrir para `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml`
1. Atualizar este `.content.xml` arquivo com:

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
           jcr:primaryType="cq:Page">
       <jcr:content
           cq:template="/conf/wknd-app/settings/wcm/templates/spa-remote-page"
           jcr:primaryType="cq:PageContent"
           jcr:title="WKND App Home Page"
           sling:resourceType="wknd-app/components/remotepage">
           <root
               jcr:primaryType="nt:unstructured"
               sling:resourceType="wcm/foundation/components/responsivegrid">
               <responsivegrid
                   jcr:primaryType="nt:unstructured"
                   sling:resourceType="wcm/foundation/components/responsivegrid">
                   <text
                       jcr:primaryType="nt:unstructured"
                       sling:resourceType="wknd-app/components/text"
                       text="&lt;p>Hello World!&lt;/p>"
                       textIsRich="true">
                       <cq:responsive jcr:primaryType="nt:unstructured"/>
                   </text>
               </responsivegrid>
           </root>
       </jcr:content>
   </jcr:root>
   ```

As alterações principais são atualizações no nó `jcr:content`:

+ `cq:template` a `/conf/wknd-app/settings/wcm/templates/spa-remote-page`
+ `sling:resourceType` a `wknd-app/components/remotepage`

O arquivo `src/main/content/jcr_root/content/wknd-app/us/en/home/.content.xml` deve ser semelhante a:

![Atualizações de .content.xml na página inicial](./assets/aem-project/home-content-xml.png)

Essas alterações permitem que esta página, que atua como a raiz do SPA no AEM, carregue o SPA remoto no editor do SPA.

>[!NOTE]
>
>Se este projeto foi implantado anteriormente no AEM, certifique-se de excluir a página AEM como __Sites > Aplicativo WKND > us > en > Página Inicial do Aplicativo WKND__, pois o projeto `ui.content` está definido como __mesclar__ nós, em vez de __atualizar__.

Esta página também pode ser removida e recriada como uma Página do SPA Remoto no próprio AEM. No entanto, como esta página é criada automaticamente no projeto `ui.content`, é melhor atualizá-la na base de código.

## Implantar o projeto AEM no AEM SDK

1. Verifique se o serviço de Autor do AEM está em execução na porta 4502
1. Na linha de comando, navegue até a raiz do projeto Maven para AEM
1. Use o Maven para implantar o projeto no serviço de autor do SDK do AEM local

   ```
   $ mvn clean install -PautoInstallSinglePackage
   ```

   ![mvn limpar instalação -PautoInstallSinglePackage](./assets/aem-project/mvn-install.png)

## Configurar a página raiz do AEM

Com o Projeto AEM implantado, há um último passo para preparar o Editor de SPA para carregar nosso SPA remoto. No AEM, marque a página AEM que corresponde à raiz do SPA,`/content/wknd-app/us/en/home`, gerada pelo Arquétipo de Projeto AEM.

1. Faça logon no AEM Author
1. Navegue até __Sites > Aplicativo WKND > us > en__
1. Selecione a __Página inicial do aplicativo WKND__ e toque em __Propriedades__

   ![Página Inicial do Aplicativo WKND - Propriedades](./assets/aem-content/edit-home-properties.png)

1. Navegue até a guia __SPA__
1. Preencha a __Configuração do SPA Remoto__
   + __URL do Host SPA__: `http://localhost:3000`
      + O URL para a raiz do SPA remoto

   ![Página Inicial do Aplicativo WKND - Configuração do SPA Remoto](./assets/aem-content/remote-spa-configuration.png)

1. Toque em __Salvar e fechar__

Lembre-se de que alteramos o tipo desta página para __Página do SPA Remoto__, que é o que nos permite ver a guia __SPA__ em suas __Propriedades da Página__.

Essa configuração só deve ser definida na página do AEM que corresponde à raiz do SPA. Todas as páginas AEM abaixo dessa página herdam o valor.

## Parabéns

Agora você preparou configurações do AEM e as implantou no autor local do AEM! Agora você sabe como:

+ Remova o SPA gerado pelo Arquétipo de Projeto AEM, comentando as dependências em `ui.frontend`
+ Adicionar mapeamentos Sling ao AEM que mapeiam as rotas SPA para recursos no AEM
+ Configurar políticas de segurança de compartilhamento de recursos entre origens do AEM que permitem que o SPA remoto consuma conteúdo do AEM
+ Implantar o projeto AEM no serviço de autor do SDK do AEM local
+ Marcar uma página AEM como a raiz de SPA remota usando a propriedade de página do URL do host do SPA

## Próximas etapas

Com o AEM configurado, podemos nos concentrar em [bootstrapping do SPA remoto](./spa-bootstrap.md) com suporte para áreas editáveis usando o Editor de AEM SPA!
