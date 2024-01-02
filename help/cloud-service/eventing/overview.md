---
title: Evento AEM
description: Saiba mais sobre eventos de AEM, o que é, por que e quando usá-los e exemplos deles.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 0
last-substantial-update: 2023-12-07T00:00:00Z
jira: KT-14649
thumbnail: KT-14649.jpeg
source-git-commit: 839d552199fe7d10a0cde4011bdfe8cf42cc8ec9
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 0%

---


# Evento AEM

Saiba mais sobre eventos de AEM, o que é, por que e quando usá-los e exemplos deles.

>[!VIDEO](https://video.tv.adobe.com/v/3426686?quality=12&learn=on)

>[!IMPORTANT]
>
>O evento as a Cloud Service de AEM só está disponível para usuários registrados no modo de pré-lançamento. Para habilitar o evento de AEM em seu ambiente as a Cloud Service AEM, entre em contato com [Equipe de evento do AEM](mailto:grp-aem-events@adobe.com).

## O que é

O AEM Eventing é um sistema de eventos nativo em nuvem que permite assinaturas de eventos AEM para processamento em sistemas externos. Um evento AEM é uma notificação de alteração de estado enviada pelo AEM sempre que uma ação específica ocorre. Por exemplo, isso pode incluir eventos quando um fragmento de conteúdo é criado, atualizado ou excluído.

![Evento AEM](./assets/aem-eventing.png)

O diagrama acima visualizava como o AEM as a Cloud Service produz eventos e os envia para os Eventos Adobe I/O, que por sua vez os expõem aos assinantes de eventos.

Em resumo, há três componentes principais:

1. **Provedor de eventos:** AEM as a Cloud Service.
1. **Eventos Adobe I/O:** Plataforma de desenvolvedor para integrar, estender e criar aplicativos e experiências com base em produtos e tecnologias Adobe.
1. **Consumidor de evento:** Sistemas de propriedade do cliente que se inscreve nos Eventos AEM. Por exemplo, um CRM (Customer Relationship Management), PIM (Product Information Management), OMS (Order Management System) ou um aplicativo personalizado.

### Como é diferente

A variável [Evento do Apache Sling](https://sling.apache.org/documentation/bundles/apache-sling-eventing-and-job-handling.html), eventos OSGi e [Observação JCR](https://jackrabbit.apache.org/oak/docs/features/observation.html) todos os mecanismos de oferta para assinar e processar eventos. No entanto, eles são distintos do evento AEM, conforme discutido nesta documentação.

As principais distinções do evento AEM incluem:

- O código do consumidor do evento é executado fora do AEM, não sendo executado na mesma JVM que o AEM.
- O código de produto AEM é responsável por definir os eventos e enviá-los para Eventos Adobe I/O.
- As informações do evento são padronizadas e enviadas no formato JSON. Para obter mais detalhes, consulte [cloudevents](https://cloudevents.io/).
- Para se comunicar de volta ao AEM, o consumidor do evento utiliza a API as a Cloud Service AEM.


## Por que e quando usá-lo

O evento AEM oferece inúmeras vantagens para a arquitetura do sistema e a eficiência operacional. Os principais motivos para usar o evento AEM incluem:

- **Para criar arquiteturas orientadas por eventos**: facilita a criação de sistemas com acoplamento flexível que podem ser dimensionados de maneira independente e são resilientes a falhas.
- **Código baixo e custos operacionais mais baixos**: evita personalizações no AEM, resultando em sistemas mais fáceis de manter e estender, reduzindo assim as despesas operacionais.
- **Simplificar a comunicação entre o AEM e sistemas externos**: elimina conexões ponto a ponto, permitindo que os Eventos Adobe I/O gerenciem comunicações, como determinar quais eventos AEM devem ser entregues a sistemas ou serviços específicos.
- **Maior durabilidade dos eventos**: o Adobe I/O Events é um sistema altamente disponível e escalável, projetado para lidar com grandes volumes de eventos e entregá-los de maneira confiável aos assinantes.
- **Processamento paralelo de eventos**: permite o delivery de eventos a vários assinantes simultaneamente, permitindo o processamento de eventos distribuídos em vários sistemas.
- **Desenvolvimento de aplicativos sem servidor**: suporta a implantação do código do consumidor de eventos como um aplicativo sem servidor, melhorando ainda mais a flexibilidade e escalabilidade do sistema.

### Limitações

O evento AEM, embora poderoso, tem certas limitações a serem consideradas:

- **Disponibilidade restrita ao AEM as a Cloud Service**: Atualmente, o evento AEM está disponível exclusivamente para AEM as a Cloud Service.
- **Suporte limitado a eventos**: a partir de agora, somente os eventos de Fragmento de conteúdo AEM são compatíveis. No entanto, espera-se que o escopo se expanda com a adição de mais eventos no futuro.

## Como ativar

O evento de AEM é ativado por ambiente as a Cloud Service AEM e só está disponível para ambientes no modo de pré-lançamento. Contato [Equipe de evento do AEM](mailto:grp-aem-events@adobe.com) para habilitar seu ambiente AEM com evento AEM.

Se já estiver ativado, consulte [Ativar eventos de AEM no ambiente AEM Cloud Service](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) para as próximas etapas.

## Como se inscrever

Para se inscrever nos Eventos AEM, não é necessário escrever nenhum código no AEM, mas sim um [Console do Adobe Developer](https://developer.adobe.com/) projeto está configurado. O Adobe Developer Console é um gateway para APIs Adobe, SDKs, Eventos, Tempo de execução e Construtor de aplicativos.

Neste caso, uma _projeto_ no Adobe Developer Console, é possível assinar eventos emitidos de ambientes as a Cloud Service AEM e configurar o delivery de eventos em sistemas externos.

Para obter mais informações, consulte [Como se inscrever em eventos AEM no console Adobe Developer](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Como consumir

Existem dois métodos primários para o consumo de eventos AEM: o _push_ e o _obter_ método.

- **método push**: nessa abordagem, o consumidor do evento é notificado proativamente pelos Eventos do Adobe I/O quando um evento se torna disponível. As opções de integração incluem Webhooks, Adobe I/O Runtime e Amazon EventBridge.
- **Método Pull**: aqui, o consumidor de eventos pesquisa ativamente os Eventos do Adobe I/O para verificar se há novos eventos. A principal opção de integração para este método é a API de registro em log de Adobe I/O.

Para obter mais informações, consulte [Processamento de eventos AEM por meio de eventos Adobe I/O](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#aem-events-processing-via-adobe-io).

## Exemplos

<table>
  <tr>
    <td>
        <a  href="./examples/webhook.md"><img alt="Receber eventos de AEM em um webhook" src="./assets/examples/webhook/Eventing-webhook.png"/></a>
        <div><strong><a href="./examples/webhook.md">Receber eventos de AEM em um webhook</a></strong></div>
        <p>
          Use o webhook fornecido pelo Adobe para receber Eventos AEM e revisar os detalhes do evento.
        </p>
      </td>
      <td>
        <a  href="./examples/journaling.md"><img alt="Carregar diário de eventos AEM" src="./assets/examples/journaling/eventing-journal.png"/></a>
        <div><strong><a href="./examples/journaling.md">Carregar diário de eventos AEM</a></strong></div>
        <p>
          Use o aplicativo da Web fornecido pelo Adobe para carregar Eventos AEM do journal e revisar os detalhes do evento.
        </p>
      </td>
    </tr>
</table>
