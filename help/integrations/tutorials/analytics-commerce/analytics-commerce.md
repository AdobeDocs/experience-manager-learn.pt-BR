---
title: Tutorial Integrar o Analytics ao Commerce
description: Saiba como integrar o Analytics ao Commerce.
solution: Analytics, Commerce
feature: Integrations
topic: Integrations
role: Leader, Architect, Admin, Developer
level: Beginner
index: true
hidefromtoc: true
kt: null
thumbnail: null
last-substantial-update: 2023-04-11T00:00:00Z
badgeIntegration: label="Integração" type="positive"
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---


# Integrar o Analytics ao Commerce

* Verifique se a conta tem acesso ao Adobe Analytics.

* Crie um projeto no Adobe Analytics.

* Crie um esquema.
   * Você precisa disso para selecionar entre as opções nas etapas posteriores. Para criar um esquema, observe na coluna à esquerda em &quot;Gerenciamento de dados&quot; e localize Esquemas. Agora, na parte superior esquerda, clique em &quot;Criar esquema&quot;. Selecione XDM ExperienceEvent.
   * À esquerda, encontre Grupos de campos e clique em Adicionar
      * Na pesquisa, você pode filtrar inserindo `ExperienceEvent Commerce`
      * Procure `Adobe Analytics ExperienceEvent Commerce` e marque a caixa
      * Certifique-se de clicar no link `Add field groups` na parte superior direita para salvar e continuar
* Crie um conjunto de dados, você precisa disso ao configurar o &quot;DataStream&quot; em seguida.
   * O conjunto de dados é encontrado na coluna à esquerda &quot;Gerenciamento de dados&quot; e procurando por &quot;Conjuntos de dados&quot;.
   * Em seguida, clique em &quot;Criar conjunto de dados&quot;, localizado na parte superior direita. Crie o conjunto de dados a partir do esquema.
   * pesquisar e usar o esquema criado anteriormente
* Criar sequência de dados. Você pode acessá-la usando a &quot;Coleção de dados na coluna à esquerda&quot; e procurando por &quot;Fluxos de dados&quot;.
* Crie tabelas com painéis e segmentos. Essa é a maneira de ser complicado para este tutorial, você precisa de uma pessoa experiente do Analytics para ajudar.


Por fim, para exibir seu relatório, navegue até experience.adobe.com encontrar o projeto do seu espaço de trabalho, clique no link do projeto que deseja exibir e você deverá ver algo como essa imagem

![Captura de tela do Analytics de alguns dados de comércio](./assets/analytics-commerce/analytics-screenshot-commerce-items.png)