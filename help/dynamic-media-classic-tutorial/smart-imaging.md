---
title: Imagem inteligente
description: O Smart Imaging no Dynamic Media Classic melhora o desempenho do delivery de imagem, otimizando automaticamente o formato e a qualidade de imagem com base nos recursos do navegador do cliente. Ele faz isso aproveitando os recursos da Adobe Sensei AI e trabalhando com as Predefinições de imagem existentes. Saiba mais sobre a Smart Imaging e como você pode usá-la para oferecer melhores experiências ao cliente por meio de carregamentos de página mais rápidos.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
topic: Gerenciamento de conteúdo
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 3%

---


# Imagem inteligente {#smart-imaging}

Um dos aspectos mais importantes da experiência do cliente em seu site ou site ou aplicativo móvel é o tempo de carregamento da página. Os clientes geralmente abandonam um site ou aplicativo se uma página demorar muito para ser carregada. As imagens constituem a maior parte do tempo de carregamento da página. O Smart Imaging no Dynamic Media Classic melhora o desempenho do delivery de imagem, otimizando automaticamente o formato e a qualidade de imagem com base nos recursos do navegador do cliente. Ele faz isso aproveitando os recursos da Adobe Sensei AI e trabalhando com as Predefinições de imagem existentes. A Smart Imaging reduz os tamanhos das imagens em 30% ou mais — o que significa carregamentos de página mais rápidos e experiências de cliente melhores.

O Smart Imaging também se beneficia do aumento de desempenho de ser totalmente integrado ao melhor serviço premium do Adobe. Esse serviço encontra a rota de Internet ideal entre servidores, redes e pontos de peering que tem a latência mais baixa e/ou a taxa de perda de pacotes que a rota padrão na Internet.

Saiba mais sobre [Imagem inteligente](https://docs.adobe.com/content/help/pt-BR/experience-manager-64/assets/dynamic/imaging-faq.html).

## Benefícios da imagem inteligente

Como as imagens constituem a maioria do tempo de carregamento de uma página, a melhoria de desempenho da Smart Imaging pode ter um impacto profundo nos KPIs comerciais, como conversão mais alta, tempo gasto no site e taxa de rejeição mais baixa do site.

![imagem](assets/smart-imaging/smart-imaging-1.png)

## Como a criação de imagens inteligentes funciona

Como mencionado anteriormente, o Smart Imaging utiliza os recursos do Adobe Sensei AI e funciona com as Predefinições de imagem existentes para converter automaticamente as imagens em formatos de imagem ideais de próxima geração, como o WebP, mantendo a fidelidade visual.

Saiba mais sobre [Como a Imagem inteligente funciona](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), incluindo detalhes como formatos de imagem compatíveis (e o que acontece se você não usa esses formatos) e seu impacto nas Predefinições de imagem existentes que estão em uso.

## Impactos da Smart Imaging

Você provavelmente está preocupado que precisará fazer alterações nos URLs, nas predefinições de imagens e no código do site para aproveitar as vantagens do Smart Imaging. Se você atender aos pré-requisitos para usar a Imagem inteligente e estiver trabalhando somente com imagens nos formatos de imagem JPEG e PNG compatíveis, não será necessário fazer alterações.

A geração de imagens inteligentes funciona com imagens entregues por HTTP, HTTPS e HTTP/2.

>[!NOTE]
>
>Passar para o Smart Imaging limpa o cache no CDN. O cache no CDN normalmente é construído novamente dentro de um ou dois dias.

O Smart Imaging está incluído em sua licença existente do Dynamic Media Classic. Não há custos adicionais para este recurso. Para aproveitar esse recurso, você deve atender a dois requisitos: têm um CDN fornecido em Adobe e um domínio dedicado. Em seguida, você deve habilitá-lo para sua conta, pois ele não é habilitado automaticamente.

A ativação da Smart Imaging começa com você enviando uma solicitação de suporte técnico ao |criação de um caso de suporte| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). O suporte funcionará com você para configurar um domínio personalizado que você associará ao Smart Imaging. Você alterará um parâmetro relacionado ao armazenamento em cache (Time To Live, ou TTL) e o suporte limpará o cache. Você também pode fazer uma etapa de preparo opcional, se desejar, antes de levar para a produção. Quando a Smart Imaging estiver ativada, você fornecerá aos clientes imagens de menor tamanho, mas com a mesma qualidade solicitada. Isso significa que eles experimentam tempos de carregamento de página mais rápidos — e tudo isso é feito automaticamente porque o Adobe Sensei ajuda a escolher o tamanho mais eficiente.

Depois de ativar o Smart Imaging, você deverá verificar se ele está funcionando como esperado.

Você provavelmente tem perguntas adicionais sobre Smart Imaging. Nós compilamos uma lista de perguntas frequentes com respostas. Leia as [Perguntas frequentes](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Recursos adicionais

Assista ao webinário sob demanda [Dynamic Media Classic Otimizing Page Performance Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) para saber mais sobre Smart Imaging.
