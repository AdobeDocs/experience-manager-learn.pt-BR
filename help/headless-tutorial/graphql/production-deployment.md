---
title: Implantação de produção usando um serviço de publicação de AEM - Introdução AEM sem cabeçalho - GraphQL
description: Saiba mais sobre os serviços de autor e publicação do AEM e o padrão de implantação recomendado para aplicativos sem cabeçalho. Neste tutorial, aprenda a usar variáveis de ambiente para alterar dinamicamente um endpoint GraphQL com base no ambiente do público alvo. Saiba como configurar corretamente o AEM para o compartilhamento de recursos entre Origens (CORS).
sub-product: ativos
topics: headless
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
translation-type: tm+mt
source-git-commit: ce4a35f763862c6d6a42795fd5e79d9c59ff645a
workflow-type: tm+mt
source-wordcount: '2361'
ht-degree: 1%

---


# Implantação de produção com um serviço de publicação de AEM

Neste tutorial, você irá configurar um ambiente local para simular o conteúdo que está sendo distribuído de uma instância de autor para uma instância de publicação. Você também gerará a criação de produção de um aplicativo React configurado para consumir conteúdo do ambiente AEM Publish usando as APIs GraphQL. Ao longo do caminho, você aprenderá a usar variáveis de ambiente com eficiência e a atualizar as configurações de CORS AEM.

## Pré-requisitos

Este tutorial é parte de um tutorial de várias partes. Pressupõe-se que as etapas descritas nas partes anteriores foram completadas.

## Objetivos

Saiba como:

* Entenda a arquitetura de autor e publicação do AEM.
* Saiba mais sobre as práticas recomendadas para gerenciar variáveis de ambiente.
* Saiba como configurar corretamente o AEM para o compartilhamento de recursos entre Origens (CORS).

## Padrão de implantação de publicação do autor {#deployment-pattern}

Um ambiente AEM completo é composto por um Autor, Publicar e Dispatcher. O serviço Autor é onde os usuários internos criam, gerenciam e pré-visualizações de conteúdo. O serviço de publicação é considerado o ambiente &quot;ao vivo&quot; e é normalmente com o que os usuários finais interagem. O conteúdo, após ser editado e aprovado no serviço Autor, é distribuído ao serviço de Publicação.

O padrão de implantação mais comum com AEM aplicativos sem cabeçalho é ter a versão de produção do aplicativo conectada a um serviço de publicação de AEM.

![Padrão de implantação de alto nível](assets/publish-deployment/high-level-deployment.png)

O diagrama acima descreve esse padrão de implantação comum.

1. Um **Autor de conteúdo** usa o serviço de autor de AEM para criar, editar e gerenciar conteúdo.
2. O **Autor de conteúdo** e outros usuários internos podem pré-visualização o conteúdo diretamente no serviço Autor. É possível configurar uma versão de Pré-visualização do aplicativo que se conecta ao serviço Autor.
3. Depois que o conteúdo é aprovado, ele pode ser **publicado** no serviço de publicação de AEM.
4. **Os** usuários finais interagem com a versão Produção do aplicativo. O aplicativo Production se conecta ao serviço Publish e usa as APIs GraphQL para solicitar e consumir conteúdo.

O tutorial simula a implantação acima adicionando uma instância de publicação de AEM à configuração atual. Em capítulos anteriores, o aplicativo React agiu como uma pré-visualização ao se conectar diretamente à instância do autor. Uma compilação de produção do aplicativo React será implantada em um servidor Node.js estático que se conecta à nova instância Publish.

No final, três servidores locais estarão em execução:

* http://localhost:4502 - Instância do autor
* http://localhost:4503 - Instância de publicação
* http://localhost:5000 - Reagir aplicativo no modo de produção, conectando-se à instância Publicar.

## Instalar AEM SDK - Modo de publicação {#aem-sdk-publish}

