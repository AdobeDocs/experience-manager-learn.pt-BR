---
title: Imagem inteligente
description: O Smart Imaging in Dynamic Media Classic melhora o desempenho do delivery de imagem ao otimizar automaticamente o formato e a qualidade da imagem com base nos recursos do navegador do cliente. Isso é feito aproveitando os recursos do Adobe Sensei AI e trabalhando com as predefinições de imagens existentes. Saiba mais sobre o Smart Imaging e como você pode usá-lo para oferta experiências melhores do cliente por meio de cargas de página mais rápidas.
sub-product: dynamic-media
feature: smart-crop
doc-type: tutorial
topics: development, authoring, configuring, renditions, images
audience: all
activity: use
translation-type: tm+mt
source-git-commit: 317fb625e7af57b7ad0079014c341eab9adda376
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 1%

---


# Imagem inteligente {#smart-imaging}

Um dos aspectos mais importantes da experiência do cliente em seu site ou site ou aplicativo móvel é o tempo de carregamento da página. Os clientes normalmente abandonarão um site ou aplicativo se uma página levar muito tempo para ser carregada. As imagens constituem a maioria do tempo de carregamento da página. O Smart Imaging in Dynamic Media Classic melhora o desempenho do delivery de imagem ao otimizar automaticamente o formato e a qualidade da imagem com base nos recursos do navegador do cliente. Isso é feito aproveitando os recursos do Adobe Sensei AI e trabalhando com as predefinições de imagens existentes. A imagem inteligente reduz os tamanhos de imagem em 30% ou mais. o que se traduz em cargas de página mais rápidas e melhores experiências do cliente.

O Smart Imaging também se beneficia do aumento de desempenho ao ser totalmente integrado ao melhor serviço premium do Adobe. Este serviço encontra a melhor rota da Internet entre servidores, redes e pontos de peering que tem a menor latência e/ou a menor taxa de perda de pacotes do que a rota padrão da Internet.

Saiba mais sobre [Imagens inteligentes](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Benefícios do Smart Imaging

Como as imagens constituem a maioria do tempo de carregamento de uma página, o aprimoramento do desempenho do Smart Imaging pode ter um profundo impacto nos KPIs comerciais, como conversão mais alta, tempo gasto no site e taxa de rejeição mais baixa do site.

![imagem](assets/smart-imaging/smart-imaging-1.png)

## Como funciona a criação de imagens inteligentes

Como observado anteriormente, o Smart Imaging aproveita os recursos do Adobe Sensei AI e funciona com as predefinições de imagens existentes para converter automaticamente as imagens em formatos ideais de imagem da próxima geração, como o WebP, mantendo a fidelidade visual.

Saiba mais sobre [Como a Imagem Inteligente funciona](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), incluindo detalhes como formatos de imagem suportados (e o que acontece se você não usar esses formatos) e seu impacto nas Predefinições de Imagem existentes que estão em uso.

## Impactos da criação de imagens inteligentes

Você provavelmente está preocupado com a necessidade de fazer alterações nos URLs, nas predefinições de imagens e no código do seu site para aproveitar as vantagens do Smart Imaging. Se você atender aos pré-requisitos para usar a Imagem inteligente e estiver trabalhando somente com imagens nos formatos de imagem JPEG e PNG suportados, não será necessário fazer alterações.

A geração de imagens inteligentes funciona com imagens entregues por HTTP, HTTPS e HTTP/2.

>[!NOTE]
>
>Passar para o Smart Imaging limpa seu cache no CDN. O cache no CDN é normalmente construído novamente dentro de um ou dois dias.

O Smart Imaging está incluído na sua licença existente do Dynamic Media Classic. Não há custos adicionais para este recurso. Para tirar proveito disso, você deve atender a dois requisitos: têm um CDN fornecido em Adobe e um domínio dedicado. Em seguida, você deve ativá-la para sua conta, pois ela não é ativada automaticamente.

Habilitar start de Imagem Inteligente com você enviando uma solicitação ao suporte técnico por meio da |criação de um caso de suporte| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). O suporte trabalhará com você para configurar um domínio personalizado que você associará ao Smart Imaging. Você alterará um parâmetro relacionado ao armazenamento em cache (Time To Live, ou TTL) e o suporte limpará o cache. Você também pode executar uma etapa de preparo opcional se desejar antes de encaminhar para a produção. Quando a opção Imagens inteligentes estiver ativada, você fornecerá aos clientes imagens de tamanho menor, mas com a mesma qualidade solicitada. Isso significa que eles experimentam tempos de carregamento de página mais rápidos. e tudo isso é feito automaticamente porque a Adobe Sensei ajuda a escolher o tamanho mais eficiente.

Depois de ativar a Imagem inteligente, verifique se ela está funcionando como esperado.

Você provavelmente tem perguntas adicionais sobre o Smart Imaging. Nós compilamos uma lista de perguntas frequentes com respostas. Leia as [Perguntas frequentes](https://docs.adobe.com/content/help/en/experience-manager-64/assets/dynamic/imaging-faq.html).

## Recursos adicionais

Assista ao webinar [Dynamic Media Classic Otimizing Page Performance Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) sob demanda para saber mais sobre o Smart Imaging.
