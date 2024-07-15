---
title: Imagem inteligente
description: A Criação de imagens inteligentes no Dynamic Media Classic melhora o desempenho da entrega de imagens, otimizando automaticamente o formato e a qualidade da imagem com base nos recursos do navegador do cliente. Ele faz isso aproveitando os recursos de IA do Adobe Sensei e trabalhando com Predefinições de imagem existentes. Saiba mais sobre a Criação de imagens inteligentes e como usá-la para oferecer melhores experiências aos clientes por meio de carregamentos de página mais rápidos.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 678671c3-af25-4da1-bc14-cbc4cc19be8d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 1%

---

# Imagem inteligente {#smart-imaging}

Um dos aspectos mais importantes da experiência do cliente em seu site ou site ou aplicativo móvel é o tempo de carregamento da página. Os clientes geralmente abandonam um site ou aplicativo se uma página demorar muito para ser carregada. As imagens constituem a maioria do tempo de carregamento da página. A Criação de imagens inteligentes no Dynamic Media Classic melhora o desempenho da entrega de imagens, otimizando automaticamente o formato e a qualidade da imagem com base nos recursos do navegador do cliente. Ele faz isso aproveitando os recursos de IA do Adobe Sensei e trabalhando com Predefinições de imagem existentes. A Smart Imaging reduz os tamanhos de imagem em 30% ou mais, o que significa carregamentos de página mais rápidos e melhores experiências para o cliente.

A Smart Imaging também se beneficia do aumento de desempenho de estar totalmente integrado ao melhor serviço premium da classe do Adobe. Este serviço encontra a rota de Internet ideal entre servidores, redes e pontos de troca de tráfego (peering) que tem a menor latência e/ou taxa de perda de pacotes do que a rota padrão na Internet.

Saiba mais sobre [Smart Imaging](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=pt-BR).

## Benefícios do Smart Imaging

Como as imagens constituem a maioria do tempo de carregamento de uma página, a melhoria de desempenho do Smart Imaging pode ter um profundo impacto nos KPIs de negócios, como maior conversão, tempo gasto no site e menor taxa de rejeição do site.

![imagem](assets/smart-imaging/smart-imaging-1.png)

## Como funciona a imagem inteligente

Como mencionado anteriormente, o Smart Imaging aproveita os recursos da IA do Adobe Sensei e funciona com as Predefinições de imagem existentes para converter automaticamente as imagens para formatos de imagem ideais de próxima geração, como WebP, mantendo a fidelidade visual.

Saiba mais sobre [Como o Smart Imaging funciona](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html#how-does-smart-imaging-work), incluindo detalhes como formatos de imagem compatíveis (e o que acontece se você não usar esses formatos) e seu impacto nas Predefinições de Imagem existentes que estão em uso.

## Impactos da imagem inteligente

Você provavelmente está preocupado com a necessidade de fazer alterações nos URLs, nas Predefinições de imagem e no código do site para aproveitar as vantagens do Smart Imaging. Se você atender aos pré-requisitos para usar a Imagem inteligente e só estiver trabalhando com imagens nos formatos de imagem JPEG e PNG compatíveis, não será necessário fazer alterações.

As imagens inteligentes funcionam com imagens entregues por HTTP, HTTPS e HTTP/2.

>[!NOTE]
>
>A migração para Imagem inteligente limpa seu cache na CDN. Normalmente, o cache na CDN é acumulado novamente em um ou dois dias.

A imagem inteligente está incluída em sua licença atual do Dynamic Media Classic. Não há custos adicionais para esse recurso. Para aproveitar isso, você deve atender a dois requisitos: ter um CDN agrupado em Adobe e um domínio dedicado. Em seguida, você deve habilitá-lo para sua conta, pois ele não é habilitado automaticamente.

A habilitação de Smart Imaging começa com o envio de uma solicitação ao suporte técnico por |criando um caso de suporte| [https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html](https://helpx.adobe.com/br/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html). O suporte trabalhará com você para configurar um domínio personalizado que será associado à Imagem inteligente. Você alterará um parâmetro relacionado ao armazenamento em cache (Time To Live, ou TTL) e o suporte limpará o cache. Se desejar, também é possível executar uma etapa opcional de preparo antes de enviar para a produção. Quando a opção Imagem inteligente estiver ativada, você fornecerá aos clientes imagens menores, mas com a mesma qualidade solicitada. Isso significa que eles experimentam tempos de carregamento de página mais rápidos — e tudo isso é feito automaticamente porque o Adobe Sensei ajuda a escolher o tamanho mais eficiente.

Depois de habilitar o Smart Imaging, você verificará se ele está funcionando como esperado.

Você provavelmente tem outras dúvidas sobre o Smart Imaging. Compilamos uma lista de perguntas frequentes com respostas. Leia as [Perguntas frequentes](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/imaging-faq.html?lang=pt-BR).

## Recursos adicionais

Assista ao [webinário sob demanda do Dynamic Media Classic Otimizing Page Performance Skill Builder](https://seminars.adobeconnect.com/pzc1gw0cihpv) para saber mais sobre Imagem inteligente.
