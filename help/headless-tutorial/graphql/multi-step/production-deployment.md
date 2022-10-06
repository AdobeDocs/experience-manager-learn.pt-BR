---
title: Implantação de produção usando um serviço de publicação do AEM - Introdução AEM headless - GraphQL
description: Saiba mais sobre os serviços de Autor e Publicação do AEM e o padrão de implantação recomendado para aplicativos sem cabeçalho. Neste tutorial, aprenda a usar variáveis de ambiente para alterar dinamicamente um ponto de extremidade GraphQL com base no ambiente de destino. Saiba como configurar corretamente o AEM para o CORS (Cross-Origin resource sharing).
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
kt: 7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2357'
ht-degree: 8%

---

# Implantação de produção com um serviço de publicação do AEM

Neste tutorial, você definirá um ambiente local para simular o conteúdo que está sendo distribuído de uma instância de autor para uma instância de publicação. Você também gerará a criação de produção de um Aplicativo React configurado para consumir conteúdo do ambiente AEM Publish usando as APIs GraphQL. Ao longo do caminho, você aprenderá a usar variáveis de ambiente de maneira eficaz e a atualizar as configurações AEM CORS.

## Pré-requisitos

Este tutorial é parte de um tutorial de várias partes. Pressupõe-se que as etapas descritas nas partes anteriores foram concluídas.

## Objetivos

Saiba como:

* Entenda a arquitetura Autor e publicação do AEM.
* Conheça as práticas recomendadas para gerenciar variáveis de ambiente.
* Saiba como configurar corretamente o AEM para o CORS (Cross-Origin resource sharing).

## Padrão de implantação de publicação do autor {#deployment-pattern}

Um ambiente AEM completo é composto de um Autor, Publicação e Dispatcher. O serviço do Autor é onde os usuários internos criam, gerenciam e visualizam conteúdo. O serviço de Publicação é considerado o ambiente &quot;Ao vivo&quot; e normalmente é com o que os usuários finais interagem. O conteúdo, após ser editado e aprovado no serviço do Autor, é distribuído ao serviço de Publicação.

O padrão de implantação mais comum com aplicativos headless do AEM é ter uma versão de produção do aplicativo conectada a um serviço de publicação do AEM.

![Padrão de implantação de alto nível](assets/publish-deployment/high-level-deployment.png)

O diagrama acima descreve esse padrão de implantação comum.

1. A **Autor do conteúdo** O usa o serviço de criação AEM para criar, editar e gerenciar conteúdo.
2. O **Autor de conteúdo** e outros usuários internos podem visualizar o conteúdo diretamente no serviço do Autor. É possível configurar uma versão de Visualização do aplicativo que se conecta ao serviço de Autor.
3. Depois que o conteúdo for aprovado, ele poderá ser **publicado** ao serviço de publicação do AEM.
4. **Os usuários finais interagem com a versão de Produção do aplicativo.** O aplicativo de Produção se conecta ao serviço de Publicação e usa as APIs GraphQL para solicitar e consumir conteúdo.

O tutorial simula a implantação acima adicionando uma instância de publicação do AEM à configuração atual. Em capítulos anteriores, o Aplicativo de reação agiu como uma visualização ao se conectar diretamente à instância do autor. Uma build de produção do aplicativo React é implantada em um servidor Node.js estático que se conecta à nova instância de publicação.

No final, três servidores locais estão em execução:

* http://localhost:4502 - Instância do autor
* http://localhost:4503 - Instância de publicação
* http://localhost:5000 - React App em modo de produção, conectando à instância de publicação.

## Instalar AEM SDK - Modo de publicação {#aem-sdk-publish}

Atualmente, temos uma instância em execução do SDK em **Autor** modo. O SDK também pode ser iniciado em **Publicar** para simular um ambiente de publicação do AEM.

