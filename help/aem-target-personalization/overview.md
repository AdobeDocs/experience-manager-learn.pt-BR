---
title: Introdução ao AEM e ao Adobe Target
seo-title: Introdução ao AEM e ao Adobe Target
description: Um tutorial completo mostrando como criar e fornecer experiências personalizadas usando o Adobe Experience Manager e o Adobe Target. Neste tutorial, você também aprenderá sobre as diferentes pessoas envolvidas no processo de ponta a ponta e como elas colaboram entre si
seo-description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando Adobe Experience Manager e Adobe Target. Neste tutorial, você também aprenderá sobre as diferentes pessoas envolvidas no processo de ponta a ponta e como elas colaboram entre si
translation-type: tm+mt
source-git-commit: c4ddafe392f74be8401f3ef6e07fc9d463d7620a
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 2%

---


# Introdução ao AEM e ao Adobe Target {#getting-started-with-aem-target}

AEM e Público alvo são soluções poderosas com recursos aparentemente sobrepostos. Às vezes, os clientes se debatem com a compreensão de como e quando usar esses produtos em conjunto para fornecer experiência personalizada. Para fornecer experiência otimizada para cada usuário final, equipes diferentes em sua organização devem trabalhar em conjunto e definir quem faz o quê.

Neste tutorial, cobrimos três cenários diferentes para AEM e Público alvo, o que ajuda você a entender o que funciona melhor para sua organização e como diferentes equipes colaboram.

* Cenário 1: Personalização usando AEM fragmentos de experiência
* Cenário 2 : Personalização usando o Visual Experience Composer
* Cenário 3: Personalização de experiências completas de página da Web

## Personalização usando AEM fragmentos de experiência {#personalization-using-aem-experience-fragment}

Para esse cenário, vamos usar AEM e Público alvo. Claramente, ambos os produtos têm seus próprios pontos fortes, e quando se trata de fornecer experiências personalizadas aos usuários do site, você precisa de **conteúdo personalizado (conteúdo do AEM)** e um **modo inteligente (Público alvo)** para servir esse conteúdo com base em um usuário específico.

AEM ajuda a criar conteúdo personalizado, reunindo todo o seu conteúdo e ativos em um local central para alimentar sua estratégia de personalização. AEM permite que você crie facilmente conteúdo para desktops, tablets e dispositivos móveis em um único lugar, sem gravar código. Não há necessidade de criar páginas para cada dispositivo. AEM ajusta automaticamente cada experiência usando seu conteúdo. Você também pode exportar o conteúdo do AEM para o Adobe Target como ofertas com um botão.

Agora nós personalizamos o conteúdo na forma de Ofertas de AEM no Público alvo. O Público alvo permite que você forneça essas ofertas em escala com base em uma combinação de abordagens de aprendizado de máquina baseadas em regras e orientadas por IA que incorporam variáveis comportamentais, contextuais e offline.  Com o Público alvo, você pode facilmente configurar e executar atividades A/B e multivariadas (MVT) para determinar as melhores ofertas, conteúdo e experiências.

**Os** fragmentos de experiência representam um grande passo em frente para vincular os criadores de conteúdo/experiência aos profissionais de personalização que estão acionando os resultados da empresa usando o Público alvo.

* AEM autores do editor de conteúdo personalizaram o conteúdo como Fragmentos de experiência e suas variações
* AEM exporta o HTML do fragmento de experiência para o &#x200B; do Público alvo
* O público alvo &#x200B; usa AEM marcação Fragmento de experiência como Ofertas no Atividade
* O público alvo oferece HTML do Fragmento de experiência, AEM fornece imagens referenciadas

   ![Personalização usando o diagrama Fragmentos de experiência](assets/personalization-use-case-1/use-case-1-diagram.png)

**Para implementar esse cenário, é necessário:**

* [Integrar AEM e Adobe Target usando o Launch e o Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM e Adobe Target usando Cloud Services herdados](./implementation.md#integrating-aem-target-options)

***Depois de implementar as integrações acima, vamos explorar o  [cenário detalhadamente](./personalization-use-case-1.md).***

## Personalização usando o Visual Experience Composer

Os profissionais de marketing podem fazer alterações rápidas em seu site sem alterar qualquer código para executar um teste usando o Adobe Target Visual Experience Composer (VEC). O VEC é a interface do usuário WYSIWYG (o que você vê é o que você obtém) que permite criar e testar facilmente experiências e Ofertas personalizadas no contexto do site. Você pode criar experiências e Ofertas para atividades de Públicos alvos arrastando e soltando, alternando e modificando o layout e o conteúdo de uma página da Web (ou Oferta) ou página da Web móvel.

O VEC é um dos principais recursos do Adobe Target. O VEC permite que profissionais de marketing e designers criem e alterem o conteúdo usando uma interface visual. Muitas opções de design podem ser feitas sem a necessidade de edição direta do código. Editar HTML e JavaScript também é possível usando as opções de edição disponíveis no compositor.

* O conteúdo reside em AEM e os editores de conteúdo criam e gerenciam as páginas do site
* O público alvo usa AEM páginas do site hospedado para executar testes e personalização
* O público alvo oferece conteúdo personalizado
* O novo conteúdo líquido é criado usando o Adobe Target VEC
* Aplica-se a sites hospedados AEM e não AEM

   ![Personalização usando o diagrama do Visual Experience Composer](assets/personalization-use-case-3/use-case-diagram-3.png)

**Para implementar esse cenário, é necessário:**

* [Integrar AEM e Adobe Target usando o Launch e o Adobe I/O](./implementation.md#integrating-aem-target-options)

***Após implementar a integração acima, vamos explorar o  [cenário detalhadamente.](./personalization-use-case-3.md)***

## Personalização de experiências completas de página da Web

Integrar o Adobe Experience Manager à Adobe Target ajuda você a fornecer uma experiência personalizada aos usuários do seu site. Além disso, também ajuda você a entender melhor quais versões do conteúdo de seu site melhoram melhor suas conversões durante um período de teste especificado. Por exemplo, um teste A/B compara duas ou mais versões do conteúdo do site para ver qual favorece mais suas conversões, vendas ou outras métricas que você identifica. Um profissional de marketing pode criar atividades no Adobe Target para entender como os usuários interagem com o conteúdo do site e como isso afeta as métricas do site.

* O conteúdo reside em AEM e os editores de conteúdo criam e gerenciam as páginas do site
* O público alvo usa AEM páginas do site hospedado para executar testes e personalização
* O público alvo oferece conteúdo personalizado
* Nenhum novo conteúdo líquido é criado aqui
* Aplica-se a sites AEM e não AEM

   ![diagrama](assets/personalization-use-case-2/use-case-2-diagram.png)

**Para implementar esse cenário, é necessário:**

* [Integrar AEM e Adobe Target usando o Launch e o Adobe I/O](./implementation.md#integrating-aem-target-options)

***Após implementar a integração acima, vamos explorar o  [cenário detalhadamente.](./personalization-use-case-2.md)***
