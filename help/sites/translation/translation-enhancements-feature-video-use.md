---
title: Aprimoramentos de tradução no AEM
description: AEM estrutura de tradução robusta permite que o conteúdo AEM seja traduzido de maneira simples por fornecedores de tradução suportados. Saiba mais sobre as melhorias mais recentes.
version: 6.4, 6.5
topic: Localization
feature: Multi Site Manager, Language Copy
role: User
level: Beginner
exl-id: 21633308-ffe4-4023-affe-59269504da69
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 5%

---

# Aprimoramentos de tradução com o gerenciamento de vários sites {#translation-enhancements}

AEM estrutura de tradução robusta permite que o conteúdo AEM seja traduzido de maneira simples por fornecedores de tradução suportados.

## Aprimoramentos de tradução no AEM 6.5

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=12&learn=on)

AEM 6.5 as melhorias na tradução incluem:

**Aprovação automática de trabalhos de tradução**: O sinalizador de aprovação no trabalho de tradução é uma propriedade binária. Ele não orienta nem integra com fluxos de trabalho de revisão e aprovação prontos para uso. Para manter o número de etapas em um trabalho de tradução mínimo, ele é definido como &quot;aprovar automaticamente&quot; em [!UICONTROL Propriedades avançadas] de um projeto de tradução. Se sua organização exigir aprovação para um trabalho de tradução, você poderá desmarcar a opção &quot;aprovar automaticamente&quot; em [!UICONTROL Propriedades avançadas] de um projeto de tradução.

**Excluir automaticamente inicializações de tradução**: Em vez de excluir manualmente as inicializações de tradução no Admin de inicializações depois do fato, agora é possível excluir automaticamente as inicializações de tradução depois de promovê-las.

**Exportar objetos de tradução no formato JSON**: AEM 6.4 e versões anteriores oferecem suporte aos formatos XML e XLIFF de objetos de tradução. Agora você pode configurar o formato de exportação para o formato JSON usando o console de sistemas [!UICONTROL Gerenciador de configurações]. Procure por [!UICONTROL Configuração da plataforma de tradução]e, em seguida, você pode selecionar o formato de exportação como JSON.

**Atualize o conteúdo AEM traduzido na Memória de Tradução (TMS)**: autor local que não tem acesso ao AEM pode fazer atualizações no conteúdo traduzido, que já foi assimilado de volta ao AEM, diretamente no TM (Memória de tradução, no TMS), e atualizar as traduções no AEM reenviando o trabalho de tradução do TMS para o AEM

## Aprimoramentos de tradução no AEM 6.4

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=12&learn=on)

Agora, os autores podem criar de forma rápida e fácil projetos de tradução em vários idiomas diretamente do administrador do Sites ou do administrador de Projetos, configurar esses projetos para promover inicializações automaticamente e até mesmo definir agendamentos para automação.

## Recursos adicionais {#additional-resources}

* [Tradução de conteúdo para sites multilíngues](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Práticas recomendadas de tradução](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
