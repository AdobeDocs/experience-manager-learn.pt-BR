---
title: Configurar regras de tradução em AEM
description: A interface do usuário de configuração de tradução permite que um usuário gerencie regras para tradução de conteúdo no AEM Sites. Este vídeo detalha a criação de uma nova regra de tradução para um componente personalizado.
feature: language-copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '321'
ht-degree: 0%

---


# Configurar regras de tradução {#set-up-translation-rules-in-aem}

A interface do usuário de configuração de tradução permite que um usuário gerencie regras para tradução de conteúdo no AEM Sites. Este vídeo detalha a criação de uma nova regra de tradução para um componente personalizado.

>[!NOTE]
>
> O vídeo abaixo foi gravado no AEM 6.3. AEM 6.4+ apresenta uma nova estrutura de repositório para armazenar o arquivo XML das regras de conversão. Ao usar a interface de usuário de configuração de tradução no AEM 6.4+, as regras são salvas no local `/conf/global/settings/translation/rules/translation_rules.xml`. Consulte [Identificar conteúdo para traduzir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) para obter mais detalhes.

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

As regras de tradução identificam o conteúdo no AEM a ser extraído para tradução. As regras de tradução imediata abrangem casos de uso comuns, como componentes de Texto e texto alternativo para componentes de Imagem. Dependendo de um projeto, podem ser necessárias regras adicionais de tradução. Em geral, as regras de tradução permitem que os usuários especifiquem:

1. Propriedades que devem ser traduzidas com base no caminho e/ou tipo de recurso
2. Filtros para propriedades que NÃO devem ser traduzidas
3. Conteúdo referenciado que deve ser traduzido (ou seja, imagens ou fragmentos de conteúdo)

O editor de regras de conversão que atualizará o arquivo xml de conversão. A interface do usuário de configuração de tradução facilita o gerenciamento de várias regras de tradução e protege contra erros de digitação ao editar o XML diretamente.

Acesse a interface de usuário de configuração de tradução:

* **[!UICONTROL Menu]  Start AEM>  [!UICONTROL Ferramentas] >  [!UICONTROL Geral] > Configuração  [[!UICONTROL de tradução]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Antes do AEM 6.3 {#prior-to-aem}

Em versões anteriores, as regras de conversão eram atualizadas manualmente ao editar um arquivo XML localizado no fluxo de trabalho de Tradução: `/etc/workflow/models/translation/translation_rules.xml`.

## Recursos adicionais {#additional-resources}

* [Identificação do conteúdo a ser traduzido](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Traduzindo conteúdo para sites multilíngues](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Práticas recomendadas de tradução](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
