---
title: Personalização da experiência completa da página da Web
description: Saiba como criar uma atividade para redirecionar as páginas do site hospedadas em AEM para uma nova página usando o Adobe Target.
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6353
thumbnail: 6353-personalization-web-page.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---


# Personalização da experiência completa da página da Web {#personalization-fpe}

Saiba como criar uma atividade para redirecionar as páginas do site hospedadas em AEM para uma nova página usando o Adobe Target.

## Visão geral do cenário

O site da WKND reprojetou seu home page e gostaria de redirecionar seus visitantes atuais para o novo home page. Ao mesmo tempo, também saiba como o home page reprojetado ajuda a melhorar a participação e a receita do usuário. Como comerciante, você recebeu a tarefa para criar uma atividade para redirecionar os visitantes para o novo home page. Vamos explorar o home page do site da WKND e aprender a criar uma atividade usando o Adobe Target.

## Etapas para criar um teste A/B usando o Visual Experience Composer (VEC)

1. Faça logon no Adobe Target e navegue até a guia Atividade
2. Clique no botão **Criar Atividade** e escolha **atividade de teste** A/B

   ![Atividade A/B](assets/ab-target-activity.png)

3. Selecione a opção **Visual Experience Composer** , forneça o URL da Atividade e clique em **Avançar**

   ![URL de atividade](assets/ab-test-url.png)

4. O Visual Experience Composer exibe duas guias no lado esquerdo após a criação de uma nova atividade: *Experiência A* e *Experiência B*. Selecione uma experiência na lista. Você pode adicionar novas experiências à lista usando o botão **Adicionar experiência** .

   ![Opções de experiência](assets/experience-options.png)

5. Opções de visualização disponíveis para a Experiência A e, em seguida, selecione a opção **Redirecionar para URL** e forneça um URL para o novo home page do site WKND.

   ![URI de redirecionamento](assets/redirect-url.png)

6. Renomeie a *experiência A* para o *novo Home page* WKND e a *experiência B* para o Home page *WKND*

   ![Aventuras](assets/new-experiences.png)

7. Clique em **Avançar** para ir para Definição de metas e manter uma alocação de tráfego Manual de 50 a 50 entre as duas experiências.

   ![Direcionar](assets/targeting.png)

8. Para Metas e configurações, escolha a fonte do Relatórios como Adobe Target e selecione a métrica Meta como Conversão com uma ação de visualização de página.

   ![Metas](assets/goals.png)

9. Forneça um nome para sua atividade e Salvar.
10. Ative sua atividade salva para colocar suas alterações em execução.

   ![Metas](assets/activate.png)

11. Abra a página do site (URL da Atividade da etapa 3) em uma nova guia e você deve ser capaz de visualização de qualquer uma das experiências (Home page WKND ou Novo Home page WKND) da nossa atividade de teste A/B. `us/en.html` redireciona para `us/home.html`.

   ![Metas](assets/redirect-test.png)

## Resumo

Como profissional de marketing, foi possível criar uma atividade para redirecionar as páginas do site hospedadas em AEM para uma nova página usando o Adobe Target.

## Links de suporte

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)

