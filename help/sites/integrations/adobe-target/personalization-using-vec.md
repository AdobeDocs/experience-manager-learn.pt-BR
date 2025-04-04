---
title: Personalization usando o Visual Experience Composer
description: Saiba como criar uma atividade do Adobe Target usando o Visual Experience Composer.
version: Experience Manager as a Cloud Service
jira: KT-6352
thumbnail: 6352-personalization-using-vec.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: becf2bed-0541-45e8-9ce2-f9fb023234e0
duration: 101
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Personalization usando o Visual Experience Composer {#personalization-vec}

Saiba como criar uma Atividade de teste A/B usando o Visual Experience Composer (VEC).

## Pré-requisitos

Para usar o VEC em um site do AEM, a seguinte configuração deve ser concluída:

1. [Adicionar o Adobe Target ao seu site da AEM](./add-target-launch-extension.md)
1. [Acionar uma chamada Adobe Target a partir de tags](./load-and-fire-target.md)

## Visão geral do cenário

A página inicial do site WKND exibe atividades locais ou as melhores coisas a serem feitas em torno de uma cidade na forma de cartões informativos. Como profissional de marketing, você recebeu a tarefa de modificar a página inicial, fazendo alterações de texto no teaser da seção de aventura e entendendo como ela melhora a conversão.

## Etapas para criar um teste A/B usando o Visual Experience Composer (VEC)

1. Faça logon no [Adobe Experience Cloud](https://experience.adobe.com/), toque em __Target__, navegue até a guia __Atividades__

   + Se você não vir __Target__ no painel do Experience Cloud, verifique se a organização correta da Adobe está selecionada no alternador de organização na parte superior direita e se o usuário recebeu acesso ao Target no [Adobe Admin Console](https://adminconsole.adobe.com/).

1. Clique no botão **Criar atividade** e escolha a atividade **Teste A/B**

   ![Atividade A/B](assets/ab-target-activity.png)

1. Selecione a opção **Visual Experience Composer**, forneça a URL da atividade e clique em **Avançar**

   ![URL da atividade](assets/ab-test-url.png)

1. O Visual Experience Composer exibe duas guias no lado esquerdo depois que você cria uma atividade: *Experiência A* e *Experiência B*. Selecione uma experiência na lista. Você pode adicionar novas experiências à lista usando o botão **Adicionar experiência**.

   ![Experiência A](assets/experience.png)

1. Selecione uma imagem ou texto na página para começar a fazer modificações ou use o editor de código para escolher um elemento HTML.

   ![Elemento](assets/select-element.png)

1. Altere o texto de *Camping na Austrália Ocidental* para *Aventuras da Austrália*. Uma lista de alterações adicionadas a uma experiência é exibida em Modificações. Você pode clicar em e editar o item modificado para exibir seu seletor de CSS e o novo conteúdo adicionado a ele.

   ![Aventuras](assets/adventures.png)

1. Renomear a *Experiência A* para *Aventura*
1. Da mesma forma, atualize o texto sobre *Experiência B* de *Camping na Austrália Ocidental* para *Explorar a Natureza da Austrália*.

   ![Explorar](assets/explore.png)

1. Clique em **Avançar** para ir para Direcionamento e vamos manter uma alocação manual de tráfego de 50 a 50 entre as duas experiências.

   ![Direcionamento](assets/targeting.png)

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

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
