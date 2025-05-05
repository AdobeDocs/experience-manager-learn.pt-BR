---
title: Depuração de uma implementação de tags
description: Uma introdução a algumas ferramentas e técnicas comuns para depurar uma implementação de tags. Saiba como usar o console do desenvolvedor do navegador e a extensão do Depurador de Experience Platform para identificar e solucionar problemas de aspectos principais de uma implementação de tags.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 259
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 1%

---

# Depuração de uma implementação de tags {#debug-tags-implementation}

Uma introdução a ferramentas e técnicas comuns usadas para depurar uma implementação de tags. Saiba como usar o console do desenvolvedor do navegador e a extensão do Depurador de Experience Platform para identificar e solucionar problemas de aspectos principais de uma implementação de tags.

>[!VIDEO](https://video.tv.adobe.com/v/327408?quality=12&learn=on&captions=por_br)

## Depuração do lado do cliente por meio do objeto Satellite

A depuração do lado do cliente é útil para verificar o carregamento da regra de propriedade da tag ou a ordem de execução. Sempre que uma propriedade Tag é adicionada ao site, o objeto JavaScript `_satellite` está presente no navegador para facilitar o rastreamento de eventos e dados no lado do cliente.

Para habilitar a depuração no lado do cliente, chame o método `setDebug(true)` no objeto `_satellite`.

1. Abra o console do navegador e execute o comando abaixo.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Recarregue a página do site AEM e verifique se o log do console mostra a mensagem _regra acionada_ como abaixo.

   ![Marcar propriedade nas páginas Autor e Publish](assets/satellite-object-debugging.png)

## Depuração via Adobe Experience Platform Debugger

O Adobe fornece o Adobe Experience Platform Debugger [Chrome extension](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) para depurar, entender e obter informações sobre a integração.

1. Abra a extensão Adobe Experience Platform Debugger e abra a página do site na instância do Publish

2. Na seção **Adobe Experience Platform Debugger > Resumo > Tags do Adobe Experience Platform**, verifique os detalhes da propriedade da Marca, como Nome, Versão, Data de Compilação, Ambiente e Extensões.

   ![Detalhes de Adobe Experience Platform Debugger e Propriedades de Marca](assets/tag-property-details.png)

## Recursos adicionais {#additional-resources}

+ [Introdução ao Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=pt-BR)

+ [Referência a objeto satélite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html?lang=pt-BR)
