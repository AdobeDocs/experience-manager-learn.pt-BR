---
title: Integrar o SDK da Web do Experience Platform
description: Saiba como integrar AEM as a Cloud Service com o SDK da Web do Experience Platform. Essa etapa fundamental é para integrar produtos Adobe Experience Cloud, como Adobe Analytics, Target ou produtos inovadores recentes, como Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.
version: Cloud Service
feature: Integrations
topic: Integrations, Architecture
role: Admin, Architect, Data Architect, Developer
level: Beginner, Intermediate
doc-type: Tutorial
last-substantial-update: 2023-04-26T00:00:00Z
jira: KT-13156
thumbnail: KT-13156.jpeg
source-git-commit: 593ef5767a5f2321c689e391f9c9019de7c94672
workflow-type: tm+mt
source-wordcount: '1060'
ht-degree: 1%

---


# Integrar o SDK da Web do Experience Platform

Saiba como integrar AEM as a Cloud Service com o Experience Platform [Web SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html). Essa etapa fundamental é para integrar produtos Adobe Experience Cloud, como Adobe Analytics, Target ou produtos inovadores recentes, como Real-time Customer Data Platform, Customer Journey Analytics e Journey Optimizer.

Você também aprende a coletar e enviar [WKND - exemplo de projeto do Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) os dados de visualização de página na [Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/landing/home.html).

