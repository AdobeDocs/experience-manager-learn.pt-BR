---
title: Integrar SDK da Web do Experience Platform
description: Saiba como integrar o AEM as a Cloud Service ao SDK da Web do Experience Platform. Esta etapa fundamental é essencial para a integração de produtos Adobe Experience Cloud, como Adobe Analytics, Target ou produtos inovadores recentes, como Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
exl-id: b5182d35-ec38-4ffd-ae5a-ade2dd3f856d
source-git-commit: 32472c8591aeb47a7c6a7253afd7ad9ab0e45171
workflow-type: tm+mt
source-wordcount: '1340'
ht-degree: 3%

---

# Integrar SDK da Web do Experience Platform

Saiba como integrar o AEM as a Cloud Service ao Experience Platform [SDK da Web](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Esta etapa fundamental é essencial para a integração de produtos Adobe Experience Cloud, como Adobe Analytics, Target ou produtos inovadores recentes, como Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.

Você também aprenderá a coletar e enviar [WKND - projeto Adobe Experience Manager de amostra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) pageview na variável [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Após concluir esta configuração, você implementou uma base sólida. Além disso, você está pronto para avançar a implementação de Experience Platform usando aplicativos como [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=pt-BR), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html), e [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). A implementação avançada ajuda a impulsionar um melhor engajamento do cliente padronizando a Web e os dados do cliente.

## Pré-requisitos

Os seguintes requisitos são necessários ao integrar o SDK da Web do Experience Platform.

Entrada **AEM como Cloud Service**:

+ Acesso do administrador do AEM ao ambiente as a Cloud Service do AEM
+ Acesso do Gerenciador de implantação ao Cloud Manager
+ Clonar e implantar o [WKND - projeto Adobe Experience Manager de amostra](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) ao seu ambiente as a Cloud Service AEM.

Entrada **Experience Platform**:

+ Acesso à produção padrão, **Prod** sandbox.
+ Acesso a **Esquemas** em Gerenciamento de dados
+ Acesso a **Conjuntos de dados** em Gerenciamento de dados
+ Acesso a **Datastreams** em Coleção de dados
+ Acesso a **Tags** (conhecido anteriormente como Launch) em Coleção de dados

Caso não tenha as permissões necessárias, o administrador do sistema que estiver usando o [Adobe Admin Console](https://adminconsole.adobe.com/) O pode conceder as permissões necessárias.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Criar esquema XDM - Experience Platform

O esquema do Experience Data Model (XDM) ajuda a padronizar os dados de experiência do cliente. Para coletar as **Exibição de página do WKND** criar um esquema XDM e usar os grupos de campos fornecidos pelo Adobe `AEP Web SDK ExperienceEvent` para coleta de dados na web.

Há setores genéricos e específicos, por exemplo, Varejo, Serviços financeiros, Saúde e muito mais, conjunto de modelos de dados de referência. Consulte [Visão geral dos modelos de dados do setor](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/industries/overview.html) para obter mais informações.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Saiba mais sobre o Esquema XDM e conceitos relacionados, como grupos de campos, tipos, classes e tipos de dados do [Visão geral do sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

A variável [Visão geral do sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) O é um excelente recurso para saber mais sobre o Esquema XDM e conceitos relacionados, como grupos de campos, tipos, classes e tipos de dados. Ele fornece uma compreensão abrangente do modelo de dados XDM e como criar e gerenciar esquemas XDM para padronizar dados na empresa. Explore-o para obter uma compreensão mais profunda do esquema XDM e como ele pode beneficiar seus processos de coleta e gerenciamento de dados.

## Criar sequência de dados - Experience Platform

Uma sequência de dados instrui a Rede de borda da plataforma para onde enviar os dados coletados. Por exemplo, ela pode ser enviada para o Experience Platform, Analytics ou Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Familiarize-se com o conceito de Datastreams e tópicos relacionados, como governança e configuração de dados, visitando o [Visão geral dos fluxos de dados](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) página.

## Criar propriedade de tag - Experience Platform

Saiba como criar uma propriedade de tag (anteriormente conhecida como Launch) no Experience Platform para adicionar a biblioteca JavaScript do SDK da Web ao site da WKND. A propriedade de tag recém-definida tem os seguintes recursos:

+ Extensões de tag: [Núcleo](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) e [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementos de dados: os elementos de dados do tipo de código personalizado que extraem o nome da página, a seção do site e o nome do host usando a Camada de dados do cliente Adobe do site WKND. Além disso, o elemento de dados do tipo Objeto XDM é compatível com a compilação do esquema XDM da WKND recém-criado anteriormente [Criar esquema XDM](#create-xdm-schema---experience-platform) etapa.
+ Regra: enviar dados para a Rede de borda da Platform sempre que uma página da Web WKND for visitada usando a Camada de dados do cliente Adobe acionada `cmp:show` evento.

Ao criar e publicar a biblioteca de tags usando o **Fluxo de publicação**, você pode usar o **Adicionar todos os recursos alterados** botão. Para selecionar todos os recursos como Elemento de dados, Regra e Extensões de tag, em vez de identificar e selecionar um recurso individual. Além disso, durante a fase de desenvolvimento, você pode publicar a biblioteca apenas no _Desenvolvimento_ e, em seguida, verifique e promova-o para o _Estágio_ ou _Produção_ ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)


>[!TIP]
>
>O elemento de dados e o código de evento de regra mostrados no vídeo estão disponíveis para sua referência, **expanda o elemento acordeão abaixo**. No entanto, se você NÃO estiver usando a Camada de dados de clientes Adobe, deverá modificar o código abaixo, mas o conceito de definir os Elementos de dados e usá-los na definição de Regra ainda se aplica.


+++ Elemento de dados e código de evento de regra

+ A variável `Page Name` Código do elemento de dados.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('dc:title')) {
       // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show` event
       return event.component['dc:title'];
   }
   ```

+ A variável `Site Section` Código do elemento de dados.

   ```javascript
   if(event && event.component && event.component.hasOwnProperty('repo:path')) {
   let pagePath = event.component['repo:path'];
   
   let siteSection = '';
   
   //Check of html String in URL.
   if (pagePath.indexOf('.html') > -1) { 
    siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
   
    //replace slash with colon
    siteSection = siteSection.replaceAll('/', ':');
   
    //remove `:content`
    siteSection = siteSection.replaceAll(':content:','');
   }
   
       return siteSection 
   }
   ```

+ A variável `Host Name` Código do elemento de dados.

   ```javascript
   if(window && window.location && window.location.hostname) {
       return window.location.hostname;
   }
   ```

+ A variável `all pages - on load` Código de evento de regra

   ```javascript
   var pageShownEventHandler = function(evt) {
   // defensive coding to avoid a null pointer exception
   if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
       //trigger Launch Rule and pass event
       console.debug("cmp:show event: " + evt.eventInfo.path);
       var event = {
           //include the path of the component that triggered the event
           path: evt.eventInfo.path,
           //get the state of the component that triggered the event
           component: window.adobeDataLayer.getState(evt.eventInfo.path)
       };
   
       //Trigger the Launch Rule, passing in the new 'event' object
       // the 'event' obj can now be referenced by the reserved name 'event' by other Launch data elements
       // i.e 'event.component['someKey']'
       trigger(event);
       }
   }
   
   //set the namespace to avoid a potential race condition
   window.adobeDataLayer = window.adobeDataLayer || [];
   
   //push the event listener for cmp:show into the data layer
   window.adobeDataLayer.push(function (dl) {
       //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
       dl.addEventListener("cmp:show", pageShownEventHandler);
   });
   ```

+++


A variável [Visão geral das tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) O fornece conhecimento profundo sobre conceitos importantes, como Elementos de dados, Regras e Extensões.

Para obter informações adicionais sobre a integração dos Componentes principais do AEM com a Camada de dados de clientes Adobe, consulte [Uso da Camada de dados do cliente Adobe com o guia dos Componentes principais AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=pt-BR).

## Conectar propriedade de tag ao AEM

Descubra como vincular a propriedade de tag recém-criada ao AEM por meio do Adobe IMS e da Configuração do Adobe Launch no AEM. Quando um ambiente as a Cloud Service do AEM é estabelecido, várias configurações de conta técnica do Adobe IMS são geradas automaticamente, incluindo o Adobe Launch. No entanto, para a versão AEM 6.5, é necessário configurar um manualmente.

Depois de vincular a propriedade da tag, o site da WKND pode carregar a biblioteca de JavaScript da propriedade da tag nas páginas da Web usando a configuração do serviço de nuvem do Adobe Launch.

### Verificar carregamento de propriedade de tag no WKND

Utilização do Adobe Experience Platform Debugger [Cromo](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) ou [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) extensão, verifique se a propriedade da tag está sendo carregada nas páginas WKND. Você pode verificar,

+ Detalhes de propriedade da tag, como extensão, versão, nome e muito mais.
+ Versão da biblioteca do SDK da Web da plataforma, ID da sequência de dados
+ Objeto XDM como parte `events` atributo no SDK da Web do Experience Platform

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Criar conjunto de dados - Experience Platform

Os dados de visualização de página coletados usando o SDK da Web são armazenados no data lake do Experience Platform como conjuntos de dados. O conjunto de dados é uma construção de armazenamento e gerenciamento para uma coleção de dados, como uma tabela de banco de dados que segue um esquema. Saiba como criar um Conjunto de dados e configurar a sequência de dados criada anteriormente para enviar dados para o Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

A variável [Visão geral dos conjuntos de dados](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) O fornece mais informações sobre conceitos, configurações e outros recursos de assimilação.


## Dados de visualização de página do WKND no Experience Platform

Após a configuração do SDK da Web com AEM, particularmente no site da WKND, é hora de gerar tráfego navegando pelas páginas do site. Em seguida, confirme se os dados de pageview estão sendo assimilados no conjunto de dados do Experience Platform. Na interface do usuário do conjunto de dados, vários detalhes, como registros totais, tamanho e lotes assimilados, são exibidos junto com um gráfico de barras visualmente atraente.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Resumo

Excelente trabalho. Você concluiu a configuração do AEM com o SDK da Web do Experience Platform para coletar e assimilar dados de um site. Com essa base, agora é possível explorar mais possibilidades para aprimorar e integrar produtos como Analytics, Target, Customer Journey Analytics (CJA) e muitos outros, a fim de criar experiências avançadas e personalizadas para seus clientes. Continue aprendendo e explorando para liberar todo o potencial do Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)


>[!AVAILABILITY]
>
>Se preferir o **vídeo completo** que cobre todo o processo de integração em vez de vídeos individuais da etapa de configuração, você pode clicar em [aqui](https://video.tv.adobe.com/v/3418905/) para acessá-lo.

## Recursos adicionais

+ [Uso da Camada de dados de clientes Adobe com os Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=pt-BR)
+ [Integração de tags de coleção de dados do Experience Platform e AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html)
+ [Visão geral do SDK da Web da Adobe Experience Platform e da rede de borda](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriais de Coleção de dados](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [Visão geral do Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
