---
title: Integrar o AEM Headless e o Target
description: Saiba como integrar o AEM Headless e o Adobe Target para personalizar experiências headless usando o Experience Platform Web SDK.
version: Experience Manager as a Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
duration: 1549
source-git-commit: adc2f352544b4718522073642c6bf971b3600616
workflow-type: tm+mt
source-wordcount: '1618'
ht-degree: 0%

---

# Integrar o AEM Headless e o Target

Saiba como integrar o AEM Headless com o Adobe Target, exportando fragmentos de conteúdo do AEM para o Adobe Target e usando-os para personalizar experiências headless usando o alloy.js do Adobe Experience Platform Web SDK. O [Aplicativo WKND React](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html?lang=pt-BR) é usado para explorar como uma atividade personalizada do Target usando Ofertas de fragmentos de conteúdo podem ser adicionadas à experiência, para promover uma aventura WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

O tutorial aborda as etapas envolvidas na configuração do AEM e do Adobe Target:

1. [Criar configuração do Adobe IMS para o Adobe Target](#adobe-ims-configuration) no AEM Author
2. [Criar Adobe Target Cloud Service](#adobe-target-cloud-service) no Autor do AEM
3. [Aplicar o Adobe Target Cloud Service às pastas do AEM Assets](#configure-asset-folders) no Autor do AEM
4. [Permissão para o Adobe Target Cloud Service](#permission) no Adobe Admin Console
5. [Exportar fragmentos de conteúdo](#export-content-fragments) do autor do AEM para o Target
6. [Criar uma atividade usando ofertas de fragmento de conteúdo](#activity) no Adobe Target
7. [Criar uma sequência de dados do Experience Platform](#datastream-id) no Experience Platform
8. [Integre a personalização a um aplicativo headless do AEM baseado no React](#code) usando o Adobe Web SDK.

## Configuração do Adobe IMS{#adobe-ims-configuration}

Uma configuração do Adobe IMS que facilita a autenticação entre o AEM e o Adobe Target.

Consulte [a documentação](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/cloud-service/integrations/target#adobe-target-cloud-service) para obter instruções passo a passo sobre como criar uma configuração do Adobe IMS.

## Adobe Target Cloud Service{#adobe-target-cloud-service}

Um Adobe Target Cloud Service é criado no AEM para facilitar a exportação de fragmentos de conteúdo para o Adobe Target.

Consulte [a documentação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=pt-BR) para obter instruções passo a passo sobre como criar uma Adobe Target Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configurar pastas de ativos{#configure-asset-folders}

O Adobe Target Cloud Service, configurado em uma configuração sensível ao contexto, deve ser aplicado à hierarquia de pastas do AEM Assets que contém os fragmentos de conteúdo a serem exportados para o Adobe Target.

+++Expandir para obter instruções passo a passo

1. Faça logon no __serviço de Autor do AEM__ como administrador do DAM
1. Navegue até __Assets > Arquivos__, localize a pasta de ativos que tem o `/conf` aplicado a
1. Selecione a pasta de ativos e selecione __Propriedades__ na barra de ações superior
1. Selecione a guia __Cloud Services__
1. Verifique se a Configuração na Nuvem está definida como a configuração com reconhecimento de contexto (`/conf`) que contém a configuração do Adobe Target Cloud Services.
1. Selecione __Adobe Target__ na lista suspensa __Configurações do Cloud Service__.
1. Selecione __Salvar e fechar__ na parte superior direita

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Permissão para a integração do AEM Target{#permission}

A integração do Adobe Target, que se manifesta como um projeto developer.adobe.com, deve receber a função de produto __Editor__ no Adobe Admin Console para exportar Fragmentos de conteúdo para o Adobe Target.

+++Expandir para obter instruções passo a passo

1. Faça logon no Experience Cloud como usuário que pode administrar o produto Adobe Target no Adobe Admin Console
1. Abra o [Adobe Admin Console](https://adminconsole.adobe.com)
1. Selecione __Produtos__ e abra o __Adobe Target__
1. Na guia __Perfis de Produto__, selecione __*Espaço de Trabalho Padrão*__
1. Selecione a guia __Credenciais da API__
1. Localize seu aplicativo developer.adobe.com nesta lista e defina sua __Função do produto__ como __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Exportar fragmentos de conteúdo para o Target{#export-content-fragments}

Os fragmentos de conteúdo que existem na [hierarquia de pastas configurada do AEM Assets](#apply-adobe-target-cloud-service-to-aem-assets-folders) podem ser exportados para o Adobe Target como Ofertas de fragmento de conteúdo. Essas Ofertas de fragmento de conteúdo, uma forma especial de ofertas JSON no Target, podem ser usadas em atividades do Target para fornecer experiências personalizadas em aplicativos headless.

+++Expandir para obter instruções passo a passo

1. Faça logon no __AEM Author__ como usuário do DAM
1. Navegue até __Assets > Arquivos__ e localize os Fragmentos de conteúdo a serem exportados como JSON para o Target na pasta &quot;Adobe Target habilitado&quot;
1. Selecione os fragmentos de conteúdo que serão exportados para o Adobe Target
1. Selecione __Exportar para Ofertas da Adobe Target__ na barra de ações superior
   + Essa ação exporta a representação JSON totalmente hidratada do fragmento de conteúdo para o Adobe Target como uma &quot;Oferta de fragmento de conteúdo&quot;
   + A representação JSON totalmente hidratada pode ser revisada no AEM
      + Selecionar o fragmento de conteúdo
      + Expanda o painel lateral
      + Selecione o ícone __Visualizar__ no painel lateral esquerdo
      + A representação JSON exportada para o Adobe Target é exibida na exibição principal
1. Faça logon no [Adobe Experience Cloud](https://experience.adobe.com) com um usuário na função de Editor do Adobe Target
1. No [Experience Cloud](https://experience.adobe.com), selecione __Target__ no alternador de produtos na parte superior direita para abrir o Adobe Target.
1. Verifique se o Workspace Padrão está selecionado no __alternador do Workspace__ na parte superior direita.
1. Selecione a guia __Ofertas__ na navegação superior
1. Selecione a lista suspensa __Tipo__ e selecione __Fragmentos de conteúdo__
1. Verifique se o fragmento de conteúdo exportado do AEM aparece na lista
   + Passe o mouse sobre a oferta e selecione o botão __Exibir__
   + Revise as __Informações da oferta__ e veja o __deep link do AEM__ que abre o fragmento de conteúdo diretamente no serviço de autor do AEM

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Atividade do Target usando ofertas de fragmento de conteúdo{#activity}

No Adobe Target, é possível criar uma Atividade que use a Oferta de fragmento de conteúdo JSON como conteúdo, permitindo experiências personalizadas em aplicativos headless com conteúdo criado e gerenciado no AEM.

Neste exemplo, usamos uma atividade A/B simples, no entanto, qualquer atividade do Target pode ser usada.

+++Expandir para obter instruções passo a passo

1. Selecione a guia __Atividades__ na navegação superior
1. Selecione __+ Criar atividade__ e selecione o tipo de atividade a ser criada.
   + Este exemplo cria um __Teste A/B__ simples, mas as Ofertas de fragmento de conteúdo podem potencializar qualquer tipo de atividade
1. No assistente __Criar atividade__
   + Selecionar __Web__
   + Em __Escolher Experience Composer__, selecione __Formulário__
   + Em __Escolher Workspace__, selecione __Workspace Padrão__
   + Em __Escolher Propriedade__, selecione a Propriedade na qual a Atividade está disponível ou selecione __Nenhuma Restrição de Propriedade__ para permitir que ela seja usada em todas as Propriedades.
   + Selecione __Avançar__ para criar a Atividade
1. Renomeie a atividade selecionando __renomear__ no canto superior esquerdo
   + Nomeie a atividade de forma significativa
1. Na Experiência inicial, defina __Local 1__ para a Atividade como destino
   + Neste exemplo, direcione a um local personalizado chamado `wknd-adventure-promo`
1. Em __Conteúdo__, selecione o Conteúdo padrão e selecione __Alterar fragmento de conteúdo__
1. Selecione o Fragmento de conteúdo exportado para servir para esta experiência e selecione __Concluído__
1. Revise o JSON da oferta do fragmento de conteúdo na área de texto Conteúdo. Esse é o mesmo JSON disponível no serviço do AEM Author por meio da ação Visualizar do fragmento de conteúdo.
1. No painel à esquerda, adicione uma Experiência e selecione uma Oferta de fragmento de conteúdo diferente para distribuir
1. Selecione __Avançar__ e configure as Regras de direcionamento conforme necessário para a atividade
   + Neste exemplo, deixe o teste A/B como uma divisão manual 50/50.
1. Selecione __Avançar__ e conclua as configurações da atividade
1. Selecione __Salvar e fechar__ e dê a ele um nome significativo
1. Na Atividade no Adobe Target, selecione __Ativar__ na lista suspensa Inativo/Ativar/Arquivar no canto superior direito.

A atividade do Adobe Target direcionada ao local `wknd-adventure-promo` agora pode ser integrada e exposta em um aplicativo AEM Headless.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## ID da sequência de dados do Experience Platform{#datastream-id}

Uma ID de [Sequência de Dados do Adobe Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html?lang=pt-BR) é necessária para que os aplicativos do AEM Headless interajam com o Adobe Target usando o [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=pt-BR).

+++Expandir para obter instruções passo a passo

1. Navegue até [Adobe Experience Cloud](https://experience.adobe.com/)
1. Abrir __Experience Platform__
1. Selecione __Coleção de dados > Sequências de dados__ e selecione __Nova sequência de dados__
1. No assistente para Novo fluxo de dados, insira:
   + Nome: `AEM Target integration`
   + Descrição: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Esquema de Evento: `Leave blank`
1. Selecione __Salvar__
1. Selecione __Adicionar Serviço__
1. Em __Serviço__, selecione __Adobe Target__
   + Habilitado: __Sim__
   + Token de propriedade: __Deixe em branco__
   + ID do Ambiente de Destino: __Deixe em branco__
      + O Ambiente de Destino pode ser definido no Adobe Target em __Administração > Hosts__.
   + Namespace de ID de terceiros de destino: __Deixe em branco__
1. Selecione __Salvar__
1. No lado direito, copie a __ID da sequência de dados__ para uso na chamada de configuração do [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html?lang=pt-BR).

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Adicionar personalização a um aplicativo AEM Headless{#code}

Este tutorial explora a personalização de um aplicativo React simples usando Ofertas de fragmentos de conteúdo no Adobe Target via [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=pt-BR). Essa abordagem pode ser usada para personalizar qualquer experiência da Web com base no JavaScript.

As experiências móveis da Android™ e da iOS podem ser personalizadas seguindo padrões semelhantes usando a [SDK móvel da Adobe](https://developer.adobe.com/client-sdks/documentation/).

### Pré-requisitos

+ Node.js 14
+ Git
+ [WKND Compartilhado 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) instalado no AEM as a Cloud Author and Publish services

### Configurar

1. Baixe o código fonte para a amostra do aplicativo React de [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abra a base de código em `~/Code/aem-guides-wknd-graphql/personalization-tutorial` no seu IDE favorito
1. Atualize o host do serviço AEM ao qual você deseja que o aplicativo se conecte `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Execute o aplicativo e verifique se ele se conecta ao serviço AEM configurado. Na linha de comando, execute:

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Instale o [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html?lang=pt-BR#option-3%3A-using-the-npm-package) como um pacote NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   O Web SDK pode ser usado no código para buscar o JSON da oferta de fragmento de conteúdo por local de atividade.

   Ao configurar o Web SDK, há duas IDs necessárias:

   + `edgeConfigId` que é a [ID da sequência de dados](#datastream-id)
   + `orgId` a ID da Organização do AEM as a Cloud Service/Target Adobe que pode ser encontrada em __Experience Cloud > Perfil > Informações da conta > ID da organização atual__

   Ao invocar o Web SDK, o local da atividade do Adobe Target (em nosso exemplo, `wknd-adventure-promo`) deve ser definido como o valor na matriz `decisionScopes`.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementação

1. Crie um componente `AdobeTargetActivity.js` do React para exibir atividades do Adobe Target.

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   O componente AdobeTargetActivity React é chamado da seguinte maneira:

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Crie um componente React `AdventurePromo.js` para renderizar os servidores JSON Adobe Target de aventura.

   Este componente React pega o JSON totalmente hidratado, representando um fragmento de conteúdo de aventura, e exibindo de uma maneira promocional. Os componentes do React que exibem o JSON fornecido pelas Ofertas de fragmento de conteúdo do Adobe Target podem ser tão variados e complexos quanto necessário com base nos Fragmentos de conteúdo exportados para o Adobe Target.

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   Esse componente do React é chamado da seguinte maneira:

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. Adicione o componente AdobeTargetActivity ao `Home.js` do aplicativo React acima da lista de aventuras.

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. Se o aplicativo React não estiver em execução, reinicie usando `npm run start`.

   Abra o aplicativo React em dois navegadores diferentes para que o teste A/B forneça experiências diferentes a cada navegador. Se ambos os navegadores mostrarem a mesma oferta de aventura, tente fechar/reabrir um dos navegadores até que a outra experiência seja exibida.

   A imagem abaixo mostra as duas Ofertas de Fragmento de Conteúdo diferentes exibidas para a Atividade `wknd-adventure-promo`, com base na lógica do Adobe Target.

   ![Ofertas de experiência](./assets/target/offers-in-app.png)

## Parabéns.

Agora que configuramos o AEM as a Cloud Service para exportar fragmentos de conteúdo para o Adobe Target, usamos as Ofertas de fragmentos de conteúdo em uma Atividade do Adobe Target e exibimos essa Atividade em um aplicativo AEM Headless, personalizando a experiência.
