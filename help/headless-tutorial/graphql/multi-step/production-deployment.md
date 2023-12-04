---
title: Implantação de produção usando um serviço de publicação do AEM - Introdução ao AEM Headless - GraphQL
description: Saiba mais sobre os serviços de Autor e Publicação do AEM e o padrão de implantação recomendado para aplicativos headless. Neste tutorial, aprenda a usar variáveis de ambiente para alterar dinamicamente um endpoint do GraphQL com base no ambiente de destino. Saiba como configurar corretamente o AEM para CORS (Cross-Origin Resource Sharing, compartilhamento de recursos entre origens).
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
mini-toc-levels: 1
jira: KT-7131
thumbnail: KT-7131.jpg
exl-id: 8c8b2620-6bc3-4a21-8d8d-8e45a6e9fc70
duration: 655
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2137'
ht-degree: 5%

---

# Implantação de produção com um serviço de publicação do AEM

Neste tutorial, você configurará um ambiente local para simular o conteúdo que está sendo distribuído de uma instância de Autor para uma instância de Publicação. Você também gerará um build de produção de um aplicativo React configurado para consumir conteúdo do ambiente de publicação AEM usando as APIs do GraphQL. Ao longo do caminho, você aprenderá a usar variáveis de ambiente de maneira eficaz e a atualizar as configurações do CORS do AEM.

## Pré-requisitos

Este tutorial faz parte de um tutorial com várias partes. Pressupõe-se que as etapas descritas nas partes anteriores foram concluídas.

## Objetivos

Saiba como:

* Entenda a arquitetura de Autor e Publicação do AEM.
* Conheça as práticas recomendadas para gerenciar variáveis de ambiente.
* Saiba como configurar corretamente o AEM para CORS (Cross-Origin Resource Sharing, compartilhamento de recursos entre origens).

## Padrão de implantação de publicação do autor {#deployment-pattern}

Um ambiente do AEM completo é composto de um Autor, Publicação e Dispatcher. O serviço do Autor é onde os usuários internos criam, gerenciam e visualizam conteúdo. O serviço de Publicação é considerado o ambiente &quot;ativo&quot; e é, normalmente, com o que os usuários finais interagem. O conteúdo, após ser editado e aprovado no serviço do Autor, é distribuído ao serviço de Publicação.

O padrão de implantação mais comum com aplicativos headless do AEM é ter uma versão de produção do aplicativo conectada a um serviço de publicação do AEM.

![Padrão de implantação de alto nível](assets/publish-deployment/high-level-deployment.png)

O diagrama acima descreve esse padrão de implantação comum.

1. A **Autor de conteúdo** O usa o serviço do autor do AEM para criar, editar e gerenciar conteúdo.
2. O **Autor de conteúdo** e outros usuários internos podem visualizar o conteúdo diretamente no serviço do Autor. É possível configurar uma versão de Visualização do aplicativo que se conecta ao serviço de Autor.
3. Depois que o conteúdo é aprovado, ele pode ser **publicado** ao serviço de publicação do AEM.
4. **Usuários finais** interagir com a versão de Produção do aplicativo. O aplicativo de Produção se conecta ao serviço de Publicação e usa as APIs do GraphQL para solicitar e consumir conteúdo.

O tutorial simula a implantação acima adicionando uma instância de publicação do AEM à configuração atual. Nos capítulos anteriores, o aplicativo React atuava como pré-visualização ao se conectar diretamente à instância do Autor. Uma build de produção do aplicativo React é implantada em um servidor Node.js estático que se conecta à nova instância de publicação.

No final, três servidores locais estão sendo executados:

* http://localhost:4502 - Instância do autor
* http://localhost:4503 - Publicar instância
* http://localhost:5000 - Aplicativo React no modo de produção, conectando-se à instância de publicação.

## Instalar o SDK do AEM - Modo de publicação {#aem-sdk-publish}

Atualmente, temos uma instância do SDK em execução no **Autor** modo. O SDK também pode ser iniciado em **Publish** para simular um ambiente de publicação AEM.

