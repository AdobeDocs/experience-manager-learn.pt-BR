---
title: Introdução ao AEM e ao Adobe Target
description: Um tutorial completo que mostra como oferecer experiências personalizadas por meio do Adobe Experience Manager e do Adobe Target. Neste tutorial, você também aprenderá sobre as diferentes personas envolvidas no processo completo e como elas colaboram entre si
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b632883f-65fd-4f89-bf39-ec2bce352d2d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '846'
ht-degree: 100%

---

# Integrar o AEM Sites e o Adobe Target {#getting-started-with-aem-target}

O AEM e o Target são soluções potentes com recursos aparentemente sobrepostos. Às vezes, os clientes têm dificuldade de saber como e quando usar esses produtos em conjunto para fornecer uma experiência personalizada. Para fornecer uma experiência otimizada para cada usuário final, equipes diferentes da sua organização devem trabalhar em conjunto e definir quem faz o quê.

Neste tutorial, abordamos três casos diferentes para o AEM e o Target, a fim de explicar o que funciona melhor para a sua organização e como as diferentes equipes colaboram.

* Caso 1: personalização com fragmentos de experiência do AEM
* Caso 2: personalização com o Visual Experience Composer
* Caso 3: personalização de experiências completas de página da web

## Personalização com fragmentos de experiência do AEM {#personalization-using-aem-experience-fragment}

Neste caso, vamos usar o AEM e o Target. Claramente, ambos os produtos têm suas próprias vantagens, e, quando se trata de fornecer experiências personalizadas aos usuários do seu site, você precisa de um **conteúdo personalizado (conteúdo do AEM)** e uma **maneira inteligente (Target)** de fornecer esse conteúdo com base em um usuário específico.

O AEM ajuda a criar o conteúdo personalizado, reunindo todo o seu conteúdo e todos os seus ativos em um local central para impulsionar a sua estratégia de personalização. O AEM permite que você crie conteúdo facilmente para desktop, tablets e dispositivos móveis em um mesmo local sem escrever código. Não é necessário criar páginas para cada dispositivo. O AEM ajusta automaticamente cada experiência, usando o seu conteúdo. Você também pode exportar o conteúdo do AEM para o Adobe Target como ofertas por meio de um simples clique.

Agora, temos um conteúdo personalizado na forma de ofertas do AEM no Target. O Target permite entregar essas ofertas em grande escala com base em uma combinação de abordagens de aprendizado de máquina baseadas em regras e IA que incorporam variáveis comportamentais, contextuais e offline.  Com o Target, você pode configurar e executar facilmente atividades A/B e multivariadas (MVT) para determinar as melhores ofertas, experiências e conteúdo.

Os **fragmentos de experiência** representam um enorme passo à frente no sentido de vincular os criadores de conteúdo/experiência aos profissionais de personalização que impulsionam os resultados comerciais com o Target.

* Os criadores do editor de conteúdo do AEM personalizam o conteúdo como fragmentos de experiência e suas variações
* O AEM exporta o HTML do fragmento de experiência para o Target
* O Target usa a marcação do fragmento de experiência do AEM como ofertas nas atividades
* O Target fornece o fragmento de experiência do HTML, o AEM fornece imagens referenciadas

  ![Personalização com o diagrama de fragmentos de experiência](assets/personalization-use-case-1/use-case-1-diagram.png)

**Para implementar este caso, você precisa:**

* [Integrar o AEM e o Adobe Target com tags e o Adobe I/O](./implementation.md#integrating-aem-target-options)
* [AEM e Adobe Target com serviços em nuvem herdados](./implementation.md#integrating-aem-target-options)

***Depois de implementar as integrações acima, vamos explorar o [caso em mais detalhes](./personalization-use-case-1.md).***

## Personalização com o Visual Experience Composer

Os profissionais de marketing podem fazer alterações rápidas em seu site sem alterar o código para executar um teste com o Visual Experience Composer (VEC) do Adobe Target. O VEC é a interface do usuário estilo WYSIWYG (“o que você vê é o que você leva”) que permite criar e testar experiências e ofertas personalizadas facilmente no contexto do site. Você pode criar experiências e ofertas para atividades do Target, arrastando e soltando, trocando e modificando o layout e o conteúdo de uma página da web (ou oferta) ou página da web para dispositivos móveis.

O VEC é um dos principais recursos do Adobe Target. O VEC permite que os profissionais de marketing e designers criem e alterem o conteúdo por meio de uma interface visual. Muitas opções de design podem ser realizadas sem a necessidade de editar o código diretamente. A edição de HTML e JavaScript também é possível, usando-se as opções de edição disponíveis no compositor.

* O conteúdo reside no AEM, e os editores de conteúdo criam e gerenciam as páginas do site
* O Target usa as páginas do site hospedadas pelo AEM para executar testes e personalização
* O Target fornece conteúdo personalizado
* O novo conteúdo é criado com o VEC do Adobe Target
* Aplica-se a sites hospedados, ou não, pelo AEM

  ![Personalização com o diagrama do Visual Experience Composer](assets/personalization-use-case-3/use-case-diagram-3.png)

**Para implementar este caso, você precisa:**

* [Integrar o AEM e o Adobe Target com tags e o Adobe I/O](./implementation.md#integrating-aem-target-options)

***Depois de implementar a integração acima, vamos explorar o [caso em mais detalhes.](./personalization-use-case-3.md)***

## Personalização de experiências completas de páginas da web

A integração do Adobe Experience Manager com o Adobe Target ajuda a fornecer uma experiência personalizada aos usuários do site. Além disso, também ajuda a entender melhor quais versões do conteúdo do seu site melhoram as conversões durante um período de teste especificado. Por exemplo, um teste A/B compara duas ou mais versões do conteúdo do site para ver qual aumenta mais as conversões, vendas ou outras métricas identificadas. Um profissional de marketing pode criar atividades no Adobe Target para entender como os usuários interagem com o conteúdo do site e como ele afeta as métricas do site.

* O conteúdo reside no AEM, e os editores de conteúdo criam e gerenciam as páginas do site
* O Target usa as páginas do site hospedadas pelo AEM para executar testes e personalização
* O Target fornece conteúdo personalizado
* Nenhum conteúdo novo é criado aqui
* Aplica-se a sites do AEM ou não

  ![diagrama](assets/personalization-use-case-2/use-case-2-diagram.png)

**Para implementar este caso, você precisa:**

* [Integrar o AEM e o Adobe Target com tags e o Adobe I/O](./implementation.md#integrating-aem-target-options)

***Depois de implementar a integração acima, vamos explorar o [caso em mais detalhes.](./personalization-use-case-2.md)***
