---
title: Personalização da experiência completa da página da Web
description: Saiba como criar uma atividade de Público alvo para redirecionar suas páginas AEM do site para novas páginas usando o Adobe Target.
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 1%

---


# Personalização da experiência completa da página da Web {#personalization-fpe}

Saiba como criar uma atividade para redirecionar as páginas do site hospedadas em AEM para uma nova página usando o Adobe Target.

## Pré-requisitos

Para personalizar as páginas completas de um site AEM, é necessário concluir a seguinte configuração:

1. [Adicionar Adobe Target ao seu site AEM](./add-target-launch-extension.md)
1. [Acionar uma chamada do Adobe Target do Launch](./load-and-fire-target.md)

## Visão geral do cenário

O site da WKND reprojetou seu home page e gostaria de redirecionar seus visitantes atuais para o novo home page. Ao mesmo tempo, também saiba como o home page reprojetado ajuda a melhorar a participação e a receita do usuário. Como comerciante, você recebeu a tarefa para criar uma atividade para redirecionar os visitantes para o novo home page. Vamos explorar o home page do site da WKND e aprender a criar uma atividade usando o Adobe Target.

## Etapas para criar um teste A/B usando o Visual Experience Composer (VEC)

1. Faça logon no Adobe Target e navegue até a guia Atividade
1. Clique no botão **Criar Atividade** e escolha **Teste A/B** atividade

   ![Atividade A/B](assets/ab-target-activity.png)

1. Selecione a opção **Visual Experience Composer**, forneça o URL da Atividade e clique em **Avançar**

   ![URL de atividade](assets/ab-test-url.png)

1. O Visual Experience Composer exibe duas guias no lado esquerdo após a criação de uma nova atividade: *Experiência A* e *Experiência B*. Selecione uma experiência na lista. Você pode adicionar novas experiências à lista usando o botão **Adicionar experiência**.

   ![Opções de experiência](assets/experience-options.png)

1. Opções de visualização disponíveis para a Experiência A e, em seguida, selecione a opção **Redirecionar para URL** e forneça um URL para o novo home page do site WKND.

   ![URI de redirecionamento](assets/redirect-url.png)

1. Renomear *Experiência A* para *Novo Home page WKND* e *Experiência B* para *Home page WKND*

   ![Aventuras](assets/new-experiences.png)

1. Clique em **Próximo** para ir para Definição de metas e manter uma alocação de tráfego Manual de 50 a 50 entre as duas experiências.

   ![Direcionar](assets/targeting.png)

1. Para Metas e configurações, escolha a fonte do Relatórios como Adobe Target e selecione a métrica Meta como Conversão com uma ação de visualização de página.

   ![Metas](assets/goals.png)

1. Forneça um nome para sua atividade e Salvar.
1. Ative sua atividade salva para colocar suas alterações em execução.

   ![Metas](assets/activate.png)

1. Abra a página do site (URL da Atividade da etapa 3) em uma nova guia e você deve ser capaz de visualização de qualquer uma das experiências (Home page WKND ou Novo Home page WKND) da nossa atividade de teste A/B. `us/en.html` redireciona para  `us/home.html`.

   ![Metas](assets/redirect-test.png)

## Resumo

Como profissional de marketing, foi possível criar uma atividade para redirecionar as páginas do site hospedadas em AEM para uma nova página usando o Adobe Target.

## Links de suporte

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

