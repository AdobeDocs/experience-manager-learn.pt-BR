---
title: Personalização usando o Visual Experience Composer
description: Saiba como criar uma atividade do Adobe Target usando o Visual Experience Composer.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 1%

---


# Personalização usando o Visual Experience Composer {#personalization-vec}

Saiba como criar uma atividade de teste A/B usando o Visual Experience Composer (VEC).

## Pré-requisitos

Para usar o VEC em um site do AEM, a seguinte configuração deve ser concluída:

1. [Adicionar o Adobe Target ao site do AEM](./add-target-launch-extension.md)
1. [Acionar uma chamada do Adobe Target a partir do Launch](./load-and-fire-target.md)

## Visão geral do cenário

A página inicial do site WKND exibe atividades locais ou a melhor coisa a se fazer em torno de uma cidade, na forma de cartões informativos. Como comerciante, você recebeu a tarefa de modificar a página inicial, fazendo alterações de texto no teaser da seção de aventura e entendendo como ela melhora a conversão.

## Etapas para criar um teste A/B usando o Visual Experience Composer (VEC)

1. Faça logon em [Adobe Experience Cloud](https://experience.adobe.com/), toque em __Target__, navegue até a guia __Atividades__

   + Se você não vir __Target__ no painel da Experience Cloud, verifique se a organização correta da Adobe está selecionada no alternador de organização na parte superior direita e se o usuário recebeu acesso ao Target em [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Clique no botão **Criar atividade** e escolha **Teste A/B** atividade

   ![Atividade A/B](assets/ab-target-activity.png)

1. Selecione a opção **Visual Experience Composer**, forneça o URL da atividade e clique em **Avançar**

   ![URL da atividade](assets/ab-test-url.png)

1. O Visual Experience Composer exibe duas guias do lado esquerdo depois que você cria uma nova atividade: *Experiência A* e *Experiência B*. Selecione uma experiência na lista. Você pode adicionar novas experiências à lista, usando o botão **Adicionar Experiência**.

   ![Experiência A](assets/experience.png)

1. Selecione uma imagem ou texto na página para começar a fazer modificações ou use o editor de códigos para escolher um elemento HTML.

   ![Elemento](assets/select-element.png)

1. Altere o texto de *Camping in Western Australia* para *Adventures of Australia*. Uma lista de alterações adicionadas a uma Experiência será exibida em Modificações. Você pode clicar em e editar o item modificado para exibir seu seletor de CSS e o novo conteúdo adicionado a ele.

   ![Aventuras](assets/adventures.png)

1. Renomeie *Experiência A* para *Aventura*
1. Da mesma forma, atualize o texto em *Experiência B* de *Camping in Western Australia* para *Explore the Australian Wilderness*.

   ![Explorar](assets/explore.png)

1. Clique em **Next** para ir para o Targeting e vamos manter uma alocação de tráfego manual de 50-50 entre as duas experiências.

   ![Direcionar](assets/targeting.png)

1. Para Metas e configurações, escolha a Fonte de relatórios como Adobe Target e selecione a métrica Meta como Conversão com uma ação de exibição de página.

   ![Metas](assets/goals.png)

1. Forneça um nome para a atividade e Salve.
1. Ative a atividade salva para colocar as alterações em funcionamento.

   ![Metas](assets/activate.png)

1. Abra a página do site (URL da atividade da etapa 3) em uma nova guia e você deve conseguir visualizar qualquer uma das experiências (Aventura ou Explorar) de nossa atividade de teste A/B.

   ![Metas](assets/publish.png)

## Resumo

Neste capítulo, um profissional de marketing foi capaz de criar uma experiência usando o Visual Experience Composer ao arrastar e soltar, trocar e modificar o layout e o conteúdo de uma página da Web sem alterar nenhum código para executar um teste.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
