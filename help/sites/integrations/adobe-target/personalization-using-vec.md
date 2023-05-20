---
title: Personalização usando o Visual Experience Composer
description: Saiba como criar uma atividade do Adobe Target usando o Visual Experience Composer.
topics: integrations, authoring, personalization, activity, offers
audience: all
doc-type: feature video
activity: use
version: Cloud Service
kt: 6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 2%

---

# Personalização usando o Visual Experience Composer {#personalization-vec}

Saiba como criar uma Atividade de teste A/B usando o Visual Experience Composer (VEC).

## Pré-requisitos

Para usar o VEC em um site de AEM, a seguinte configuração deve ser concluída:

1. [Adicionar o Adobe Target ao seu site AEM](./add-target-launch-extension.md)
1. [Acionar uma chamada do Adobe Target no Launch](./load-and-fire-target.md)

## Visão geral do cenário

A página inicial do site WKND exibe atividades locais ou a melhor coisa a ser feita em torno de uma cidade na forma de cartões informativos. Como profissional de marketing, você recebeu a tarefa de modificar a página inicial, fazendo alterações de texto no teaser da seção de aventura e entendendo como ela melhora a conversão.

## Etapas para criar um teste A/B usando o Visual Experience Composer (VEC)

1. Fazer logon em [Adobe Experience Cloud](https://experience.adobe.com/), toque em __Target__, navegue até o __Atividades__ guia

   + Se você não vir __Target__ no painel Experience Cloud, verifique se a organização correta do Adobe está selecionada no alternador de organização na parte superior direita e se o acesso ao Target foi concedido ao usuário no [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Clique em **Criar atividade** e escolha **Teste A/B** atividade

   ![Atividade A/B](assets/ab-target-activity.png)

1. Selecione o **Visual Experience Composer** , forneça o URL da atividade e clique em **Próxima**

   ![URL da atividade](assets/ab-test-url.png)

1. O Visual Experience Composer exibe duas guias no lado esquerdo depois que você cria uma nova atividade: *Experiência A* e *Experiência B*. Selecione uma experiência na lista. É possível adicionar novas experiências à lista, usando o **Adicionar experiência** botão.

   ![Experiência A](assets/experience.png)

1. Selecione uma imagem ou texto na página para começar a fazer modificações ou use o editor de código para escolher e HTML elemento.

   ![Elemento](assets/select-element.png)

1. Alterar o texto de *Acampamento em Western Australia* para *Aventuras da Austrália*. Uma lista de alterações adicionadas a uma experiência é exibida em Modificações. Você pode clicar em e editar o item modificado para exibir seu seletor de CSS e o novo conteúdo adicionado a ele.

   ![Aventuras](assets/adventures.png)

1. Renomear *Experiência A* para *Aventura*
1. Do mesmo modo, atualize o texto em *Experiência B* de *Acampamento em Western Australia* para *Explore a natureza selvagem australiana*.

   ![Explorar](assets/explore.png)

1. Clique em **Próxima** para ir para Direcionamento e vamos manter uma alocação manual de tráfego de 50 a 50 entre as duas experiências.

   ![Direcionar](assets/targeting.png)

1. Para Metas e configurações, escolha a origem de Relatório como Adobe Target e selecione a métrica Meta como Conversão com uma ação de exibição de página.

   ![Metas](assets/goals.png)

1. Forneça um nome para a atividade e clique em Salvar.
1. Ative a atividade salva para ativar as alterações.

   ![Metas](assets/activate.png)

1. Abra a página do site (URL da atividade a partir da etapa 3) em uma nova guia e você deverá conseguir visualizar qualquer uma das experiências (Aventura ou Explorar) da nossa atividade de teste A/B.

   ![Metas](assets/publish.png)

## Resumo

Neste capítulo, um profissional de marketing conseguiu criar uma experiência usando o Visual Experience Composer arrastando e soltando, alternando e modificando o layout e o conteúdo de uma página da Web sem alterar nenhum código para executar um teste.

## Links de suporte

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)
