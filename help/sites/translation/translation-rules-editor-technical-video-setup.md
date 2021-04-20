---
title: Configurar regras de tradução no AEM
description: A interface do usuário Configuração de tradução permite que um usuário gerencie regras para a tradução de conteúdo no AEM Sites. Este vídeo detalha a criação de uma nova regra de tradução para um componente personalizado.
feature: Language Copy
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Localization
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 3%

---


# Configurar regras de tradução {#set-up-translation-rules-in-aem}

A interface do usuário Configuração de tradução permite que um usuário gerencie regras para a tradução de conteúdo no AEM Sites. Este vídeo detalha a criação de uma nova regra de tradução para um componente personalizado.

>[!NOTE]
>
> O vídeo abaixo foi gravado no AEM 6.3. O AEM 6.4+ introduz uma nova estrutura de repositório para armazenar o arquivo XML de regras de tradução. Ao usar a interface de configuração de tradução no AEM 6.4+, as regras são salvas no local `/conf/global/settings/translation/rules/translation_rules.xml`. Consulte [Identificação de conteúdo para traduzir](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) para obter mais detalhes.

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

As regras de tradução identificam o conteúdo no AEM que será extraído para tradução. As regras de tradução prontas para uso abrangem casos de uso comuns, como componentes de Texto e texto alternativo para componentes de Imagem . Dependendo de um projeto, podem ser necessárias regras adicionais de tradução. Em geral, as regras de tradução permitem que os usuários especifiquem:

1. Propriedades que devem ser traduzidas com base no caminho e/ou no tipo de recurso
2. Filtros para propriedades que NÃO devem ser traduzidas
3. Conteúdo referenciado que deve ser traduzido (ou seja, Imagens ou Fragmentos de conteúdo)

O editor de regras de tradução que atualizará o arquivo xml de tradução. A interface do usuário Configuração de tradução facilita o gerenciamento de várias regras de tradução e protege contra erros de digitação ao editar o XML diretamente.

Acesse a interface do usuário de configuração de tradução:

* **[!UICONTROL Menu]  inicial do AEM >  [!UICONTROL Ferramentas]  >  [!UICONTROL Geral]  > Configuração  [[!UICONTROL de tradução]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Antes do AEM 6.3 {#prior-to-aem}

Em versões anteriores do AEM, as regras de tradução eram atualizadas manualmente editando um arquivo XML localizado no fluxo de trabalho de Tradução: `/etc/workflow/models/translation/translation_rules.xml`.

## Recursos adicionais {#additional-resources}

* [Identificação de conteúdo a ser traduzido](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Tradução de conteúdo para sites multilíngues](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Práticas recomendadas de tradução](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