Um guia mais detalhado para configurar um ambiente de desenvolvimento do local [pode ser encontrado aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=en#local-development-environment-set-up).

1. No sistema de arquivos local, crie uma pasta dedicada para instalar a instância de publicação, ou seja, chamada `~/aem-sdk/publish`.
1. Copie o arquivo jar de Início rápido usado para a instância do Autor nos capítulos anteriores e cole-o no `publish` diretório. Como alternativa, navegue até o [Portal de distribuição de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) e baixe o SDK mais recente e extraia o arquivo jar Quickstart.
1. Renomeie o arquivo jar para `aem-publish-p4503.jar`.

   A variável `publish` string especifica que o jar de início rápido é iniciado no modo de publicação. A variável `p4503` especifica que o servidor Quickstart é executado na porta 4503.

1. Abra uma nova janela de terminal e navegue até a pasta que contém o arquivo jar. Instale e inicie a instância do AEM:

   ```shell
   $ cd ~/aem-sdk/publish
   $ java -jar aem-publish-p4503.jar
   ```

1. Forneça uma senha de administrador como `admin`. Qualquer senha de administrador é aceitável, no entanto, é recomendável usar o padrão para desenvolvimento local para evitar configurações adicionais.
1. Quando a instância do AEM terminar de ser instalada, uma nova janela do navegador será aberta em [http://localhost:4503/content.html](http://localhost:4503/content.html)

   Espera-se que retorne uma página 404 Não encontrado. Esta é uma instância de AEM totalmente nova e nenhum conteúdo foi instalado.

## Instalar conteúdo de amostra e endpoints do GraphQL {#wknd-site-content-endpoints}

Assim como na instância do Autor, a instância de Publicação precisa ter os endpoints do GraphQL ativados e precisa de conteúdo de amostra. Em seguida, instale o Site de referência WKND na instância de Publicação.

1. Baixe o pacote AEM compilado mais recente para o Site WKND: [aem-guides-wknd.all-x.x.x.zip](https://github.com/adobe/aem-guides-wknd/releases/latest).

   >[!NOTE]
   >
   > Certifique-se de baixar a versão padrão compatível com o AEM as a Cloud Service e **não** o `classic` versão.

1. Faça logon na instância de Publicação navegando diretamente para: [http://localhost:4503/libs/granite/core/content/login.html](http://localhost:4503/libs/granite/core/content/login.html) com o nome de usuário `admin` e senha `admin`.
1. Em seguida, acesse o Gerenciador de pacotes em [http://localhost:4503/crx/packmgr/index.jsp](http://localhost:4503/crx/packmgr/index.jsp).
1. Clique em **Fazer upload do pacote** e escolha o pacote WKND baixado na etapa anterior. Clique em **Instalar** para instalar o pacote.
1. Após instalar o pacote, o site de referência WKND agora está disponível em [http://localhost:4503/content/wknd/us/en.html](http://localhost:4503/content/wknd/us/en.html).
1. Sair como `admin` ao clicar no botão &quot;Sair&quot; na barra de menus.

   ![Site de referência de saída WKND](assets/publish-deployment/sign-out-wknd-reference-site.png)

   Diferentemente da instância do Autor AEM, as instâncias AEM Publish assumem o padrão de acesso anônimo somente leitura. Queremos simular a experiência de um usuário anônimo ao executar o aplicativo React.

## Atualizar variáveis de Ambiente para apontar para a instância de Publicação {#react-app-publish}

Em seguida, atualize as variáveis de ambiente usadas pelo aplicativo React para apontar para a instância de publicação. O aplicativo React deve **somente** conecte-se à instância de Publicação no modo de produção.

Em seguida, adicione um novo arquivo `.env.production.local` para simular a experiência em produção.

1. Abra o aplicativo WKND GraphQL React no IDE.

1. Abaixo `aem-guides-wknd-graphql/react-app`, adicione um arquivo chamado `.env.production.local`.
1. Preencher `.env.production.local` com o seguinte:

   ```plain
   REACT_APP_HOST_URI=http://localhost:4503
   REACT_APP_GRAPHQL_ENDPOINT=/content/graphql/global/endpoint.json
   ```

   ![Adicionar novo arquivo de variável de ambiente](assets/publish-deployment/env-production-local-file.png)

   Usar variáveis de ambiente facilita a alternância do endpoint do GraphQL entre um ambiente de Autor ou Publicação sem adicionar lógica extra no código do aplicativo. Mais informações sobre [as variáveis de ambiente personalizadas para o React podem ser encontradas aqui](https://create-react-app.dev/docs/adding-custom-environment-variables).

   >[!NOTE]
   >
   > Observe que nenhuma informação de autenticação é incluída, pois os ambientes de publicação fornecem acesso anônimo ao conteúdo por padrão.

## Implantar um servidor de nó estático {#static-server}

O aplicativo React pode ser iniciado usando o servidor do webpack, mas isso é somente para desenvolvimento. Em seguida, simule uma implantação de produção usando [serve](https://github.com/vercel/serve) para hospedar um build de produção do aplicativo React usando Node.js.

1. Abra uma nova janela de terminal e navegue até a `aem-guides-wknd-graphql/react-app` diretório

   ```shell
   $ cd aem-guides-wknd-graphql/react-app
   ```

1. Instalar [serve](https://github.com/vercel/serve) com o seguinte comando:

   ```shell
   $ npm install serve --save-dev
   ```

1. Abra o arquivo `package.json` em `react-app/package.json`. Adicionar um script chamado `serve`:

   ```diff
    "scripts": {
       "start": "react-scripts start",
       "build": "react-scripts build",
       "test": "react-scripts test",
       "eject": "react-scripts eject",
   +   "serve": "npm run build && serve -s build"
   },
   ```

   A variável `serve` O script executa duas ações. Primeiro, um build de produção do aplicativo React é gerado. Segundo, o servidor Node.js é iniciado e usa a build de produção.

1. Retorne ao terminal e digite o comando para iniciar o servidor estático:

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

1. Abra um novo navegador e acesse [http://localhost:5000/](http://localhost:5000/). Você deve ver o aplicativo React sendo disponibilizado.

   ![Aplicativo React Fornecido](assets/publish-deployment/react-app-served-port5000.png)

   Observe que a consulta do GraphQL está funcionando na página inicial. INSPECT o **XHR** solicite usando suas ferramentas de desenvolvedor. Observe que o POST GraphQL está na instância de publicação em `http://localhost:4503/content/graphql/global/endpoint.json`.

   No entanto, todas as imagens são quebradas na página inicial!

1. Clique em uma das páginas de Detalhe de Aventura.

   ![Erro de detalhe de aventura](assets/publish-deployment/adventure-detail-error.png)

   Observe que um erro de GraphQL é lançado para `adventureContributor`. Nos próximos exercícios, as imagens quebradas e o `adventureContributor` problemas foram corrigidos.

## Referências absolutas de imagem {#absolute-image-references}

As imagens parecem quebradas porque o `<img src` atributo é definido como um caminho relativo e acaba apontando para o servidor estático Node em `http://localhost:5000/`. Em vez disso, essas imagens devem apontar para a instância de publicação do AEM. Há várias soluções possíveis para isso. Ao usar o servidor de desenvolvimento do webpack, o arquivo `react-app/src/setupProxy.js` configure um proxy entre o servidor do webpack e a instância do autor do AEM para todas as solicitações para `/content`. Uma configuração de proxy pode ser usada em um ambiente de produção, mas deve ser configurada no nível do servidor Web. Por exemplo, [Módulo proxy do Apache](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html).

O aplicativo pode ser atualizado para incluir um URL absoluto usando o `REACT_APP_HOST_URI` variável de ambiente. Em vez disso, vamos usar um recurso da API GraphQL do AEM para solicitar um URL absoluto para a imagem.

1. Pare o servidor Node.js.
1. Retorne ao IDE e abra o arquivo `Adventures.js` em `react-app/src/components/Adventures.js`.
1. Adicione o `_publishUrl` para a propriedade `ImageRef` no prazo de `allAdventuresQuery`:

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

   `_publishUrl` e `_authorUrl` são valores incorporados na variável `ImageRef` para facilitar a inclusão de urls absolutos.

1. Repita as etapas acima para modificar a consulta usada na variável `filterQuery(activity)` função para incluir o `_publishUrl` propriedade.
1. Modifique o `AdventureItem` componente em `function AdventureItem(props)` para referenciar a `_publishUrl` em vez de `_path` propriedade ao construir o `<img src=''>` tag:

   ```diff
   - <img className="adventure-item-image" src={props.adventurePrimaryImage._path} alt={props.adventureTitle}/>
   + <img className="adventure-item-image" src={props.adventurePrimaryImage._publishUrl} alt={props.adventureTitle}/>
   ```

1. Abra o arquivo `AdventureDetail.js` em `react-app/src/components/AdventureDetail.js`.
1. Repita as mesmas etapas para modificar a consulta do GraphQL e adicionar a `_publishUrl` propriedade para a Aventura

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

1. Modifique os dois `<img>` tags para a imagem principal do Adventure e a referência de Imagem do colaborador no `AdventureDetail.js`:

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

1. Navegue até [http://localhost:5000/](http://localhost:5000/) e observe que as imagens aparecem e que a variável `<img src''>` atributo aponta para `http://localhost:4503`.

   ![Imagens corrompidas corrigidas](assets/publish-deployment/broken-images-fixed.png)

## Simular publicação de conteúdo {#content-publish}

Lembre-se de que um erro de GraphQL é lançado para `adventureContributor` quando uma página Detalhes da aventura é solicitada. A variável **Colaborador** O modelo de fragmento de conteúdo ainda não existe na instância de publicação. As atualizações feitas na **Aventura** O modelo de fragmento de conteúdo também não está disponível na instância de publicação. Essas alterações foram feitas diretamente na instância do Autor e precisam ser distribuídas para a instância de Publicação.

Isso é algo a ser considerado ao implantar novas atualizações em um aplicativo que depende de atualizações em um Fragmento de conteúdo ou em um Modelo de fragmento de conteúdo.

Em seguida, vamos simular a publicação de conteúdo entre as instâncias locais de Autor e Publicação.

1. Inicie a instância do Autor (se ainda não tiver sido iniciada) e navegue até o Gerenciador de pacotes em [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp)
1. Baixar o pacote [EnableReplicationAgent.zip](./assets/publish-deployment/EnableReplicationAgent.zip) e instale-o usando o Gerenciador de pacotes.

   Este pacote instala uma configuração que permite que a instância do Autor publique conteúdo na instância de Publicação. Etapas manuais para [esta configuração pode ser encontrada aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en#content-distribution).

   >[!NOTE]
   >
   > Em um ambiente de AEM as a Cloud Service, a camada Autor é configurada automaticamente para distribuir conteúdo para a camada Publicar.

1. No **Início do AEM** , navegue até **Ferramentas** > **Assets** > **Modelos de fragmentos do conteúdo**.

1. Clique em na **Site da WKND** pasta.

1. Selecione todos os três modelos e clique em **Publish**:

   ![Publicar modelos de fragmento do conteúdo](assets/publish-deployment/publish-contentfragment-models.png)

   Uma caixa de diálogo de confirmação é exibida, clique em **Publish**.

1. Navegue até o fragmento de conteúdo do campo de surfe de Bali em [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp).

1. Clique em **Publish** na barra de menu superior.

   ![Clique no botão Publicar no editor de fragmento de conteúdo](assets/publish-deployment/publish-bali-content-fragment.png)

1. O Assistente de publicação mostra todos os ativos dependentes que devem ser publicados. Nesse caso, o fragmento referenciado **stacey-roswells** O está listado e várias imagens também são referenciadas. Os ativos referenciados são publicados junto com o fragmento.

   ![Ativos referenciados para publicação](assets/publish-deployment/referenced-assets.png)

   Clique em **Publish** botão novamente para publicar o Fragmento de conteúdo e os ativos dependentes.

1. Retorne ao aplicativo React em execução em [http://localhost:5000/](http://localhost:5000/). Agora você pode clicar no Acampamento de Surf de Bali para ver os detalhes da aventura.

1. Retorne à instância do autor do AEM em [http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:4502/editor.html/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e atualize o **Título** do fragmento. **Salvar e fechar** o fragmento. Depois **publicar** o fragmento.
1. Retornar para [http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp](http://localhost:5000/adventure:/content/dam/wknd/en/adventures/bali-surf-camp/bali-surf-camp) e observe as alterações publicadas.

   ![Bali Surf Camp Publicar atualização](assets/publish-deployment/bali-surf-camp-update.png)

## Atualizar configuração de CORs

O AEM é seguro por padrão e não permite que propriedades da Web que não sejam AEM façam chamadas do lado do cliente. A configuração do Compartilhamento de recursos entre origens (CORS) do AEM pode permitir que domínios específicos façam chamadas para AEM.

Em seguida, experimente a configuração CORS da instância de publicação do AEM.

1. Retorne à janela do terminal onde o aplicativo React está em execução com o comando `npm run serve`:

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

1. Navegue até o endereço que começa com [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000). O endereço é um pouco diferente para cada computador local. Observe que há um erro CORS ao buscar os dados. Isso ocorre porque a configuração atual do CORS só permite solicitações de `localhost`.

   ![Erro do CORS](assets/publish-deployment/cors-error-not-fetched.png)

   Em seguida, atualize a configuração do CORS de publicação do AEM para permitir solicitações do endereço IP da rede.

1. Navegue até [http://localhost:4503/content/wknd/us/en/errors/sign-in.html](http://localhost:4503/content/wknd/us/en/errors/sign-in.html) e entrar com o nome de usuário `admin` e senha `admin`.

1. Navegue até [http://localhost:4503/system/console/configMgr](http://localhost:4503/system/console/configMgr) e localize a configuração WKND GraphQL em `com.adobe.granite.cors.impl.CORSPolicyImpl~wknd-graphql`.

1. Atualize o **Origens permitidas** para incluir o endereço IP da rede:

   ![Atualizar configuração do CORS](assets/publish-deployment/cors-update.png)

   Também é possível incluir uma expressão regular para permitir todas as solicitações de um subdomínio específico. Salve as alterações.

1. Pesquisar por **Filtro referenciador do Apache Sling** e revise a configuração. A variável **Permitir vazio** A configuração do também é necessária para habilitar solicitações do GraphQL de um domínio externo.

   ![Sling Referrer Filter](assets/publish-deployment/sling-referrer-filter.png)

   Eles foram configurados como parte do site de referência WKND. É possível visualizar o conjunto completo de configurações de OSGi por meio de [o repositório do GitHub](https://github.com/adobe/aem-guides-wknd/tree/master/ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig).

   >[!NOTE]
   >
   > As configurações de OSGi são gerenciadas em um projeto AEM comprometido com o controle do código-fonte. Um projeto AEM pode ser implantado no AEM como ambientes Cloud Service usando o Cloud Manager. A variável [Arquétipo de projeto AEM](https://github.com/adobe/aem-project-archetype) O pode ajudar a gerar um projeto para uma implementação específica.

1. Retornar ao aplicativo React começando com [http://192.168.86.XXX:5000](http://192.168.86.XXX:5000) e observe que o aplicativo não lança mais um erro do CORS.

   ![Erro de CORS corrigido](assets/publish-deployment/cors-error-corrected.png)

## Parabéns. {#congratulations}

Parabéns! Agora você simulou uma implantação de produção completa usando um ambiente de publicação do AEM. Você também aprendeu a usar a configuração do CORS no AEM.

## Outros recursos

Para obter mais detalhes sobre os Fragmentos de conteúdo e o GraphQL, consulte os seguintes recursos:

* [Entrega de conteúdo headless usando fragmentos de conteúdo com o GraphQL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/content-fragments/content-fragments-graphql.html?lang=pt-BR)
* [API GraphQL do AEM para uso com Fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/graphql-api-content-fragments.html?lang=pt-BR)
* [Autenticação baseada em token](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=en#authentication)
* [Implantação de código no AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/cloud-manager/devops/deploy-code.html?lang=en#cloud-manager)
