---
title: Modificar Definições de Configuração do Data Source.
description: Modifique o nome do host e outras configurações nas Configurações do Data Source.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 6c63787c-e511-4764-9a03-2c85c394bcc0
last-substantial-update: 2019-06-09T00:00:00Z
duration: 28
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Capacidade de modificar as Configurações do Data Source{#ability-to-modify-data-source-configuration-settings}

Até a versão AEM Forms 6.4, depois que uma fonte de dados era configurada, não era possível alterar schema, host, caminho base para o serviço RESTful. Isso era problemático se você quisesse testar suas fontes de dados em ambientes diferentes.

Com o lançamento do AEM Forms 6.5, agora é possível alterar facilmente as propriedades mencionadas acima. Com esse novo recurso, agora é possível criar Modelos de dados de formulário em relação a ambientes de desenvolvimento e, uma vez satisfeito com os resultados, você poderá alterar as propriedades para apontar para um ambiente diferente.

As capturas de tela abaixo mostram as configurações da fonte de dados no AEM Forms 6.4 e no Forms 6.5

**Configuração do Data Source no AEM 6.4**

![64Configuração de DataSource](assets/64release.gif)
**Configuração de Source de Dados Editáveis no AEM 6.5 e posterior**
![65Configuração de DataSource](assets/modifiable_data_source.png)
