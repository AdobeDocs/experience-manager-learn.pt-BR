---
title: Aprimoramentos de tradução no AEM
description: Uma estrutura de tradução robusta para AEM permite que o conteúdo do AEM seja traduzido perfeitamente por fornecedores de tradução compatíveis. Saiba mais sobre os últimos aprimoramentos.
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

# Aprimoramentos de tradução com o Gerenciador de vários sites {#translation-enhancements}

Uma estrutura de tradução robusta para AEM permite que o conteúdo do AEM seja traduzido perfeitamente por fornecedores de tradução compatíveis.

## Melhorias na tradução no AEM 6.5

>[!VIDEO](https://video.tv.adobe.com/v/27405?quality=12&learn=on)

As melhorias na tradução do AEM 6.5 incluem:

**Aprovar trabalhos de tradução automaticamente**: o sinalizador de aprovação no trabalho de tradução é uma propriedade binária. Ele não direciona nem se integra a fluxos de trabalho de revisão e aprovação prontos para uso. Para manter o número de etapas em um trabalho de tradução mínimo, por padrão, ele é definido para &quot;aprovar automaticamente&quot; no [!UICONTROL Propriedades avançadas] de um projeto de tradução. Se sua organização exigir aprovação para um trabalho de tradução, você poderá desmarcar a opção &quot;aprovar automaticamente&quot; no [!UICONTROL Propriedades avançadas] de um projeto de tradução.

**Excluir inicializações de tradução automaticamente**: em vez de excluir manualmente inicializações de tradução no Administrador de inicializações após o fato, agora é possível excluir automaticamente inicializações de tradução depois que elas foram promovidas.

**Exportar objetos de Tradução no formato JSON**: o AEM 6.4 e as versões anteriores são compatíveis com formatos XML e XLIFF de objetos de tradução. Agora você pode configurar o formato de exportação para o formato JSON usando o console de sistemas [!UICONTROL Gerenciador de configurações]. Procure [!UICONTROL Configuração da plataforma de tradução]e, em seguida, você poderá selecionar o formato de exportação como JSON.

**Atualizar conteúdo de AEM traduzido na Memória de tradução (TMS)**: o autor local que não tem acesso ao AEM pode fazer atualizações no conteúdo traduzido, que já foi assimilado de volta no AEM, diretamente no TM (Translation Memory, em TMS), e atualizar as traduções no AEM reenviando o trabalho de tradução do TMS para o AEM

## Melhorias na tradução no AEM 6.4

>[!VIDEO](https://video.tv.adobe.com/v/21309?quality=12&learn=on)

Agora, os autores podem criar rápida e facilmente projetos de tradução em vários idiomas diretamente do administrador do Sites ou do administrador do Projects, configurar esses projetos para promover inicializações automaticamente e até definir agendamentos para automação.

## Recursos adicionais {#additional-resources}

* [Tradução de conteúdo para sites multilíngues](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [Práticas recomendadas de tradução](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
