---
title: Habilitar Componentes do Portal AEM Forms
description: Criar um portal do AEM Forms usando componentes principais
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Core Components
jira: KT-10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
duration: 78
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Componentes do portal do Forms

A AEM Forms fornece os seguintes componentes do portal prontos para uso:

**Pesquisa e Lister**: esse componente permite listar formulários do repositório de formulários na página do portal e fornece opções de configuração para listar formulários com base em critérios especificados.

**Rascunhos e Envios**: enquanto o componente Pesquisa e Listagem exibe formulários que são tornados públicos pelo autor do Forms, o componente Rascunhos e Envios exibe formulários que são salvos como rascunho para conclusão posterior e formulários enviados. Este componente fornece experiência personalizada para qualquer usuário conectado.

**Link**: este componente permite criar um link para um formulário em qualquer lugar da página.

## Habilitar Componentes do Portal Forms

Inicie o IntelliJ e abra o projeto BankingApplication criado na etapa [anterior.](./getting-started.md) Expanda ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Para usar qualquer componente principal (incluindo os componentes de portal prontos para uso) em um site do Adobe Experience Manager (AEM), você deve criar um componente proxy e habilitá-lo para o seu site.
O componente proxy recém-criado precisa apontar para o componente de formulários pronto para uso, para que eles herdem tudo deles. Isso é feito alterando o resourceSuperType no content.xml do componente proxy. No content.xml, também especificamos o título e o grupo de componentes.
>[!NOTE]
>
> Você pode construir o supertipo de recurso para cada um destes [componentes daqui](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Rascunhos e envios

Faça uma cópia de um componente existente (por exemplo, `button`) e nomeie-o como _rascunhos e envios_.
![rascunhos e envios](assets/forms-portal-components2.png)
Substituir o conteúdo de `.content.xml` pelo seguinte XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Pesquisa e listagem

Faça uma cópia do componente de botão e renomeie-o para _searchandlister_.
Substituir o conteúdo de `.content.xml` pelo seguinte XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Componente do link

Faça uma cópia do componente de botão e renomeie-o para _link_.
Substituir o conteúdo de `.content.xml` pelo seguinte XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Depois que o projeto for implantado, você poderá usar esses componentes na página do AEM para criar o portal do Forms.

## Próximas etapas

[Incluir configuração de serviços em nuvem](./azure-storage-fdm.md)