Após concluir essa configuração, você pode avançar para implementar o Experience Platform e aplicativos relacionados, como [Real-time Customer Data Platform (Real-Time CDP)](https://experienceleague.adobe.com/docs/experience-platform/rtcdp/overview.html?lang=pt-BR), [Customer Journey Analytics (CJA)](https://experienceleague.adobe.com/docs/customer-journey-analytics.html) e [Adobe Journey Optimizer (AJO)](https://experienceleague.adobe.com/docs/journey-optimizer.html). Para impulsionar um melhor engajamento do cliente padronizando os dados da Web e do cliente.

## Pré-requisitos

Os itens a seguir são necessários ao integrar o SDK da Web do Experience Platform.

Em **AEM como Cloud Service**:

+ AEM Acesso do administrador a AEM ambiente as a Cloud Service
+ Acesso do Gerenciador de implantação ao Cloud Manager
+ Clonar e implantar o [WKND - exemplo de projeto do Adobe Experience Manager](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) ao seu ambiente as a Cloud Service AEM.

Em **Experience Platform**:

+ Acesso à produção padrão, **Prod** sandbox.
+ Acesso ao **Esquemas** em Gestão de Dados
+ Acesso ao **Conjuntos de dados** em Gestão de Dados
+ Acesso ao **Datastreams** em Coleta de dados
+ Acesso ao **Tags** (anteriormente conhecido como Launch) em Coleta de dados

Caso você não tenha as permissões necessárias, o administrador do sistema está usando [Adobe Admin Console](https://adminconsole.adobe.com/) O pode conceder as permissões necessárias.

>[!VIDEO](https://video.tv.adobe.com/v/3418856?quality=12&learn=on)

## Criar esquema XDM - Experience Platform

O Esquema do Experience Data Model (XDM) ajuda a padronizar os dados de experiência do cliente. Para coletar a variável **Visualização de página WKND** , crie um Esquema XDM e use os grupos de campos fornecidos pelo Adobe `AEP Web SDK ExperienceEvent` para coleta de dados da web.


>[!VIDEO](https://video.tv.adobe.com/v/3418894?quality=12&learn=on)

Saiba mais sobre o Esquema XDM e conceitos relacionados, como grupos de campos, tipos, classes e tipos de dados do [Visão geral do sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html).

O [Visão geral do sistema XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html) O é um excelente recurso para saber mais sobre o Esquema XDM e conceitos relacionados, como grupos de campos, tipos, classes e tipos de dados. Ele fornece uma compreensão abrangente do modelo de dados XDM e como criar e gerenciar esquemas XDM para padronizar dados na empresa. Explore-o para obter uma compreensão mais profunda do esquema XDM e como ele pode beneficiar seus processos de coleta e gerenciamento de dados.

## Criar DataStream - Experience Platform

Um DataStream instrui a Platform Edge Network onde enviar os dados coletados. Por exemplo, ele pode ser enviado para o Experience Platform, Analytics ou Adobe Target.


>[!VIDEO](https://video.tv.adobe.com/v/3418895?quality=12&learn=on)

Familiarize-se com o conceito de Datastreams e tópicos relacionados, como governança e configuração de dados, ao visitar o site [Visão geral dos conjuntos de dados](https://experienceleague.adobe.com/docs/experience-platform/edge/datastreams/overview.html) página.

## Criar propriedade de tag - Experience Platform

Saiba como criar uma propriedade de tag (anteriormente conhecida como Launch) no Experience Platform para adicionar a biblioteca JavaScript do SDK da Web ao site WKND. A propriedade de tag recém-definida tem os seguintes recursos:

+ Extensões de tag: [Núcleo](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) e [Adobe Experience Platform Web SDK](https://exchange.adobe.com/apps/ec/106387/aep-web-sdk)
+ Elementos de dados: Os elementos de dados do tipo de código personalizado que extraem nome da página, seção do site e nome do host usando a Camada de dados do cliente de Adobe do site WKND. Além disso, o elemento de dados do tipo Objeto XDM que está em conformidade com a build de esquema WKND XDM recém-criada anteriormente [Criar esquema XDM](#create-xdm-schema---experience-platform) etapa.
+ Regra: Enviar dados para a Rede de borda da plataforma sempre que uma página da Web WKND for visitada usando a Camada de dados do cliente do Adobe acionada `cmp:show` evento.


>[!VIDEO](https://video.tv.adobe.com/v/3418896?quality=12&learn=on)

O [Visão geral das tags](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html) O fornece conhecimento profundo sobre conceitos importantes, como Elementos de dados, Regras e Extensões.

Para obter informações adicionais sobre a integração dos Componentes principais AEM com a camada de dados do cliente Adobe, consulte [Uso da camada de dados do cliente do Adobe com AEM guia Componentes principais](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/adobe-client-data-layer/data-layer-overview.html?lang=pt-BR).

## Conectar a propriedade Tag ao AEM

Descubra como vincular a propriedade de tag recém-criada à AEM por meio do Adobe IMS e da Configuração do Adobe Launch no AEM. Quando um ambiente AEM as a Cloud Service é estabelecido, várias configurações de conta técnica do Adobe IMS são geradas automaticamente, incluindo o Adobe Launch. No entanto, para AEM versão 6.5, é necessário configurar uma manualmente.

Depois de vincular a propriedade de tag, o site WKND é capaz de carregar a biblioteca JavaScript da propriedade de tag nas páginas da Web usando a configuração do serviço de nuvem do Adobe Launch.

### Verificar o carregamento da propriedade de tag no WKND

Uso do Adobe Experience Platform Debugger [Cromo](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) ou [Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) , verifique se a propriedade da tag está sendo carregada em páginas WKND. Você pode verificar,

+ Detalhes da propriedade da tag, como extensão, versão, nome e muito mais.
+ Versão da biblioteca do SDK da Web da plataforma, ID do conjunto de dados
+ Objeto XDM como parte `events` atributo no SDK da Web do Experience Platform

>[!VIDEO](https://video.tv.adobe.com/v/3418897?quality=12&learn=on)

## Criar conjunto de dados - Experience Platform

Os dados de visualização de página coletados com o SDK da Web são armazenados no Experience Platform data lake como conjuntos de dados. O conjunto de dados é uma construção de armazenamento e gerenciamento para uma coleção de dados como uma tabela de banco de dados que segue um esquema. Saiba como criar um conjunto de dados e configurar o conjunto de dados criado anteriormente para enviar dados para o Experience Platform.


>[!VIDEO](https://video.tv.adobe.com/v/3418898?quality=12&learn=on)

O [Visão geral dos conjuntos de dados](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) O fornece mais informações sobre conceitos, configurações e outros recursos de assimilação.


## WKND pageview data no Experience Platform

Após a configuração do SDK da Web com AEM, especialmente no site WKND, é hora de gerar tráfego navegando pelas páginas do site. Em seguida, confirme se os dados de visualização de página estão sendo assimilados no Experience Platform Dataset. Na interface do usuário do conjunto de dados, vários detalhes, como registros totais, tamanho e lotes assimilados, são exibidos junto com um gráfico de barras visualmente atraente.

>[!VIDEO](https://video.tv.adobe.com/v/3418899?quality=12&learn=on)


## Resumo

Excelente trabalho. Você concluiu a configuração do AEM com o SDK da Web do Adobe Experience Platform (Experience Platform) para coletar e assimilar dados de um site. Com essa base, agora você pode explorar mais possibilidades para aprimorar e integrar produtos como o Analytics, Target, Customer Journey Analytics (CJA) e muitos outros para criar experiências ricas e personalizadas para seus clientes. Continue aprendendo e explorando para explorar todo o potencial do Adobe Experience Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3418900?quality=12&learn=on)
