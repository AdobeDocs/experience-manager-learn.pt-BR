---
title: Configurar regras de tradução no AEM
description: A interface de configuração de tradução permite que um usuário gerencie regras para traduzir conteúdo no AEM Sites. Este vídeo detalha a criação de uma nova regra de tradução para um componente personalizado.
feature: Language Copy
version: 6.4, 6.5
topic: Localization
role: User
level: Beginner
doc-type: Technical Video
exl-id: 359da531-839c-4680-abf9-c880cc700159
duration: 542
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Configurar regras de tradução {#set-up-translation-rules-in-aem}

A interface de configuração de tradução permite que um usuário gerencie regras para traduzir conteúdo no AEM Sites. Este vídeo detalha a criação de uma nova regra de tradução para um componente personalizado.

>[!NOTE]
>
> O vídeo abaixo foi gravado no AEM 6.3. O AEM 6.4+ apresenta uma nova estrutura de repositório para armazenar o arquivo XML de regras de tradução. Ao usar a interface de configuração de tradução no AEM 6.4+, as regras são salvas no local `/conf/global/settings/translation/rules/translation_rules.xml`. Consulte [Identificação do conteúdo a ser traduzido](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html) para obter mais detalhes.

>[!VIDEO](https://video.tv.adobe.com/v/18135?quality=12&learn=on)

As regras de tradução identificam o conteúdo no AEM que será extraído para tradução. As regras de tradução prontas para uso abrangem casos de uso comuns, como componentes de Texto e texto alternativo para componentes de Imagem. Dependendo dos requisitos de tradução de um projeto, podem ser necessárias regras adicionais. Em geral, as regras de tradução permitem que os usuários especifiquem:

1. Propriedades que devem ser traduzidas com base no caminho e/ou tipo de recurso
2. Filtros para propriedades que NÃO devem ser traduzidas
3. Conteúdo referenciado que deve ser traduzido (ou seja, imagens ou fragmentos de conteúdo)

O editor de regras de tradução que atualizará o arquivo xml de tradução. A interface de configuração de tradução facilita o gerenciamento de várias regras de tradução e protege contra erros de digitação ao editar diretamente o XML.

Acesse a interface de configuração de tradução:

* **[!UICONTROL Menu Iniciar AEM] > [!UICONTROL Ferramentas] > [!UICONTROL Geral] > [[!UICONTROL Configuração de tradução]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## Antes do AEM 6.3 {#prior-to-aem}

Em versões anteriores do AEM, as regras de tradução eram atualizadas manualmente ao editar um arquivo XML localizado no workflow de tradução: `/etc/workflow/models/translation/translation_rules.xml`.

## Recursos adicionais {#additional-resources}

* [Identificação do conteúdo a ser traduzido](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [Tradução de conteúdo para sites multilíngues](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Práticas recomendadas de tradução](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
