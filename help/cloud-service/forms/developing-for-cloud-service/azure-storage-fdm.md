---
title: Envio da configuração dos serviços em nuvem e do modelo de dados de formulário para a instância da nuvem
description: Crie e envie um Formulário adaptável com base no modelo de dados do formulário de armazenamento do Azure para a instância da nuvem.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# Incluir a configuração dos serviços em nuvem no seu projeto

Crie um contêiner de configuração chamado &quot;Tutorial do Forms&quot; para manter a configuração dos serviços de nuvem Crie uma configuração de serviços de nuvem para o Armazenamento do Azure chamada &quot;Armazenar envios de formulário no Azure&quot; no contêiner &quot;Tutorial do Forms&quot;. Forneça os detalhes da conta de armazenamento do Azure e a chave da conta

Abra seu projeto de AEM no IntelliJ. Certifique-se de adicionar a pasta FormTutorial conforme mostrado abaixo no projeto ui.content
![configuração de serviços em nuvem](assets/cloud-services-configuration.png)

Certifique-se de adicionar a seguinte entrada no filter.xml do projeto ui.content

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## Incluir o modelo de dados de formulário no projeto

Crie um modelo de dados de formulário com base na configuração dos serviços em nuvem que você criou na etapa anterior. Para incluir o modelo de dados de formulário em seu projeto, crie a estrutura de pastas apropriada em seu projeto de AEM no intelliJ. Por exemplo, meu modelo de dados de formulário está em uma pasta chamada registros
![fdm-content](assets/ui-content-fdm.png)

Inclua a entrada apropriada no filter.xml do projeto ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Agora, ao criar e implantar seu projeto, o projeto terá o modelo de dados de formulário com base na configuração de serviços em nuvem disponível na instância da nuvem
