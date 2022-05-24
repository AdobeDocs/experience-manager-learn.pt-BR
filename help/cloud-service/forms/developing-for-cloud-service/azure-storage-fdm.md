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
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 2ac0f6b3964590e5443700f730a3fc02cb3f63bc
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Incluir a configuração dos serviços em nuvem no seu projeto

Crie um contêiner de configuração chamado &quot;FormTutorial&quot; para manter a configuração dos serviços de nuvem Crie uma configuração de serviços de nuvem para o Armazenamento do Azure chamada &quot;FormsCSAndAzureBlob&quot; no contêiner &quot;FormTutorial&quot; fornecendo os detalhes da conta de armazenamento do Azure e a chave de acesso do Azure.

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
>Agora, ao criar e implantar seu projeto usando o cloud manager, você terá que inserir novamente sua chave de acesso do Azure na configuração dos serviços em nuvem. Para evitar inserir novamente a chave de acesso, é recomendável criar uma configuração sensível ao contexto usando as variáveis de ambiente, conforme explicado na seção [próximo artigo](./context-aware-fdm.md)
