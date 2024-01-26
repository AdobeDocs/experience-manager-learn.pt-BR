---
title: Introdução ao AEM e ao Adobe Target
description: Um tutorial completo mostrando como criar e fornecer experiências personalizadas usando o Adobe Experience Manager e o Adobe Target. Neste tutorial, você também aprenderá sobre as diferentes personalidades envolvidas no processo completo e como elas colaboram entre si
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 219
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '846'
ht-degree: 0%

---

# Integrar o AEM Sites e o Adobe Target {#getting-started-with-aem-target}

O AEM e o Target são soluções eficientes com recursos aparentemente sobrepostos. Às vezes, os clientes têm dificuldades em saber como e quando usar esses produtos em conjunto para fornecer uma experiência personalizada. Para fornecer uma experiência otimizada para cada usuário final, equipes diferentes em sua organização devem trabalhar em conjunto e definir quem faz o quê.

Neste tutorial, abordamos três cenários diferentes para AEM e Target, que ajudam você a entender o que funciona melhor para sua organização e como equipes diferentes colaboram.

* Cenário 1: personalização usando fragmentos de experiência de AEM
* Cenário 2: personalização usando o Visual Experience Composer
* Cenário 3: personalização de experiências completas de página da Web

## Personalização usando fragmentos de experiência do AEM {#personalization-using-aem-experience-fragment}

Neste cenário, usaremos AEM e Target. Claramente, ambos os produtos têm seus próprios pontos fortes e, quando se trata de fornecer experiências personalizadas aos usuários do seu site, você precisa **conteúdo personalizado (conteúdo do AEM)** e uma **forma inteligente (Target)** para distribuir esse conteúdo com base em um usuário específico.

O AEM ajuda você a criar conteúdo personalizado, reunindo todo o seu conteúdo e ativos em um local central para alimentar sua estratégia de personalização. O AEM permite que você crie conteúdo facilmente para desktops, tablets e dispositivos móveis em um único local sem escrever código. Não há necessidade de criar páginas para cada dispositivo. O AEM ajusta automaticamente cada experiência usando seu conteúdo. Você também pode exportar o conteúdo do AEM para o Adobe Target como ofertas pressionando um botão.

Agora temos conteúdo personalizado na forma de ofertas de AEM no Target. O Target permite entregar essas ofertas em escala com base em uma combinação de abordagens de aprendizagem de máquina baseadas em regras e AI que incorporam variáveis comportamentais, contextuais e offline.  Com o Target, você pode configurar e executar facilmente atividades A/B e multivariadas (MVT) para determinar as melhores ofertas, experiências e conteúdo.

**Fragmentos de experiência** representam um enorme passo à frente para vincular os criadores de conteúdo/experiência aos profissionais de personalização que impulsionam os resultados de negócios usando o Target.

* Autores do editor de conteúdo AEM personalizaram o conteúdo como Fragmentos de experiência e suas variações
* O AEM exporta o HTML de fragmento de experiência para o Target&#x200B;
* Target&#x200B; usa a marcação de fragmento de experiência AEM como ofertas nas atividades
* O Target fornece o HTML do fragmento de experiência, o AEM fornece imagens referenciadas

  ![Personalização usando o diagrama de Fragmentos de experiência](assets/personalization-use-case-1/use-case-1-diagram.png)

**Para implementar esse cenário, é necessário:**

* [Integrar AEM e Adobe Target usando o Launch e o Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM e Adobe Target usando Cloud Service herdados](./implementation.md#integrating-aem-target-options)

***Depois de implementar as integrações acima, vamos explorar as [cenário em detalhes](./personalization-use-case-1.md).***

## Personalização usando o Visual Experience Composer

Os profissionais de marketing podem fazer alterações rápidas em seu site sem alterar nenhum código para executar um teste usando o Adobe Target Visual Experience Composer (VEC). O VEC é uma interface WYSIWYG (o que você vê é o que você obtém) que permite criar e testar facilmente experiências e ofertas personalizadas no contexto do site. Você pode criar experiências e ofertas para atividades do Target arrastando e soltando, trocando e modificando o layout e o conteúdo de uma página da Web (ou oferta) ou página da Web móvel.

O VEC é um dos principais recursos do Adobe Target. O VEC permite aos profissionais de marketing e designers criarem e alterarem o conteúdo usando uma interface visual. Muitas opções de design podem ser feitas sem a necessidade de editar diretamente o código. A edição de HTML e JavaScript também é possível usando as opções de edição disponíveis no compositor.

* O conteúdo reside no AEM e os editores de conteúdo criam e gerenciam as páginas do site
* O Target usa páginas de site hospedadas no AEM para executar testes e personalização
* O Target fornece conteúdo personalizado
* O novo conteúdo de rede é criado usando o Adobe Target VEC
* Aplicável a sites hospedados pelo AEM e sites não hospedados pelo AEM

  ![Personalização usando o diagrama do Visual Experience Composer](assets/personalization-use-case-3/use-case-diagram-3.png)

**Para implementar esse cenário, é necessário:**

* [Integrar AEM e Adobe Target usando o Launch e o Adobe I/O](./implementation.md#integrating-aem-target-options)

***Depois de implementar a integração acima, vamos explorar as [cenário em detalhes.](./personalization-use-case-3.md)***

## Personalização de experiências completas de página da Web

A integração do Adobe Experience Manager com o Adobe Target ajuda a fornecer uma experiência personalizada aos usuários do site. Além disso, também ajuda você a entender melhor quais versões do conteúdo do seu site melhoram suas conversões durante um período de teste especificado. Por exemplo, um teste A/B compara duas ou mais versões do conteúdo do site para ver qual eleva melhor suas conversões, vendas ou outras métricas identificadas. Um profissional de marketing pode criar atividades no Adobe Target para entender como os usuários interagem com o conteúdo do site e como ele afeta as métricas do site.

* O conteúdo reside no AEM e os editores de conteúdo criam e gerenciam as páginas do site
* O Target usa páginas de site hospedadas no AEM para executar testes e personalização
* O Target fornece conteúdo personalizado
* Nenhum conteúdo novo é criado aqui
* AEM Aplica-se a locais com ou sem AEM

  ![diagrama](assets/personalization-use-case-2/use-case-2-diagram.png)

**Para implementar esse cenário, é necessário:**

* [Integrar AEM e Adobe Target usando o Launch e o Adobe I/O](./implementation.md#integrating-aem-target-options)

***Depois de implementar a integração acima, vamos explorar as [cenário em detalhes.](./personalization-use-case-2.md)***
