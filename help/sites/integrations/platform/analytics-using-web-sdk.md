---
title: Integrar o AEM Sites e o Adobe Analytics ao SDK da Web da plataforma
description: Integre o AEM Sites e o Adobe Analytics usando a abordagem moderna do SDK da Web da plataforma.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-25T00:00:00Z
jira: KT-13328
thumbnail: KT-13328.jpeg
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
exl-id: 0cc3d3bc-e4ea-4ab2-8878-adbcf0c914f5
duration: 2252
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# Integrar o AEM Sites e o Adobe Analytics ao SDK da Web da plataforma

Saiba mais sobre a **abordagem moderna** sobre como integrar o Adobe Experience Manager (AEM) e o Adobe Analytics usando o SDK da Web da plataforma. Este tutorial abrangente orienta você pelo processo de coleta contínua da visualização de página do [WKND](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) e dos dados de cliques do CTA. Obtenha insights valiosos visualizando os dados coletados no Adobe Analysis Workspace, onde é possível explorar várias métricas e dimensões. Além disso, explore o conjunto de dados da plataforma para verificar e analisar os dados. Junte-se a nós nesta jornada para aproveitar o poder do AEM e do Adobe Analytics para a tomada de decisões orientadas por dados.

## Visão geral

Obter insights sobre o comportamento do usuário é um objetivo crucial para cada equipe de marketing. Ao entender como os usuários interagem com seu conteúdo, as equipes podem tomar decisões informadas, otimizar estratégias e gerar melhores resultados. A equipe de marketing da WKND, uma entidade fictícia, se preocupou em implementar o Adobe Analytics em seu site para atingir essa meta. O objetivo principal é coletar dados em duas métricas principais: visualizações de página e cliques de frase de chamariz (CTA) da página inicial.

Ao rastrear visualizações de página, a equipe pode analisar quais páginas estão recebendo mais atenção dos usuários. Além disso, o rastreamento dos cliques na CTA da página inicial fornece informações valiosas sobre a eficácia dos elementos de chamada para ação da equipe. Esses dados podem revelar quais CTAs estão repercutindo com os usuários, quais precisam de ajuste e, possivelmente, descobrir novas oportunidades para melhorar o engajamento do usuário e impulsionar conversões.


