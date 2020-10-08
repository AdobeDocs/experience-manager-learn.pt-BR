---
title: Personalização usando o Visual Experience Composer
description: Saiba como criar uma Atividade Adobe Target usando o Visual Experience Composer.
feature: targeting
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
translation-type: tm+mt
source-git-commit: 7a830d5a04ce53014b86f9f05238dd64f79edffc
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 0%

---


# Personalização usando o Visual Experience Composer {#personalization-vec}

Saiba como criar uma Atividade de teste A/B usando o Visual Experience Composer (VEC).

Antes de criar uma Atividade no Público alvo, é necessário fazer a configuração:

1. [Integrar Experience Platform Launch e AEM](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html)
2. [Integre o Adobe Experience Manager ao Adobe Target usando Cloud Services](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/target/setup-aem-target-cloud-service.html)

## Visão geral do cenário

O home page do site WKND exibe atividades locais ou a melhor coisa a se fazer em uma cidade na forma de cartões informativos. Como comerciante, você recebeu a tarefa para modificar o home page, fazendo alterações no texto da seção de aventura e entendendo como ela melhora a conversão.

## Etapas para criar um teste A/B usando o Visual Experience Composer (VEC)

1. Faça logon no Adobe Target e navegue até a guia Atividade
1. Clique no botão **Criar Atividade** e escolha **atividade de teste** A/B

   ![Atividade A/B](assets/ab-target-activity.png)

1. Selecione a opção **Visual Experience Composer** , forneça o URL da Atividade e clique em **Avançar**

   ![URL de atividade](assets/ab-test-url.png)

1. O Visual Experience Composer exibe duas guias no lado esquerdo após a criação de uma nova atividade: *Experiência A* e *Experiência B*. Selecione uma experiência na lista. Você pode adicionar novas experiências à lista usando o botão **Adicionar experiência** .

   ![Experiência A](assets/experience.png)

1. Selecione uma imagem ou texto na sua página para fazer modificações ou use o editor de código para selecionar um elemento HTML.

   ![Elemento](assets/select-element.png)

1. Altere o texto de *Camping in Western Australia* para *Adventures of Australia*. Uma lista de alterações adicionadas a uma Experiência será exibida em Modificações. Você pode clicar e editar o item modificado para visualização em seu seletor de CSS e o novo conteúdo adicionado a ele.

   ![Aventuras](assets/adventures.png)

1. Renomeie a *Experiência A* para *Aventura*
1. Da mesma forma, atualize o texto sobre a *Experiência B* de *Camping no oeste da Austrália* para *Explorar a natureza selvagem* australiana.

   ![Explorar](assets/explore.png)

1. Clique em **Avançar** para ir para Definição de metas e vamos manter uma alocação de tráfego Manual de 50 a 50 entre as duas experiências.

   ![Direcionar](assets/targeting.png)

1. Para Metas e configurações, escolha a fonte do Relatórios como Adobe Target e selecione a métrica Meta como Conversão com uma ação de visualização de página.

   ![Metas](assets/goals.png)

1. Forneça um nome para sua atividade e Salvar.
1. Ative sua atividade salva para colocar suas alterações em execução.

   ![Metas](assets/activate.png)

1. Abra a página do site (URL da Atividade da etapa 3) em uma nova guia e você deve ser capaz de visualização de qualquer uma das experiências (Adventure ou Explore) de nossa atividade de teste A/B.

   ![Metas](assets/publish.png)

## Resumo

Neste capítulo, um profissional de marketing conseguiu criar uma experiência usando o Visual Experience Composer arrastando e soltando, trocando e modificando o layout e o conteúdo de uma página da Web sem alterar nenhum código para executar um teste.

## Links de suporte

* [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
* [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
