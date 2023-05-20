---
title: Depuração de uma implementação de tags
description: Uma introdução a algumas ferramentas e técnicas comuns para depurar uma implementação de tags. Saiba como usar o console do desenvolvedor do navegador e a extensão do Depurador de Experience Platform para identificar e solucionar problemas de aspectos principais de uma implementação de tags.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Depuração de uma implementação de tags {#debug-tags-implementation}

Uma introdução a ferramentas e técnicas comuns usadas para depurar uma implementação de tags. Saiba como usar o console do desenvolvedor do navegador e a extensão do Depurador de Experience Platform para identificar e solucionar problemas de aspectos principais de uma implementação de tags.

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Depuração do lado do cliente por meio do objeto Satellite

A depuração do lado do cliente é útil para verificar o carregamento da regra de propriedade da tag ou a ordem de execução. Sempre que uma propriedade de tag for adicionada ao site, a variável `_satellite` O objeto JavaScript está presente no navegador para facilitar o evento do lado do cliente e o rastreamento de dados.

Para ativar a depuração no lado do cliente, chame o `setDebug(true)` no `_satellite` objeto.

1. Abra o console do navegador e execute o comando abaixo.

   ```javascript
       _satellite.setDebug(true);
   ```

1. Recarregue a página do site AEM e verifique se o log do console é exibido _regra acionada_ como abaixo.

   ![Marcar propriedade nas páginas Autor e Publicar](assets/satellite-object-debugging.png)

## Depuração via Adobe Experience Platform Debugger

O Adobe fornece o Adobe Experience Platform Debugger [Extensão do Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) e [Complemento do Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) para depurar, entender e obter insights sobre a integração.

1. Abra a extensão Adobe Experience Platform Debugger e abra a página do site na instância de Publicação

1. No **Adobe Experience Platform Debugger > Resumo > Tags do Adobe Experience Platform** verifique os detalhes da propriedade da tag, como Nome, Versão, Data de build, Ambiente e Extensões.

   ![Detalhes do depurador da Adobe Experience Platform e da propriedade da tag](assets/tag-property-details.png)

## Recursos adicionais {#additional-resources}

+ [Introdução ao Adobe Experience Platform Debugger](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Referência a objeto satélite](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