>[!VIDEO](https://video.tv.adobe.com/v/3419872?quality=12&learn=on)

## Pré-requisitos

Os itens a seguir são necessários ao integrar o Adobe Analytics usando o SDK da Web da plataforma.

Você concluiu as etapas de configuração no tutorial **[Integrar SDK da Web do Experience Platform](./web-sdk.md)**.

Em **AEM como Cloud Service**:

+ [Acesso de administrador do AEM ao ambiente do AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/accessing/overview.html?lang=pt-BR)
+ Acesso do gerente de implantação ao Cloud Manager
+ Clonar e implantar o [WKND - projeto de amostra do Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) no ambiente do AEM as a Cloud Service.

Em **Adobe Analytics**:

+ Acesso para criar o **Conjunto de Relatórios**
+ Acesso para criar o **Analysis Workspace**

Em **Experience Platform**:

+ Acesso à sandbox de produção padrão, **Prod**.
+ Acesso a **Esquemas** em Gerenciamento de dados
+ Acesso a **Conjuntos de dados** em Gerenciamento de dados
+ Acesso a **Datastreams** em Coleção de dados
+ Acesso a **Tags** em Coleção de dados

Caso você não tenha as permissões necessárias, o administrador do sistema que usa o [Adobe Admin Console](https://adminconsole.adobe.com/) poderá conceder as permissões necessárias.

Antes de nos aprofundar no processo de integração do AEM e do Analytics usando o SDK da Web da Platform, vamos _recapitular os componentes essenciais e os elementos-chave_ que foram estabelecidos no tutorial [Integrar SDK da Web do Experience Platform](./web-sdk.md). Ele fornece uma base sólida para a integração.

>[!VIDEO](https://video.tv.adobe.com/v/3419873?quality=12&learn=on)

Após o resumo da conexão de esquema XDM, sequência de dados, conjunto de dados, propriedade da tag e, AEM e propriedade da tag, vamos embarcar na jornada de integração.

## Definir o documento Referência de design da solução do Analytics (SDR)

Como parte do processo de implementação, é recomendável criar um documento de Referência de design de solução (SDR). Este documento desempenha uma função crucial como um blueprint para definir requisitos de negócios e projetar estratégias eficazes de coleta de dados.

O documento de DSE fornece uma visão geral abrangente do plano de implementação, garantindo que todas as partes interessadas estejam alinhadas e entendam os objetivos e o escopo do projeto.


>[!VIDEO](https://video.tv.adobe.com/v/3419874?quality=12&learn=on)

Para obter mais informações sobre conceitos e vários elementos que devem ser incluídos no documento de SDR, visite o [Documento de Referência de Design de Solução (SDR)](https://experienceleague.adobe.com/docs/analytics-learn/tutorials/implementation/implementation-basics/creating-and-maintaining-an-sdr.html). Você também pode baixar um modelo de amostra do Excel, mas a versão específica do WKND também está disponível [aqui](./assets/Initial-WKND-WebSDK-BRD-SDR.xlsx).

## Configurar o Analytics - conjunto de relatórios, Analysis Workspace

A primeira etapa é configurar o Adobe Analytics, especificamente o conjunto de relatórios com variáveis de conversão (ou eVar) e eventos bem-sucedidos. As variáveis de conversão são usadas para medir causa e efeito. Os eventos bem-sucedidos são usados para rastrear ações.

Neste tutorial, o `eVar5, eVar6, and eVar7` rastreia o _Nome da página WKND, a ID CTA WKND e o Nome CTA WKND_, respectivamente, e o `event7` é usado para rastrear o _Evento de clique no CTA WKND_.

Para analisar, coletar insights e compartilhar esses insights com outras pessoas a partir dos dados coletados, um projeto no Analysis Workspace é criado.

>[!VIDEO](https://video.tv.adobe.com/v/3419875?quality=12&learn=on)

Para saber mais sobre a configuração e os conceitos do Analytics, os seguintes recursos são altamente recomendados:

+ [Conjunto de Relatórios](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html)
+ [Variáveis de conversão](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin.html)
+ [Eventos bem-sucedidos](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/success-events/success-event.html)
+ [Analysis Workspace](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/home.html)

## Atualizar sequência de dados - Adicionar serviço do Analytics

Um fluxo de dados instrui o Edge Network da plataforma para onde enviar os dados coletados. No [tutorial anterior](./web-sdk.md), um Datastream está configurado para enviar os dados para o Experience Platform. Esta sequência de dados é atualizada para enviar os dados ao conjunto de relatórios do Analytics que foi configurado na etapa [acima](#setup-analytics---report-suite-analysis-workspace).

>[!VIDEO](https://video.tv.adobe.com/v/3419876?quality=12&learn=on)

## Criar esquema XDM

O esquema do Experience Data Model (XDM) ajuda a padronizar os dados coletados. No [tutorial anterior](./web-sdk.md), um esquema XDM com `AEP Web SDK ExperienceEvent` um grupo de campos é criado. Além disso, usando esse esquema XDM, um conjunto de dados é criado para armazenar os dados coletados no Experience Platform.

No entanto, esse Esquema XDM não tem grupos de campos específicos do Adobe Analytics para enviar os dados de eVar e evento. Um novo schema XDM é criado em vez de atualizar o schema existente para evitar o armazenamento dos dados do eVar e do evento na plataforma.

O esquema XDM recém-criado tem `AEP Web SDK ExperienceEvent` e `Adobe Analytics ExperienceEvent Full Extension` grupos de campos.

>[!VIDEO](https://video.tv.adobe.com/v/3419877?quality=12&learn=on)


## Atualizar propriedade de tag

No [tutorial anterior](./web-sdk.md), uma propriedade de tag é criada, ela tem Elementos de dados e uma Regra para coletar, mapear e enviar os dados de visualização de página. Deve ser melhorado para:

+ Mapeando o nome da página para `eVar5`
+ Acionando a chamada do Analytics **pageview** (ou enviar beacon)
+ Coleta de dados CTA usando a Camada de dados de clientes Adobe
+ Mapeando a ID e o Nome do CTA para `eVar6` e `eVar7`, respectivamente. Além disso, a contagem de cliques do CTA para `event7`
+ Acionando a chamada **de clique no link** do Analytics (ou enviar beacon)


>[!VIDEO](https://video.tv.adobe.com/v/3419882?quality=12&learn=on)

>[!TIP]
>
>O elemento de dados e o código de evento de regra mostrados no vídeo estão disponíveis para sua referência, **expanda o elemento de acordeão abaixo**. No entanto, se você NÃO estiver usando a Camada de dados de clientes Adobe, deverá modificar o código abaixo, mas o conceito de definir os Elementos de dados e usá-los na definição de Regra ainda se aplica.

+++ Elemento de dados e código de evento de regra

+ O código do Elemento de Dados `Component ID`.

  ```javascript
  if(event && event.path && event.path.includes('.')) {    
      // split on the `.` to return just the component ID for e.g. button-06bc532b85, tabs-bb27f4f426-item-cc9c2e6718
      return event.path.split('.')[1];
  }else {
      //return dummy ID
      return "WKND-CTA-ID";
  }
  ```

+ O código do Elemento de Dados `Component Name`.

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('dc:title')) {
      // Return the Button, Link, Image, Tab name, for e.g. View Trips, Full Article, See Trips
      return event.component['dc:title'];
  }else {
      //return dummy ID
      return "WKND-CTA-Name";    
  }    
  ```

+ O código `all pages - on load` **Condição-Regra**

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
      return true;
  }else{
      return false;
  }    
  ```

+ O código `home page - cta click` **Rule-Event**

  ```javascript
  var componentClickedHandler = function(evt) {
  // defensive coding to avoid a null pointer exception
  if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
      //trigger Tag Rule and pass event
      console.log("cmp:click event: " + evt.eventInfo.path);
  
      var event = {
          //include the path of the component that triggered the event
          path: evt.eventInfo.path,
          //get the state of the component that triggered the event
          component: window.adobeDataLayer.getState(evt.eventInfo.path)
      };
  
      //Trigger the Tag Rule, passing in the new `event` object
      // the `event` obj can now be referenced by the reserved name `event` by other Tag Property data elements
      // i.e `event.component['someKey']`
      trigger(event);
  }
  }
  
  //set the namespace to avoid a potential race condition
  window.adobeDataLayer = window.adobeDataLayer || [];
  //push the event listener for cmp:click into the data layer
  window.adobeDataLayer.push(function (dl) {
  //add event listener for `cmp:click` and callback to the `componentClickedHandler` function
  dl.addEventListener("cmp:click", componentClickedHandler);
  });    
  ```

+ O código `home page - cta click` **Condição-Regra**

  ```javascript
  if(event && event.component && event.component.hasOwnProperty('@type')) {
      //Check for Button Type OR Teaser CTA type
      if(event.component['@type'] === 'wknd/components/button' ||
      event.component['@type'] === 'wknd/components/teaser/cta') {
          return true;
      }
  }
  
  // none of the conditions are met, return false
  return false;    
  ```

+++

Para obter informações adicionais sobre a integração dos Componentes principais do AEM com a Camada de Dados de Clientes Adobe, consulte o [guia Uso da Camada de Dados de Clientes Adobe AEM com Componentes Principais do](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=pt-BR).


>[!INFO]
>
>Para obter uma compreensão abrangente dos detalhes da propriedade da guia **Mapa de Variáveis** no documento Referência de Design de Solução (SDR), acesse a versão concluída específica do WKND para download [aqui](./assets/Final-WKND-WebSDK-BRD-SDR.xlsx).



## Verificar propriedade de tag atualizada na WKND

Para garantir que a propriedade de tag atualizada seja criada, publicada e esteja funcionando corretamente nas páginas do site da WKND. Use a [extensão de Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) do navegador da Web Google Chrome:

+ Para garantir que a propriedade da tag seja da versão mais recente, verifique a data de build.

+ Para verificar os dados do evento XDM para o Clique em PageView e HomePage CTA, use a opção de menu do SDK da Web do Experience Platform na extensão.

>[!VIDEO](https://video.tv.adobe.com/v/3419883?quality=12&learn=on)

## Simular tráfego da Web - Automação do Selenium

Para gerar uma quantidade significativa de tráfego para fins de teste, um script de automação do Selenium é desenvolvido. Esse script personalizado simula as interações do usuário com o site da WKND, como exibição de página e clique em CTAs.

>[!VIDEO](https://video.tv.adobe.com/v/3419884?quality=12&learn=on)

## Verificação do conjunto de dados - Exibição de página WKND, dados CTA

O conjunto de dados é uma construção de armazenamento e gerenciamento para uma coleção de dados, como uma tabela de banco de dados que segue um esquema. O Conjunto de dados criado no [tutorial anterior](./web-sdk.md) é reutilizado para verificar se os dados de cliques de pageview e CTA são assimilados no Conjunto de dados do Experience Platform. Na interface do usuário do conjunto de dados, vários detalhes, como registros totais, tamanho e lotes assimilados, são exibidos junto com um gráfico de barras visualmente atraente.

>[!VIDEO](https://video.tv.adobe.com/v/3419885?quality=12&learn=on)

## Analytics - Exibição de página WKND, visualização de dados CTA

O Analysis Workspace é uma ferramenta eficiente no Adobe Analytics que permite explorar e visualizar dados de maneira flexível e interativa. Ele fornece uma interface de arrastar e soltar para criar relatórios personalizados, executar segmentação avançada e aplicar várias visualizações de dados.

Vamos reabrir o projeto do Analysis Workspace criado na etapa [Configurar o Analytics](#setup-analytics---report-suite-analysis-workspace). Na seção **Principais páginas**, examine várias métricas, como visitas, visitantes únicos, entradas, taxa de rejeição e muito mais. Para avaliar o desempenho de páginas WKND e CTAs da página inicial, arraste e solte as dimensões específicas da WKND (Nome da página WKND, Nome da CTA WKND) e as métricas (Evento de clique WKND CTA). Esses insights são valiosos para que os profissionais de marketing entendam quais CTAs são mais eficazes e tomem decisões orientadas por dados, alinhadas a seus objetivos de negócios.

Para visualizar jornadas de usuário, use a Visualização de fluxo, começando com o **Nome da página do WKND** e expandindo em vários caminhos.

>[!VIDEO](https://video.tv.adobe.com/v/3419886?quality=12&learn=on)

## Resumo

Excelente trabalho. Você concluiu a configuração do AEM e do Adobe Analytics usando o SDK da Web da plataforma para coletar, analisar a visualização de página e os dados de cliques do CTA.

A implementação do Adobe Analytics é fundamental para que as equipes de marketing obtenham insights do comportamento do usuário, tomem decisões informadas, permitindo que otimizem seu conteúdo e tomem decisões orientadas por dados.

Ao implementar as etapas recomendadas e usar os recursos fornecidos, como o documento Referência de design de solução (SDR) e entender os principais conceitos do Analytics, os profissionais de marketing podem coletar e analisar dados de maneira eficaz.

>[!VIDEO](https://video.tv.adobe.com/v/3419888?quality=12&learn=on)


>[!AVAILABILITY]
>
>Se preferir o **vídeo completo** que cobre todo o processo de integração em vez de vídeos individuais da etapa de configuração, você pode clicar [aqui](https://video.tv.adobe.com/v/3419889/) para acessá-lo.


## Recursos adicionais

+ [Integrar SDK da Web do Experience Platform](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform/web-sdk.html)
+ [Usando a Camada de Dados de Clientes Adobe com os Componentes Principais](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=pt-BR)
+ [Integrando as Marcas de Coleta de Dados Experience Platform e AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-data-collection-tags/overview.html?lang=pt-BR)
+ [Visão geral do Adobe Experience Platform Web SDK e do Edge Network](https://experienceleague.adobe.com/docs/platform-learn/data-collection/web-sdk/overview.html)
+ [Tutoriais da Coleção de dados](https://experienceleague.adobe.com/docs/platform-learn/data-collection/overview.html)
+ [visão geral do Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)