Atualmente, temos uma instância em execução do SDK no modo **Author**. O SDK também pode ser iniciado no modo **Publicar** para simular um ambiente de publicação de AEM.

Um guia mais detalhado para configurar um ambiente de desenvolvimento local [pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. No sistema de arquivos local, crie uma pasta dedicada para instalar a instância Publicar, ou seja, `~/aem-sdk/publish`.
1. Copie o arquivo jar do Quickstart usado para a instância Autor em capítulos anteriores e cole-o no diretório `publish`. Como alternativa, navegue até [Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) e baixe o SDK mais recente e extraia o arquivo jar do Quickstart.
1. Renomeie o arquivo jar para `aem-publish-p4503.jar`.

   A string `publish` especifica que os start jar do Quickstart no modo de Publicação. O `p4503` especifica que o servidor Quickstart é executado na porta 4503.

1. Abra uma nova janela de terminal e navegue até a pasta que contém o arquivo jar. Instale e start a instância AEM:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Forneça uma senha de administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para evitar configurações adicionais.
1. Quando a instância AEM terminar de instalar, uma nova janela do navegador será aberta em [http://localhost:4503/content.html](http://localhost:4503/content.html)

   Espera-se que retorne uma página 404 Not Found. Esta é uma instância de AEM totalmente nova e nenhum conteúdo foi instalado.

## Instalar conteúdo de amostra e pontos finais GraphQL {#wknd-site-content-endpoints}

Assim como na instância Autor, a instância Publicar precisa ter os pontos finais GraphQL ativados e precisa de conteúdo de amostra. Em seguida, instale o Site de referência WKND na instância Publicar.

1. Baixe o pacote de AEM compilado mais recente para o site WKND: [aem-guides-wknd.all-x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Certifique-se de baixar a versão padrão compatível com AEM como Cloud Service e **not** a versão `classic`.

1. Faça logon na instância Publicar navegando diretamente para: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) com o nome de usuário `admin` e a senha `admin`.
1. Em seguida, navegue até Package Manager em [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Clique em **Carregar pacote** e escolha o pacote WKND baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.
1. Após a instalação do pacote, o site de referência WKND está disponível em [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Faça logoff como o usuário `admin` clicando no botão &quot;Sair&quot; na barra de menus.

   ![Site de referência de logout da WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   Ao contrário da instância do autor de AEM, as instâncias de publicação de AEM assumem o padrão de acesso anônimo somente leitura. Queremos simular a experiência de um usuário anônimo ao executar o aplicativo React.

## Atualize as variáveis de Ambiente para apontar a instância de publicação {#react-app-publish}

Em seguida, atualize as variáveis de ambiente usadas pelo aplicativo React para apontar para a instância Publish. O aplicativo React deve **somente** se conectar à instância Publish no modo de produção.

Em seguida, adicione um novo arquivo `.env.production.local` para simular a experiência de produção.

1. Abra o aplicativo WKND GraphQL React no IDE.

1. Abaixo de `aem-guides-wknd-graphql/react-app`, adicione um arquivo chamado `.env.production.local`.
1. Preencha `.env.production.local` com o seguinte:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Adicionar novo arquivo de variável de ambiente](assets/publish-deployment/env-production-local-file.png)

   O uso de variáveis de ambiente facilita a alternância do ponto de extremidade GraphQL entre um ambiente Autor ou Publicar sem adicionar lógica extra dentro do código do aplicativo. Mais informações sobre as variáveis de ambiente personalizadas [para React podem ser encontradas aqui](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Observe que nenhuma informação de autenticação é incluída, pois os ambientes de publicação fornecem acesso anônimo ao conteúdo por padrão.

## Implantar um servidor Nó estático {#static-server}

O aplicativo React pode ser iniciado usando o servidor webpack, mas isso é apenas para desenvolvimento. Em seguida, simule uma implantação de produção usando [serve](https://github.com/vercel/serve) para hospedar uma compilação de produção do aplicativo React usando Node.js.

1. Abra uma nova janela de terminal e navegue até o diretório `aem-guides-wknd-graphql/react-app`

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Instale [serve](https://github.com/vercel/serve) com o seguinte comando:

   ```shell
   $ npm install serve --save-dev
   ```

1. Abra o arquivo `package.json` em `react-app/package.json`. Adicione um script chamado `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   O script `serve` executa duas ações. Primeiro, uma criação de produção do aplicativo React é gerada. Segundo, o servidor Node.js é start e usa a compilação de produção.

1. Retorne ao terminal e digite o comando para start do servidor estático:

   ```shell
   $ npm run serve
   
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.111:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

1. Abra um novo navegador e navegue até [http://localhost:5000/](http://localhost:5000/). Você deve ver o aplicativo React sendo servido.

   ![Aplicativo React Servido](assets/publish-deployment/react-app-served-port5000.png)

   Observe que o query GraphQL está funcionando no home page. Inspect a solicitação **XHR** usando suas ferramentas de desenvolvedor. Observe que o POST GraphQL está na instância Publicar em `http://localhost:4503/content/graphql/global/endpoint.json`.

   No entanto, todas as imagens estão quebradas no home page!

1. Clique em uma das páginas de Detalhes da Aventura.

   ![Erro de Detalhe da Aventura](assets/publish-deployment/adventure-detail-error.png)

   Observe que um erro de GraphQL é lançado para `adventureContributor`. Nos próximos exercícios, as imagens quebradas e os problemas de `adventureContributor` são corrigidos.

## Referências absolutas de imagem {#absolute-image-references}

As imagens parecem quebradas porque o atributo `<img src` está definido para um caminho relativo e acaba apontando para o servidor estático Nó em `http://localhost:5000/`. Em vez disso, essas imagens devem apontar para a instância de publicação de AEM. Existem várias soluções possíveis para isso. Ao usar o servidor dev de webpack, o arquivo `react-app/src/setupProxy.js` configura um proxy entre o servidor de webpack e a instância do autor AEM para quaisquer solicitações de `/content`. Uma configuração proxy pode ser usada em um ambiente de produção, mas deve ser configurada no nível do servidor da Web. Por exemplo, [módulo proxy do Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

O aplicativo pode ser atualizado para incluir um URL absoluto usando a variável de ambiente `REACT_APP_HOST_URI`. Em vez disso, vamos usar um recurso AEM API do GraphQL para solicitar um URL absoluto para a imagem.

1. Pare o servidor Node.js.
1. Retorne ao IDE e abra o arquivo `Adventures.js` em `react-app/src/components/Adventures.js`.
1. Adicione a propriedade `_publishUrl` a `ImageRef` dentro de `allAdventuresQuery`:

   ```diff
   const allAdventuresQuery = `
   {
       adventureList {
       items {
           _path
           adventureTitle
           adventurePrice
           adventureTripLength
           adventurePrimaryImage {
           ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
           }
           }
       }
       }
   }
   `;
   ```

   `_publishUrl` e  `_authorUrl` são valores incorporados ao  `ImageRef` objeto para facilitar a inclusão de urls absolutas.

1. Repita as etapas acima para modificar o query usado na função `filterQuery(activity)` para incluir a propriedade `_publishUrl`.
1. Modifique o componente `AdventureItem` em `function AdventureItem(props)` para fazer referência à propriedade `_publishUrl` em vez da propriedade `_path` ao criar a tag `<img src=''>`:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Abra o arquivo `AdventureDetail.js` em `react-app/src/components/AdventureDetail.js`.
1. Repita as mesmas etapas para modificar o query GraphQL e adicionar a propriedade `_publishUrl` para o Adventure

   ```diff
    adventureByPath (_path: "${_path}") {
       item {
           _path
           adventureTitle
           adventureActivity
           adventureType
           adventurePrice
           adventureTripLength
           adventureGroupSize
           adventureDifficulty
           adventurePrice
           adventurePrimaryImage {
               ... on ImageRef {
               _path
   +           _publishUrl
               mimeType
               width
               height
               }
           }
           adventureDescription {
               html
           }
           adventureItinerary {
               html
           }
           adventureContributor {
               fullName
               occupation
               pictureReference {
                   ...on ImageRef {
                       _path
   +                   _publishUrl
                   }
               }
           }
       }
       }
   } 
   ```

1. Modifique as duas tags `<img>` para a Adventure Primary Image e a referência Imagem do contribuidor em `AdventureDetail.js`:

   ```diff
   /* AdventureDetail.js */
   ...
   <img className="adventure-detail-primaryimage"
   -       src={adventureData.adventurePrimaryImage._path} 
   +       src={adventureData.adventurePrimaryImage._publishUrl} 
           alt={adventureData.adventureTitle}/>
   ...
   pictureReference =  <img className="contributor-image" 
   -                        src={props.pictureReference._path}
   +                        src={props.pictureReference._publishUrl} 
                            alt={props.fullName} />
   ```

1. Retorne ao terminal e start o servidor estático:

   ```shell
   $ npm run serve
   ```

1. Navegue até [http://localhost:5000/](http://localhost:5000/) e observe que as imagens são exibidas e que o atributo `<img src''>` aponta para `http://localhost:4503`.

   ![Imagens quebradas corrigidas](assets/publish-deployment/broken-images-fixed.png)

## Simular publicação de conteúdo {#content-publish}

Lembre-se de que um erro de GraphQL é lançado para `adventureContributor` quando uma página de Detalhes da Aventura é solicitada. O **Contributor** Modelo de fragmento de conteúdo ainda não existe na instância Publicar. As atualizações feitas no **Adventure** Modelo de fragmento de conteúdo também não estão disponíveis na instância Publicar. Essas alterações foram feitas diretamente na instância Autor e precisam ser distribuídas para a instância Publicar.

Isso é algo a ser considerado ao implementar novas atualizações em um aplicativo que depende de atualizações em um Fragmento de conteúdo ou em um Modelo de fragmento de conteúdo.

Em seguida, permite simular a publicação de conteúdo entre as instâncias locais de Autor e Publicação.

1. Start a instância Autor (se ainda não tiver sido iniciada) e navegue até Gerenciador de pacotes em [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Baixe o pacote [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) e instale-o usando o Package Manager.

   Este pacote instala uma configuração que permite que a instância Autor publique conteúdo na instância Publicar. As etapas manuais para [esta configuração podem ser encontradas aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > Em um AEM como ambiente Cloud Service, a camada Autor é configurada automaticamente para distribuir conteúdo para a camada Publicar.

1. No menu **AEM Start**, navegue até **Ferramentas** > **Ativos** > **Modelos de fragmento de conteúdo**.

1. Clique na pasta **Site WKND**.

1. Selecione todos os três modelos e clique em **Publicar**:

   ![Publicar modelos de fragmento do conteúdo](assets/publish-deployment/publish-contentfragment-models.png)

   Uma caixa de diálogo de confirmação é exibida; clique em **Publicar**.

1. Navegue até o Fragmento de conteúdo do campo de navegação de Bali em [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Clique no botão **Publicar** na barra de menus superior.

   ![Clique no botão Publicar no Editor de fragmentos de conteúdo](assets/publish-deployment/publish-bali-content-fragment.png)

1. O assistente de publicação mostra todos os ativos dependentes que devem ser publicados. Nesse caso, o fragmento referenciado **stacey-roswells** é listado e várias imagens também são referenciadas. Os ativos referenciados são publicados junto com o fragmento.

   ![Ativos referenciados para publicar](assets/publish-deployment/referenced-assets.png)

   Clique no botão **Publicar** novamente para publicar o Fragmento de conteúdo e os ativos dependentes.

1. Retorne ao aplicativo React em execução em [http://localhost:5000/](http://localhost:5000/). Agora você pode clicar em Bali Surf Camp para ver os detalhes da aventura.

1. Volte para a instância do autor de AEM em [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e atualize o **Título** do fragmento. **Salvar e** fechar o fragmento. Em seguida, **publique** o fragmento.
1. Retorne para [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e observe as alterações publicadas.

   ![Atualização de publicação do campo de pesquisa de Bali](assets/publish-deployment/bali-surf-camp-update.png)

## Atualizar configuração de CORs

AEM é protegido por padrão e não permite que propriedades da Web não AEM façam chamadas do cliente. AEM configuração de CORS (Cross-Origem Resource Sharing, compartilhamento de recursos entre ) pode permitir que domínios específicos façam chamadas para AEM.

Em seguida, experimente a configuração do CORS da instância de publicação de AEM.

1. Retorne à janela do terminal onde o aplicativo React está sendo executado com o comando `npm run serve`:

   ```shell
   ┌────────────────────────────────────────────────────┐
   │                                                    │
   │   Serving!                                         │
   │                                                    │
   │   - Local:            http://localhost:5000        │
   │   - On Your Network:  http://192.168.86.205:5000   │
   │                                                    │
   │   Copied local address to clipboard!               │
   │                                                    │
   └────────────────────────────────────────────────────┘
   ```

   Observe que dois URLs são fornecidos. Um usando `localhost` e outro usando o endereço IP da rede local.

1. Navegue até o endereço que começa com [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). O endereço será um pouco diferente para cada computador local. Observe que há um erro de CORS ao buscar os dados. Isso ocorre porque a configuração atual do CORS só permite solicitações de `localhost`.

   ![Erro CORS](assets/publish-deployment/cors-error-not-fetched.png)

   Em seguida, atualize a configuração do CORS de publicação de AEM para permitir solicitações do endereço IP da rede.

1. Navegue até [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) e entre com o nome de usuário `admin` e a senha `admin`.

1. Navegue até [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) e localize a configuração WKND GraphQL em `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Atualize o campo **Origem** permitido para incluir o endereço IP da rede:

   ![Atualizar configuração do CORS](assets/publish-deployment/cors-update.png)

   Também é possível incluir uma expressão regular para permitir todas as solicitações de um subdomínio específico. Salve as alterações.

1. Procure **Filtro de Quem indicou Apache Sling** e reveja a configuração. A configuração **Permitir vazio** também é necessária para habilitar solicitações GraphQL de um domínio externo.

   ![Sling Referrer Filter](assets/publish-deployment/sling-referrer-filter.png)

   Eles foram configurados como parte do site de referência WKND. Você pode visualização o conjunto completo de configurações OSGi por [o repositório GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > As configurações OSGi são gerenciadas em um projeto AEM que é comprometido com o controle da origem. Um AEM Project pode ser implantado em AEM como ambientes Cloud Service usando o Cloud Manager. O [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) pode ajudar a gerar um projeto para uma implementação específica.

1. Retorne ao aplicativo React começando com [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) e observe que o aplicativo não emite mais um erro CORS.

   ![Erro de CORS corrigido](assets/publish-deployment/cors-error-corrected.png)

## Parabéns! {#congratulations}

Parabéns! Agora você simulou uma implantação de produção completa usando um ambiente de publicação de AEM. Você também aprendeu a usar a configuração do CORS no AEM.

## Outros recursos

Para obter mais detalhes sobre os Fragmentos de conteúdo e o GraphQL, consulte os seguintes recursos:

* [Delivery de conteúdo sem cabeçalho usando fragmentos de conteúdo com GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html)
* [AEM API GraphQL para uso com Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html)
* [Autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [Implantação do código em AEM como Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