Um guia mais detalhado para configurar um ambiente de desenvolvimento local [pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. No sistema de arquivos local, crie uma pasta dedicada para instalar a instância de publicação, ou seja, nomeada `~/aem-sdk/publish`.
1. Copie o arquivo jar do Quickstart usado para a instância do Autor em capítulos anteriores e cole-o no `publish` diretório. Como alternativa, navegue até a [Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) e baixe o SDK mais recente e extraia o arquivo jar do Quickstart.
1. Renomeie o arquivo jar para `aem-publish-p4503.jar`.

   O `publish` A string especifica que o jar do Quickstart começa no modo de Publicação. O `p4503` especifica que o servidor Quickstart é executado na porta 4503.

1. Abra uma nova janela do terminal e navegue até a pasta que contém o arquivo jar. Instale e inicie a instância de AEM:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Forneça uma senha de administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para evitar configurações adicionais.
1. Quando a instância de AEM terminar de instalar, uma nova janela do navegador será aberta em [http://localhost:4503/content.html](http://localhost:4503/content.html)

   É esperado que retorne uma página 404 Not Found . Esta é uma instância de AEM totalmente nova e nenhum conteúdo foi instalado.

## Instalar conteúdo de amostra e pontos de extremidade GraphQL {#wknd-site-content-endpoints}

Assim como na instância Autor, a instância de Publicação precisa ter os pontos de extremidade GraphQL ativados e precisa de conteúdo de amostra. Em seguida, instale o site de referência WKND na instância de publicação.

1. Baixe o pacote de AEM compilado mais recente para o site WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Faça o download da versão padrão compatível com AEM as a Cloud Service e **not** o `classic` versão.

1. Faça logon na instância de publicação navegando diretamente para: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) com o nome de usuário `admin` e senha `admin`.
1. Em seguida, navegue até o Gerenciador de pacotes em [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Clique em **Fazer upload do pacote** e escolha o pacote WKND baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.
1. Depois de instalar o pacote, o site de referência WKND agora está disponível em [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Fazer logoff como o `admin` ao clicar no botão &quot;Sair&quot; na barra de menus.

   ![Site de referência de logout da WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   Diferentemente da instância do autor do AEM, as instâncias de Publicação do AEM assumem o padrão de acesso anônimo somente leitura. Queremos simular a experiência de um usuário anônimo ao executar o aplicativo React.

## Atualizar variáveis de Ambiente para apontar para a instância de publicação {#react-app-publish}

Em seguida, atualize as variáveis de ambiente usadas pelo aplicativo React para apontar para a instância de publicação. O aplicativo React deve **only** conecte-se à instância Publish no modo de produção.

Em seguida, adicione um novo arquivo `.env.production.local` para simular a experiência de produção.

1. Abra o aplicativo WKND GraphQL React no IDE.

1. Beneath `aem-guides-wknd-graphql/react-app`, adicione um arquivo com o nome `.env.production.local`.
1. Preencher `.env.production.local` com o seguinte:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Adicionar novo arquivo de variável de ambiente](assets/publish-deployment/env-production-local-file.png)

   O uso de variáveis de ambiente facilita a alternância do ponto de extremidade GraphQL entre um ambiente de Autor ou Publicação sem adicionar lógica extra dentro do código do aplicativo. Mais informações sobre [variáveis de ambiente personalizadas para o React podem ser encontradas aqui](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Observe que nenhuma informação de autenticação é incluída, pois os ambientes de Publicação fornecem acesso anônimo ao conteúdo por padrão.

## Implantar um servidor Nó estático {#static-server}

O aplicativo React pode ser iniciado usando o servidor webpack, mas isso é somente para desenvolvimento. Em seguida, simule uma implantação de produção usando [serve](https://github.com/vercel/serve) para hospedar uma build de produção do aplicativo React usando Node.js.

1. Abra uma nova janela de terminal e navegue até a `aem-guides-wknd-graphql/react-app` diretory

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Instalar [serve](https://github.com/vercel/serve) com o seguinte comando:

   ```shell
   $ npm install serve --save-dev
   ```

1. Abra o arquivo `package.json` at `react-app/package.json`. Adicionar um script chamado `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   O `serve` O script executa duas ações. Primeiro, uma criação de produção do aplicativo React é gerada. Segundo, o servidor Node.js é iniciado e usa a build de produção.

1. Retorne ao terminal e insira o comando para iniciar o servidor estático:

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

1. Abra um novo navegador e acesse [http://localhost:5000/](http://localhost:5000/). Você deve ver o aplicativo React sendo exibido.

   ![Aplicativo React Servido](assets/publish-deployment/react-app-served-port5000.png)

   Observe que o query GraphQL está funcionando na página inicial. A Inspect **XHR** solicite usando as ferramentas do desenvolvedor. Observe que o POST GraphQL está disponível para a instância de publicação em `http://localhost:4503/content/graphql/global/endpoint.json`.

   No entanto, todas as imagens estão quebradas na página inicial!

1. Clique em uma das páginas Detalhes da Aventura.

   ![Erro de Detalhe da Aventura](assets/publish-deployment/adventure-detail-error.png)

   Observe que um erro GraphQL é lançado para `adventureContributor`. Nos próximos exercícios, as imagens quebradas e as `adventureContributor` os problemas são corrigidos.

## Referências de imagem absoluta {#absolute-image-references}

As imagens aparecem quebradas porque a variável `<img src` está definido como um caminho relativo e acaba apontando para o servidor estático do Nó em `http://localhost:5000/`. Em vez disso, essas imagens devem apontar para a instância de publicação do AEM. Existem várias soluções possíveis para isso. Ao usar o servidor de desenvolvimento do webpack, o arquivo `react-app/src/setupProxy.js` configure um proxy entre o servidor do webpack e a instância do autor do AEM para qualquer solicitação de `/content`. Uma configuração de proxy pode ser usada em um ambiente de produção, mas deve ser configurada no nível do servidor da Web. Por exemplo, [Módulo proxy do Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

O aplicativo pode ser atualizado para incluir um URL absoluto usando o `REACT_APP_HOST_URI` variável de ambiente. Em vez disso, vamos usar um recurso de AEM API GraphQL para solicitar um URL absoluto para a imagem.

1. Pare o servidor Node.js.
1. Retorne ao IDE e abra o arquivo `Adventures.js` at `react-app/src/components/Adventures.js`.
1. Adicione o `_publishUrl` para a `ImageRef` no `allAdventuresQuery`:

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

   `_publishUrl` e `_authorUrl` são valores incorporados ao `ImageRef` para facilitar a inclusão de urls absolutos.

1. Repita as etapas acima para modificar a consulta usada no `filterQuery(activity)` para incluir a função `_publishUrl` propriedade.
1. Modifique o `AdventureItem` componente em `function AdventureItem(props)` para fazer referência à `_publishUrl` em vez de `_path` ao construir a `<img src=''>` tag:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Abra o arquivo `AdventureDetail.js` at `react-app/src/components/AdventureDetail.js`.
1. Repita as mesmas etapas para modificar a consulta GraphQL e adicionar o `_publishUrl` propriedade da Aventura

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

1. Modifique os dois `<img>` tags para a Imagem principal da Aventura e a referência Imagem do colaborador em `AdventureDetail.js`:

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

1. Retorne ao terminal e inicie o servidor estático:

   ```shell
   $ npm run serve
   ```

1. Navegar para [http://localhost:5000/](http://localhost:5000/) e observe que as imagens são exibidas e que a variável `<img src''>` pontos de atributo para `http://localhost:4503`.

   ![Imagens quebradas fixas](assets/publish-deployment/broken-images-fixed.png)

## Simular publicação de conteúdo {#content-publish}

Lembre-se de que um erro GraphQL é lançado para `adventureContributor` quando uma página Detalhes da Aventura é solicitada. O **Colaborador** O Modelo de fragmento de conteúdo ainda não existe na instância de publicação. Atualizações feitas ao **Aventura** O Modelo de fragmento de conteúdo também não está disponível na instância de publicação. Essas alterações foram feitas diretamente na instância de autor e precisam ser distribuídas para a instância de publicação.

Isso é algo a ser considerado ao lançar novas atualizações em um aplicativo que depende de atualizações em um Fragmento de conteúdo ou um Modelo de fragmento de conteúdo.

Em seguida, permite simular a publicação de conteúdo entre as instâncias de Autor e Publicação local.

1. Inicie a instância do autor (se ainda não tiver sido iniciada) e navegue até o Gerenciador de pacotes em [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Baixe o pacote [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) e instale-o usando o Gerenciador de pacotes.

   Esse pacote instala uma configuração que permite à instância do autor publicar conteúdo na instância de publicação. Etapas manuais para [esta configuração pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > Em um ambiente AEM as a Cloud Service, a camada Autor é configurada automaticamente para distribuir conteúdo para a camada Publicar .

1. No **Início do AEM** , navegue até **Ferramentas** > **Ativos** > **Modelos de fragmentos do conteúdo**.

1. Clique no botão **Site WKND** pasta.

1. Selecione todos os três modelos e clique em **Publicar**:

   ![Publicar modelos de fragmento do conteúdo](assets/publish-deployment/publish-contentfragment-models.png)

   Uma caixa de diálogo de confirmação é exibida, clique em **Publicar**.

1. Navegue até o Fragmento de conteúdo da Campanha de Navegação em Bali em [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Clique no botão **Publicar** na barra de menu superior.

   ![Clique no botão Publicar no Editor de fragmento de conteúdo](assets/publish-deployment/publish-bali-content-fragment.png)

1. O Assistente de publicação mostra todos os ativos dependentes que devem ser publicados. Nesse caso, o fragmento referenciado **rochedos de ferro** está listada e várias imagens também são referenciadas. Os ativos referenciados são publicados junto com o fragmento.

   ![Ativos referenciados para publicar](assets/publish-deployment/referenced-assets.png)

   Clique no botão **Publicar** novamente para publicar o Fragmento de conteúdo e os ativos dependentes.

1. Retorne ao aplicativo React em execução em [http://localhost:5000/](http://localhost:5000/). Agora você pode clicar em Bali Surf Camp para ver os detalhes da aventura.

1. Volte para a instância do autor do AEM em [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e atualize o **Título** do fragmento. **Salvar e fechar** o fragmento. Então **publicar** o fragmento.
1. Retornar para [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e observe as alterações publicadas.

   ![Atualização de publicação do Camp Bali Surf](assets/publish-deployment/bali-surf-camp-update.png)

## Atualizar configuração de CORs

AEM é seguro por padrão e não permite que propriedades da Web que não sejam AEM façam chamadas do lado do cliente. AEM configuração de CORS (Cross-Origin Resource Sharing, Compartilhamento de recursos de várias origens) pode permitir que domínios específicos façam chamadas para AEM.

Em seguida, experimente a configuração do CORS da instância de publicação do AEM.

1. Retorne à janela do terminal onde o Aplicativo de Reação está sendo executado com o comando `npm run serve`:

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

1. Navegue até o endereço que começa com [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). O endereço é um pouco diferente para cada computador local. Observe que há um erro de CORS ao buscar os dados. Isso ocorre porque a configuração atual do CORS permite somente solicitações de `localhost`.

   ![Erro CORS](assets/publish-deployment/cors-error-not-fetched.png)

   Em seguida, atualize a configuração do CORS de publicação do AEM para permitir solicitações do endereço IP de rede.

1. Navegar para [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) e faça logon com o nome de usuário `admin` e senha `admin`.

1. Navegar para [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) e encontre a configuração GraphQL da WKND em `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Atualize o **Origens permitidas** para incluir o endereço IP da rede:

   ![Atualizar configuração do CORS](assets/publish-deployment/cors-update.png)

   Também é possível incluir uma expressão regular para permitir todas as solicitações de um subdomínio específico. Salve as alterações.

1. Procurar por **Filtro de referenciador do Apache Sling** e revise a configuração. O **Permitir vazio** também é necessária a configuração para ativar solicitações GraphQL de um domínio externo.

   ![Sling Referrer Filter](assets/publish-deployment/sling-referrer-filter.png)

   Eles foram configurados como parte do site de referência WKND. Você pode visualizar o conjunto completo de configurações do OSGi por meio de [o repositório GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > As configurações de OSGi são gerenciadas em um projeto AEM que é comprometido com o controle de origem. Um AEM Project pode ser implantado em ambientes AEM como Cloud Service usando o Cloud Manager. O [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) pode ajudar a gerar um projeto para uma implementação específica.

1. Retorne ao aplicativo React a partir de [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) e observe que o aplicativo não gera mais um erro de CORS.

   ![Erro CORS corrigido](assets/publish-deployment/cors-error-corrected.png)

## Parabéns.  {#congratulations}

Parabéns. Agora você simulou uma implantação de produção completa usando um ambiente de publicação do AEM. Você também aprendeu a usar a configuração do CORS no AEM.

## Outros recursos

Para obter mais detalhes sobre Fragmentos de conteúdo e GraphQL, consulte os seguintes recursos:

* [Entrega de conteúdo headless usando fragmentos de conteúdo com GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=pt-BR)
* [API GraphQL do AEM para uso com Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=pt-BR)
* [Autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [Implantação do código em AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
