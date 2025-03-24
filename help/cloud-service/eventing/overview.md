---
title: Evento do AEM
description: Saiba mais sobre os eventos do AEM, o que são, por que e quando usá-los e exemplos deles.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 540
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
exl-id: 142ed6ae-1659-4849-80a3-50132b2f1a86
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 0%

---

# Evento do AEM

Saiba mais sobre os eventos do AEM, o que são, por que e quando usá-los e exemplos deles.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

## O que é

O AEM Eventing é um sistema de eventos nativo em nuvem que permite assinaturas de eventos do AEM para processamento em sistemas externos. Um Evento AEM é uma notificação de alteração de estado enviada pela AEM sempre que uma ação específica ocorre. Por exemplo, isso pode incluir eventos quando um fragmento de conteúdo é criado, atualizado ou excluído.

![Evento do AEM](./assets/aem-eventing.png)

O diagrama acima visualizou como o AEM as a Cloud Service produz eventos e os envia para o Adobe I/O Events, que, por sua vez, os expõe aos assinantes de eventos.

Em resumo, há três componentes principais:

1. **Provedor de eventos:** AEM as a Cloud Service.
1. **Adobe I/O Events:** plataforma de desenvolvedor para integrar, estender e criar aplicativos e experiências com base nos produtos e tecnologias da Adobe.
1. **Consumidor de eventos:** sistemas de propriedade do cliente que assinou os Eventos da AEM. Por exemplo, um CRM (Customer Relationship Management), PIM (Product Information Management), OMS (Order Management System) ou um aplicativo personalizado.

### Como é diferente

