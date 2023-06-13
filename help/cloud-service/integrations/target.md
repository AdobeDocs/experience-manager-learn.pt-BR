---
title: Personalização do AEM Headless e do Target
description: Este tutorial explora como os fragmentos de conteúdo do AEM são exportados para o Adobe Target e, em seguida, usados para personalizar experiências headless usando o SDK da Web do Adobe.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
exl-id: 60a3e18a-090f-4b0e-8ba0-d4afd30577dd
source-git-commit: d81c66e041abbd56e7115f37732550cf10e59359
workflow-type: tm+mt
source-wordcount: '1671'
ht-degree: 1%

---

# Personalizar experiências AEM headless com fragmentos de conteúdo

Este tutorial explora como os fragmentos de conteúdo do AEM são exportados para o Adobe Target e, em seguida, usados para personalizar experiências headless usando o SDK da Web do Adobe. A variável [React WKND App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) O é usado para explorar como uma atividade personalizada do Target usando Ofertas de fragmentos de conteúdo pode ser adicionada à experiência, para promover uma aventura WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

O tutorial aborda as etapas envolvidas na configuração do AEM e do Adobe Target:

1. [Criar configuração do Adobe IMS para o Adobe Target](#adobe-ims-configuration) no AEM Author
1. [Criar Cloud Service Adobe Target](#adobe-target-cloud-service) no AEM Author
1. [Aplicar o Cloud Service do Adobe Target às pastas do AEM Assets](#configure-asset-folders) no AEM Author
1. [Permissão para o Cloud Service Adobe Target](#permission) no Adobe Admin Console
1. [Exportar fragmentos de conteúdo](#export-content-fragments) do autor do AEM para o Target
1. [Criar uma atividade usando ofertas de fragmento de conteúdo](#activity) no Adobe Target
1. [Criar uma sequência de dados de Experience Platform](#datastream-id) no Experience Platform
1. [Integrar personalização em um aplicativo AEM Headless baseado no React](#code) usando o SDK da Web do Adobe.

## Configuração do Adobe IMS{#adobe-ims-configuration}

Uma configuração do Adobe IMS que facilita a autenticação entre AEM e Adobe Target.

Revisão [a documentação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) para obter instruções detalhadas sobre como criar uma configuração do Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Cloud Service Adobe Target{#adobe-target-cloud-service}

Um Cloud Service do Adobe Target é criado no AEM para facilitar a exportação de fragmentos de conteúdo para o Adobe Target.

Revisão [a documentação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html) para obter instruções passo a passo sobre como criar um Cloud Service Adobe Target.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configurar pastas de ativos{#configure-asset-folders}

O Cloud Service do Adobe Target, configurado em uma configuração sensível ao contexto, deve ser aplicado à hierarquia de pastas do AEM Assets que contém os fragmentos de conteúdo a serem exportados para o Adobe Target.

+++Expandir para obter instruções passo a passo

1. Efetue logon no __Serviço de autoria do AEM__ como administrador do DAM
1. Navegue até __Ativos > Arquivos__, localize a pasta de ativos que tem o `/conf` aplicado a
1. Selecione a pasta de ativos e selecione __Propriedades__ na barra de ação superior
1. Selecione a guia __Cloud Services__
1. Verifique se a Configuração na nuvem está definida como configuração com reconhecimento de contexto (`/conf`) que contém a configuração do Adobe Target Cloud Services.
1. Selecionar __Adobe Target__ do __Configurações do Cloud Service__ lista suspensa.
1. Selecionar __Salvar e fechar__ na parte superior direita

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Permissão para integração com o AEM Target{#permission}

A integração do Adobe Target, que se manifesta como um projeto developer.adobe.com, deve receber a __Editor__ Função do produto no Adobe Admin Console, para exportar fragmentos de conteúdo para o Adobe Target.

+++Expandir para obter instruções passo a passo

1. Faça logon no Experience Cloud como usuário que pode administrar o produto Adobe Target no Adobe Admin Console
1. Abra o [Adobe Admin Console](https://adminconsole.adobe.com)
1. Selecionar __Produtos__ e depois abra __Adobe Target__
1. No __Perfis de produto__ selecione __*Espaço de trabalho padrão*__
1. Selecione o __Credenciais da API__ guia
1. Localize seu aplicativo developer.adobe.com nesta lista e defina seus __Função do produto__ para __Editor__

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Exportar fragmentos de conteúdo para o Target{#export-content-fragments}

Fragmentos de conteúdo existentes no [hierarquia de pastas configurada do AEM Assets](#apply-adobe-target-cloud-service-to-aem-assets-folders) pode ser exportado para o Adobe Target como Ofertas de fragmento de conteúdo. Essas Ofertas de fragmento de conteúdo, uma forma especial de ofertas JSON no Target, podem ser usadas em atividades do Target para fornecer experiências personalizadas em aplicativos headless.

+++Expandir para obter instruções passo a passo

1. Efetue logon no __AEM Author__ como usuário do DAM
1. Navegue até __Ativos > Arquivos__ e localize os Fragmentos de conteúdo a serem exportados como JSON para o Target na pasta &quot;Adobe Target ativado&quot;
1. Selecione os fragmentos de conteúdo que serão exportados para o Adobe Target
1. Selecionar __Exportar para ofertas do Adobe Target__ na barra de ação superior
   + Essa ação exporta a representação JSON totalmente hidratada do fragmento de conteúdo para o Adobe Target como uma &quot;Oferta de fragmento de conteúdo&quot;
   + A representação JSON totalmente hidratada pode ser revista no AEM
      + Selecionar o fragmento de conteúdo
      + Expanda o painel lateral
      + Selecionar __Visualizar__ ícone no painel lateral esquerdo
      + A representação JSON exportada para o Adobe Target é exibida na exibição principal
1. Efetue logon no [Adobe Experience Cloud](https://experience.adobe.com) com um usuário na função de Editor do Adobe Target
1. No [Experience Cloud](https://experience.adobe.com), selecione __Target__ no alternador de produtos na parte superior direita para abrir o Adobe Target.
1. Certifique-se de que o Espaço de trabalho padrão esteja selecionado no __Alternador de espaço de trabalho__ no canto superior direito.
1. Selecione o __Ofertas__ na navegação superior
1. Selecione o __Tipo__ lista suspensa e seleção __Fragmentos de conteúdo__
1. Verifique se o fragmento de conteúdo exportado do AEM aparece na lista
   + Passe o mouse sobre a oferta e selecione o __Exibir__ botão
   + Revise o __Informações da oferta__ e consulte a __Deep link AEM__ que abre o fragmento de conteúdo diretamente no serviço do AEM Author

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Atividade do Target usando ofertas de fragmento de conteúdo{#activity}

No Adobe Target, é possível criar uma Atividade que use a Oferta de fragmento de conteúdo JSON como conteúdo, permitindo experiências personalizadas em aplicativos headless com conteúdo criado e gerenciado no AEM.

Neste exemplo, usamos uma atividade A/B simples, no entanto, qualquer atividade do Target pode ser usada.

+++Expandir para obter instruções passo a passo

1. Selecione o __Atividades__ na navegação superior
1. Selecionar __+ Criar atividade__ e selecione o tipo de atividade a ser criada.
   + Este exemplo cria uma __Teste A/B__ mas as ofertas de fragmento de conteúdo podem potencializar qualquer tipo de atividade
1. No __Criar atividade__ assistente
   + Selecionar __Web__
   + Entrada __Escolher Experience Composer__, selecione __Formulário__
   + Entrada __Escolher espaço de trabalho__, selecione __Espaço de trabalho padrão__
   + Entrada __Escolher propriedade__, selecione a Propriedade em que a Atividade está disponível ou selecione __Sem restrições de propriedade__ para permitir que seja usado em todas as Propriedades.
   + Selecionar __Próxima__ para criar a Atividade
1. Renomear a atividade selecionando __renomear__ no canto superior esquerdo
   + Nomeie a atividade de forma significativa
1. Na experiência inicial, defina __Local 1__ para direcionamento da Atividade
   + Neste exemplo, direcione a um local personalizado chamado `wknd-adventure-promo`
1. Em __Conteúdo__ selecione o Default content e selecione __Alterar fragmento do conteúdo__
1. Selecione o fragmento de conteúdo exportado para ser utilizado nessa experiência e selecione __Concluído__
1. Revise o JSON da oferta do fragmento de conteúdo na área de texto Conteúdo. Esse é o mesmo JSON disponível no serviço do AEM Author por meio da ação Visualizar do fragmento de conteúdo.
1. No painel à esquerda, adicione uma Experiência e selecione uma Oferta de fragmento de conteúdo diferente para distribuir
1. Selecionar __Próxima__ e configurar as regras de direcionamento conforme necessário para a atividade
   + Neste exemplo, deixe o teste A/B como uma divisão manual 50/50.
1. Selecionar __Próxima__ e conclua as configurações da atividade
1. Selecionar __Salvar e fechar__ e dê a ele um nome significativo
1. Na Atividade no Adobe Target, selecione __Ativar__ na lista suspensa Inativo/Ativar/Arquivar na parte superior direita.

A atividade do Adobe Target direcionada ao `wknd-adventure-promo` A localização agora pode ser integrada e exposta em um aplicativo AEM Headless.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## ID da sequência de dados do Experience Platform{#datastream-id}

Um [Sequência de dados do Adobe Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) A ID é necessária para que os aplicativos AEM Headless interajam com o Adobe Target usando o [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Expandir para obter instruções passo a passo

1. Navegue até [Adobe Experience Cloud](https://experience.adobe.com/)
1. Abertura __Experience Platform__
1. Selecionar __Coleta de dados > Fluxos de dados__ e selecione __Nova sequência de dados__
1. No assistente para Novo fluxo de dados, insira:
   + Nome: `AEM Target integration`
   + Descrição: `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Esquema de evento: `Leave blank`
1. Selecione __Salvar__
1. Selecionar __Adicionar serviço__
1. Entrada __Serviço__ selecionar __Adobe Target__
   + Ativado: __Sim__
   + Token de propriedade: __Deixe em branco__
   + ID do ambiente de destino: __Deixe em branco__
      + O ambiente do Target pode ser definido no Adobe Target em __Administração > Hosts__.
   + Namespace da ID de terceiros do Target: __Deixe em branco__
1. Selecione __Salvar__
1. No lado direito, copie o __ID da sequência de dados__ para uso no [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html) chamada de configuração.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Adicionar personalização a um aplicativo AEM Headless{#code}

Este tutorial explora a personalização de um aplicativo React simples usando Ofertas de fragmento de conteúdo no Adobe Target por meio de [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Essa abordagem pode ser usada para personalizar qualquer experiência da Web baseada em JavaScript.

As experiências móveis do Android™ e do iOS podem ser personalizadas seguindo padrões semelhantes usando o [Adobe SDK móvel](https://developer.adobe.com/client-sdks/documentation/).

### Pré-requisitos

+ Node.js 14
+ Git
+ [WKND compartilhado 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) instalado nos serviços de autor e publicação do AEM as a Cloud

### Configurar

1. Baixe o código-fonte para a amostra do aplicativo React de [Github.com](https://github.com/adobe/aem-guides-wknd-graphql)

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Abrir base de código em `~/Code/aem-guides-wknd-graphql/personalization-tutorial` em seu IDE favorito
1. Atualize o host do serviço AEM ao qual você deseja que o aplicativo se conecte `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Execute o aplicativo e verifique se ele está conectado ao serviço AEM configurado. Na linha de comando, execute:

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Instale o [Adobe Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) como um pacote NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   O SDK da Web pode ser usado no código para buscar o JSON da oferta de fragmento de conteúdo por local de atividade.

   Ao configurar o SDK da Web, há duas IDs necessárias:

   + `edgeConfigId` que é o [ID da sequência de dados](#datastream-id)
   + `orgId` a ID da organização AEM as a Cloud Service/Target Adobe que pode ser encontrada em __Experience Cloud > Perfil > Informações da conta > ID da organização atual__

   Ao invocar o SDK da Web, o local da atividade do Adobe Target (em nosso exemplo, `wknd-adventure-promo`) deve ser definido como o valor no campo `decisionScopes` matriz.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implementação

1. Criar um componente do React `AdobeTargetActivity.js` para exibir atividades do Adobe Target.

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

1. Criar um componente do React `AdventurePromo.js` para renderizar a aventura JSON Adobe Target serve.

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

1. Adicionar o componente AdobeTargetActivity ao aplicativo React `Home.js` acima da lista de aventuras.

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

   A imagem abaixo mostra as duas Ofertas de fragmento de conteúdo diferentes exibidas para o `wknd-adventure-promo` Atividade, com base na lógica do Adobe Target.

   ![Ofertas de experiência](./assets/target/offers-in-app.png)

## Parabéns!

Agora que configuramos o AEM as a Cloud Service para exportar fragmentos de conteúdo para o Adobe Target, usamos as Ofertas de fragmentos de conteúdo em uma Atividade do Adobe Target AEM e vimos essa Atividade em um aplicativo Headless, personalizando a experiência.
