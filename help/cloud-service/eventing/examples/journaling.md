---
title: Eventos de registro e AEM
description: Saiba como recuperar o conjunto inicial de eventos do AEM do journal e explorar os detalhes sobre cada evento.
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 280
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
exl-id: 33eb0757-f0ed-4c2d-b8b9-fa6648e87640
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---

# Eventos de registro e AEM

Saiba como recuperar o conjunto inicial de eventos do AEM do journal e explorar os detalhes sobre cada evento.

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

O registro em log é um método de recebimento para consumir Eventos da AEM, e um registro em log é uma lista ordenada de eventos. Usando a API do Adobe I/O Events Journaling, você pode buscar os Eventos da AEM no journal e processá-los no aplicativo. Essa abordagem permite gerenciar eventos com base em uma cadência especificada e processá-los com eficiência em massa. Consulte o [Registro em log](https://developer.adobe.com/events/docs/guides/journaling_intro/) para obter insights detalhados, incluindo considerações essenciais como períodos de retenção, paginação e muito mais.

No projeto do Adobe Developer Console, cada registro de evento é ativado automaticamente para registro em log, permitindo uma integração perfeita.

Neste exemplo, o uso de um _aplicativo Web hospedado_ fornecido pela Adobe permite buscar o primeiro lote de eventos do AEM no diário sem a necessidade de configurar seu aplicativo. Este aplicativo Web fornecido pela Adobe está hospedado em [Glitch](https://glitch.com/), uma plataforma conhecida por oferecer um ambiente baseado na Web propício à criação e implantação de aplicativos Web. No entanto, a opção de usar seu próprio aplicativo também está disponível, se preferir.

## Pré-requisitos

Para concluir este tutorial, você precisa:

- Ambiente AEM as a Cloud Service com [Evento AEM habilitado](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Projeto do Adobe Developer Console configurado para AEM Events](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

## Acessar aplicativo web

Para acessar o aplicativo web fornecido pela Adobe, siga estas etapas:

- Verifique se você pode acessar o [Glitch - aplicativo Web hospedado](https://indigo-speckle-antler.glitch.me/) em uma nova guia do navegador.

  ![Falha - aplicativo Web hospedado](../assets/examples/journaling/glitch-hosted-web-application.png)

## Coletar detalhes do projeto do Adobe Developer Console

Para buscar os Eventos da AEM no diário, credenciais como _IMS Organization ID_, _Client ID_ e _Access Token_ são necessárias. Para coletar essas credenciais, siga estas etapas:

- Na [Adobe Developer Console](https://developer.adobe.com), navegue até o projeto e clique em para abri-lo.

- Na seção **Credenciais**, clique no link **Servidor para servidor OAuth** para abrir a guia **Detalhes das credenciais**.

- Clique no botão **Gerar token de acesso** para gerar o token de acesso.

  ![Token de acesso de geração de projeto do Adobe Developer Console](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- Copie o **token de acesso gerado**, a **ID do CLIENTE** e a **ID da ORGANIZAÇÃO**. Você precisa deles mais tarde neste tutorial.

  ![Credenciais de Cópia de Projeto do Adobe Developer Console](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- Cada registro de evento é ativado automaticamente para registro em log. Para obter o _ponto de extremidade exclusivo da API de registro em diário_ do seu registro de evento, clique no cartão de evento que é assinante do AEM Events. Na guia **Detalhes do Registro**, copie o **PONTO DE EXTREMIDADE DE API ÚNICO DO JOURNALING**.

  ![Cartão de Eventos do Adobe Developer Console Project](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## Carregar diário de eventos do AEM

Para simplificar, esse aplicativo Web hospedado busca somente o primeiro lote de eventos do AEM no journal. Esses são os eventos mais antigos disponíveis no journal. Para obter mais detalhes, consulte [primeiro lote de eventos](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- No [Falha - aplicativo Web hospedado](https://indigo-speckle-antler.glitch.me/), digite a **ID da Organização IMS**, a **ID do Cliente** e o **Token de Acesso** copiados anteriormente do projeto do Adobe Developer Console e clique em **Enviar**.

- Após o sucesso, o componente de tabela exibe os dados do diário de eventos do AEM.

  ![Dados do Diário de Eventos da AEM](../assets/examples/journaling/load-journal.png)

- Para exibir a carga útil completa do evento, clique duas vezes na linha. Você pode ver que os detalhes do evento do AEM têm todas as informações necessárias para processar o evento no webhook. Por exemplo, o tipo de evento (`type`), a origem do evento (`source`), a ID do evento (`event_id`), a hora do evento (`time`) e os dados do evento (`data`).

  ![Carga Concluída do Evento AEM](../assets/examples/journaling/complete-journal-data.png)

## Recursos adicionais

- [O código de origem do webhook com falha](https://glitch.com/edit/#!/indigo-speckle-antler) está disponível para referência. É um aplicativo simples do React que usa componentes do [Adobe React Spectrum](https://react-spectrum.adobe.com/react-spectrum/index.html) para renderizar a interface.

- A [API do Adobe I/O Events Journaling](https://developer.adobe.com/events/docs/guides/api/journaling_api/) fornece informações detalhadas sobre a API, como primeiro, próximo e último lote de eventos, paginação e muito mais.
