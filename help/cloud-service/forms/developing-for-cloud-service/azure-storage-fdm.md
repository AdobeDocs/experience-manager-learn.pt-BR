---
title: Envio da configuração dos serviços em nuvem e do modelo de dados de formulário para a instância da nuvem
description: Crie e envie por push um formulário adaptável com base no modelo de dados de formulário de armazenamento do Azure para a instância da nuvem.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-9006
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
duration: 45
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%

---

# Incluir a configuração dos serviços em nuvem no projeto

Crie um contêiner de configuração chamado &quot;FormTutorial&quot; para manter a configuração dos serviços em nuvem
Crie uma configuração de serviços de nuvem para o Armazenamento do Azure chamada &quot;FormsCSAndAzureBlob&quot; no contêiner &quot;FormTutorial&quot; fornecendo os detalhes da conta de armazenamento do Azure e a chave de acesso do Azure.

Abra o projeto do AEM no IntelliJ. Adicione a pasta FormTutorial como mostrado abaixo no projeto ui.content
![configuração-de-serviços-em-nuvem](assets/cloud-services-configuration.png)

Certifique-se de adicionar a seguinte entrada no filter.xml do projeto ui.content

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![xml-filtro](assets/ui-content-filter.png)

## Incluir modelo de dados de formulário no projeto

Crie o modelo de dados de formulário com base na configuração dos serviços em nuvem que você criou na etapa anterior. Para incluir o modelo de dados de formulário no projeto, crie a estrutura de pastas apropriada no projeto do AEM no IntelliJ. Por exemplo, meu modelo de dados de formulário está em uma pasta chamada registros
![fdm-content](assets/ui-content-fdm.png)

Inclua a entrada apropriada no filter.xml do projeto ui.content

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>Agora, ao criar e implantar seu projeto usando o Cloud Manager, será necessário inserir novamente a chave de acesso do Azure na configuração dos serviços em nuvem. Para evitar inserir novamente a chave de acesso, é recomendável criar uma configuração sensível ao contexto usando as variáveis de ambiente conforme explicado no [próximo artigo](./context-aware-fdm.md)

## Próximas etapas

[Criar configuração sensível ao contexto](./context-aware-fdm.md)