Os [eventos do Apache Sling](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), os eventos OSGi e a [observação JCR](https://jackrabbit.apache.org/oak/docs/features/observation.html) oferecem todos os mecanismos para assinar e processar eventos. No entanto, eles são distintos do Evento do AEM, conforme discutido nesta documentação.

As principais distinções do evento do AEM incluem:

- O código do consumidor de eventos é executado fora do AEM, não sendo executado na mesma JVM que o AEM.
- O código de produto do AEM é responsável por definir os eventos e enviá-los para o Adobe I/O Events.
- As informações do evento são padronizadas e enviadas no formato JSON. Para obter mais detalhes, consulte [cloudevents](https://cloudevents.io/).
- Para se comunicar de volta ao AEM, o consumidor de eventos usa a API do AEM as a Cloud Service.


## Por que e quando usá-lo

O AEM Eventing oferece várias vantagens para a arquitetura do sistema e a eficiência operacional. Os principais motivos para usar o AEM Eventing incluem:

- **Para criar Arquiteturas orientadas por eventos**: facilita a criação de sistemas dispersos que podem ser dimensionados independentemente e são resilientes a falhas.
- **Código baixo e custos operacionais mais baixos**: evita personalizações no AEM, resultando em sistemas mais fáceis de manter e estender, reduzindo assim as despesas operacionais.
- **Simplifique a comunicação entre o AEM e sistemas externos**: elimina conexões ponto a ponto permitindo que a Adobe I/O Events gerencie comunicações, como determinar quais eventos do AEM devem ser entregues a sistemas ou serviços específicos.
- **Maior durabilidade dos eventos**: o Adobe I/O Events é um sistema altamente disponível e escalável, projetado para lidar com grandes volumes de eventos e entregá-los de forma confiável aos assinantes.
- **Processamento paralelo de eventos**: permite a entrega de eventos para vários assinantes simultaneamente, permitindo o processamento de eventos distribuídos em vários sistemas.
- **Desenvolvimento de aplicativos sem servidor**: oferece suporte à implantação do código de consumidor de eventos como um aplicativo sem servidor, melhorando ainda mais a flexibilidade e escalabilidade do sistema.

### Limitações

O evento do AEM, embora poderoso, tem determinadas limitações a serem consideradas:

- **Disponibilidade restrita ao AEM as a Cloud Service**: atualmente, o AEM Eventing está disponível exclusivamente para o AEM as a Cloud Service.

- **Tipos de evento disponíveis**: revise a lista atual de tipos de evento disponíveis [aqui](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#available-event-types).

## Como ativar

Consulte [Habilitar eventos do AEM no ambiente do AEM Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) para obter as próximas etapas.

## Como se inscrever

Para se inscrever nos Eventos da AEM, não é necessário gravar código no AEM, mas um projeto do [Adobe Developer Console](https://developer.adobe.com/) está configurado. O Adobe Developer Console é um gateway para APIs, SDKs, Eventos, Tempo de execução e App Builder do Adobe.

Nesse caso, um _projeto_ no Adobe Developer Console permite que você assine eventos emitidos do ambiente do AEM as a Cloud Service e configure a entrega do evento para sistemas externos.

Para obter mais informações, consulte [Como assinar Eventos da AEM na Adobe Developer Console](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Como consumir

Há dois métodos primários para consumir Eventos do AEM: o método _push_ e o método _pull_.

- **Método de push**: nessa abordagem, o consumidor de eventos é notificado proativamente pela Adobe I/O Events quando um evento se torna disponível. As opções de integração incluem Webhooks, Adobe I/O Runtime e Amazon EventBridge.
- **Método de pull**: aqui, o consumidor de eventos pesquisa ativamente a Adobe I/O Events para verificar se há novos eventos. A principal opção de integração para esse método é a API do Adobe Developer Journaling.

Para obter mais informações, consulte [Processamento de eventos do AEM via Adobe I/O Events](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Exemplos

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="Receber eventos do AEM em um webhook" src="./assets/examples/webhook/webhook-example.png"/></a>
        <div><strong><a href="./examples/webhook.md">Receber eventos do AEM em um webhook</a></strong></div>
        <p>
          Use o webhook fornecido pela Adobe para receber Eventos da AEM e revisar os detalhes do evento.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="Carregar diário de eventos do AEM" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">Carregar diário de Eventos da AEM</a></strong></div>
        <p>
          Use o aplicativo web fornecido pela Adobe para carregar Eventos da AEM do journal e revisar os detalhes do evento.
        </p>
      </td>
    </tr>
  <tr>
    <td>
        <a  href="./examples/runtime-action.md"><img alt="Ação Receber eventos do AEM no Adobe I/O Runtime" src="./assets/examples/runtime-action/eventing-runtime.png"/></a>
        <div><strong><a href="./examples/runtime-action.md">Receber Eventos do AEM na Ação do Adobe I/O Runtime</a></strong></div>
        <p>
          Receba Eventos da AEM e analise os detalhes do evento.
        </p>
      </td>
      <td>
        <a  href="./examples/event-processing-using-runtime-action.md"><img alt="Processamento de eventos do AEM usando a ação do Adobe I/O Runtime" src="./assets/examples/event-processing-using-runtime-action/event-processing.png"/></a>
        <div><strong><a href="./examples/event-processing-using-runtime-action.md">Processamento de eventos do AEM usando a Ação do Adobe I/O Runtime</a></strong></div>
        <p>
          Saiba como processar eventos AEM recebidos usando a Ação do Adobe I/O Runtime. O processamento do evento inclui o retorno de chamada do AEM, a persistência de dados do evento e a exibição deles no SPA.
        </p>
      </td>
  </tr>
  <tr>
    <td>
        <a  href="./examples/assets-pim-integration.md"><img alt="Eventos do AEM Assets para integração do PIM" src="./assets/examples/assets-pim-integration/PIM-integration-tile.png"/></a>
        <div><strong><a href="./examples/assets-pim-integration.md">Eventos do AEM Assets para integração com o PIM</a></strong></div>
        <p>
          Saiba como integrar o AEM Assets e os sistemas de Gerenciamento de informações de produtos (PIM) para atualizações de metadados.
        </p>
      </td>
  </tr> 
</table>
