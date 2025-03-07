---
title: Personalization da experiência completa da página da Web
description: Saiba como criar uma atividade do Target para redirecionar as páginas do site de AEM para novas páginas usando o Adobe Target.
jira: KT-6353
thumbnail: 6353-personalization-web-page.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2d201b48-c0fb-4bb4-a7d8-da9f4702e9ff
duration: 89
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Personalization da experiência completa da página da Web {#personalization-fpe}

Saiba como criar uma atividade para redirecionar as páginas do site hospedadas no AEM para uma nova página usando o Adobe Target.

## Pré-requisitos

Para personalizar páginas inteiras de um site de AEM, a seguinte configuração deve ser concluída:

1. [Adicionar o Adobe Target ao seu site AEM](./add-target-launch-extension.md)
1. [Acionar uma chamada Adobe Target a partir de tags](./load-and-fire-target.md)

## Visão geral do cenário

O site da WKND reprojetou sua página inicial e gostaria de redirecionar os visitantes da página inicial atual para a nova página inicial. Ao mesmo tempo, você também entende como a página inicial reprojetada ajuda a melhorar o envolvimento do usuário e a receita. Como profissional de marketing, você recebeu a tarefa de criar uma atividade para redirecionar os visitantes para a nova página inicial. Vamos explorar a página inicial do site WKND e aprender a criar uma atividade usando o Adobe Target.

## Etapas para criar um teste A/B usando o Visual Experience Composer (VEC)

1. Faça logon no Adobe Target e navegue até a guia Atividades
1. Clique no botão **Criar atividade** e escolha a atividade **Teste A/B**

   ![Atividade A/B](assets/ab-target-activity.png)

1. Selecione a opção **Visual Experience Composer**, forneça a URL da atividade e clique em **Avançar**

   ![URL da atividade](assets/ab-test-url.png)

1. O Visual Experience Composer exibe duas guias no lado esquerdo depois que você cria uma atividade: *Experiência A* e *Experiência B*. Selecione uma experiência na lista. Você pode adicionar novas experiências à lista usando o botão **Adicionar experiência**.

   ![Opções de experiência](assets/experience-options.png)

1. Exiba as opções disponíveis para a Experiência A e selecione a opção **Redirecionar para URL** e forneça uma URL para a nova home page do Site WKND.

   ![URL de redirecionamento](assets/redirect-url.png)

1. Renomear a *Experiência A* como *Nova Página Inicial da WKND* e *Experiência B* como *Página Inicial da WKND*

   ![Aventuras](assets/new-experiences.png)

1. Clique em **Avançar** para ir para Direcionamento e manter uma alocação de tráfego manual de 50 a 50 entre as duas experiências.

   ![Direcionamento](assets/targeting.png)

1. Para Metas e configurações, escolha a origem de Relatório como Adobe Target e selecione a métrica Meta como Conversão com uma ação de exibição de página.

   ![Metas](assets/goals.png)

1. Forneça um nome para a atividade e clique em Salvar.
1. Ative a atividade salva para ativar as alterações.

   ![Metas](assets/activate.png)

1. Abra a página do site (URL da atividade a partir da etapa 3) em uma nova guia e você deverá conseguir visualizar qualquer uma das experiências (Página inicial da WKND ou Nova página inicial da WKND) a partir de nossa atividade de teste A/B. `us/en.html` redireciona para `us/home.html`.

   ![Metas](assets/redirect-test.png)

## Resumo

Como profissional de marketing, você conseguiu criar uma atividade para redirecionar as páginas do site hospedadas no AEM para uma nova página usando o Adobe Target.

## Links de suporte

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
