---
title: Aprimoramentos de tradução no AEM
description: AEM estrutura de tradução robusta permite que o conteúdo AEM seja traduzido de maneira simples por fornecedores de tradução suportados. Saiba mais sobre as melhorias mais recentes.
feature: gerenciador de vários sites, cópia de idioma
topics: localization, authoring, content-architecture
audience: author, marketer, developer
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
topic: Localização
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 2%

---


# Aprimoramentos de tradução com o gerenciamento de vários sites {#translation-enhancements}

AEM estrutura de tradução robusta permite que o conteúdo AEM seja traduzido de maneira simples por fornecedores de tradução suportados.

## Aprimoramentos de tradução no AEM 6.5

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=9&learn=on)

AEM 6.5 as melhorias na tradução incluem:

**Aprovar trabalhos** de tradução automaticamente: O sinalizador de aprovação no trabalho de tradução é uma propriedade binária. Ele não orienta nem integra com fluxos de trabalho de revisão e aprovação prontos para uso. Para manter o número mínimo de etapas em um trabalho de tradução, ele é definido por padrão como &quot;aprovar automaticamente&quot; em [!UICONTROL Propriedades avançadas] de um projeto de tradução. Se sua organização exigir aprovação para um trabalho de tradução, você poderá desmarcar a opção &quot;aprovar automaticamente&quot; em [!UICONTROL Propriedades avançadas] de um projeto de tradução.

**Excluir automaticamente inicializações** de tradução: Em vez de excluir manualmente as inicializações de tradução no Admin de inicializações depois do fato, agora é possível excluir automaticamente as inicializações de tradução depois de promovê-las.

**Exportar objetos de tradução no formato** JSON: AEM 6.4 e versões anteriores oferecem suporte aos formatos XML e XLIFF de objetos de tradução. Agora você pode configurar o formato de exportação para o formato JSON usando o console de sistemas [!UICONTROL Config Manager]. Procure por [!UICONTROL Translation Platform Configuration] e você pode selecionar o formato de exportação como JSON.

**Atualize o conteúdo AEM traduzido na Memória de Tradução (TMS)**: autor local que não tem acesso ao AEM pode fazer atualizações no conteúdo traduzido, que já foi assimilado de volta ao AEM, diretamente no TM (Memória de tradução, no TMS), e atualizar as traduções no AEM reenviando o trabalho de tradução do TMS para o AEM

## Aprimoramentos de tradução no AEM 6.4

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=9&learn=on)

Agora, os autores podem criar de forma rápida e fácil projetos de tradução em vários idiomas diretamente do administrador do Sites ou do administrador de Projetos, configurar esses projetos para promover inicializações automaticamente e até mesmo definir agendamentos para automação.

## Recursos adicionais {#additional-resources}

* [Tradução de conteúdo para sites multilíngues](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Práticas recomendadas de tradução](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)