---
title: Ativar componentes do AEM Forms Portal
description: Criar um portal do AEM Forms usando componentes principais
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 10373
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Componentes do Forms Portal

O AEM Forms fornece os seguintes componentes de portal prontos para uso:

**Pesquisar &amp; Lister**: Esse componente permite listar formulários do repositório de formulários na página do portal e fornece opções de configuração para listar formulários com base em critérios especificados.

**Rascunhos e envios**: Enquanto o componente Pesquisar e listar exibe formulários que são tornados públicos pelo autor do Forms, o componente Rascunhos e envios exibe formulários que são salvos como rascunho para preencher formulários posteriores e enviados. Esse componente fornece experiência personalizada para qualquer usuário conectado.

**Link**: Esse componente permite criar um link para um formulário em qualquer lugar na página.

## Ativar componentes do Forms Portal

Inicie o IntelliJ e abra o projeto BankingApplication criado no [etapa anterior.](./getting-started.md) Expanda a interface ui.apps->src->main->content->jcr_root->apps.bankingapplication->components

Para usar qualquer componente principal (incluindo os componentes prontos para uso do portal) em um site do Adobe Experience Manager (AEM), você deve criar um componente proxy e ativá-lo para o site.
O componente proxy recém-criado precisa apontar para o componente de formulários prontos para uso, para que herdem tudo dele. Isso é feito alterando o resourceSuperType no content.xml do componente proxy. No content.xml, também especificamos o título e o grupo de componentes.
>[!NOTE]
>
> Você pode criar o super tipo de recurso para cada um dos [esses componentes daqui](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Rascunhos e envios

Fazer uma cópia de um componente existente (por exemplo, `button`) e nomeá-lo como _projetos de parecer_.
![projetos de parecer](assets/forms-portal-components2.png)
Substitua o conteúdo no `.content.xml` com o seguinte XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Pesquisar e Lister

Faça uma cópia do componente de botão e renomeie-o para _searchandlister_.
Substitua o conteúdo no `.content.xml` com o seguinte XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Componente de link

Faça uma cópia do componente de botão e renomeie-o para _link_.
Substitua o conteúdo no `.content.xml` com o seguinte XML:


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Depois que o projeto for implantado, você poderá usar esses componentes na página de AEM para criar o portal do Forms.