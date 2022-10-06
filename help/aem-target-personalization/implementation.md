---
title: Integração do Adobe Experience Manager com o Adobe Target
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: Um artigo sobre como configurar o Adobe Experience Manager com o Adobe Target para diferentes cenários.
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 5%

---

# Integração do Adobe Experience Manager com o Adobe Target

Nesta seção, discutiremos como configurar o Adobe Experience Manager com o Adobe Target para diferentes cenários. Com base em seu cenário e requisitos organizacionais.

* **Adicionar a biblioteca JavaScript do Adobe Target (necessária para todos os cenários)**
Para sites hospedados em AEM, você pode adicionar bibliotecas do Target ao seu site usando, [Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). O Launch fornece uma forma simples de implantar e gerenciar todas as tags necessárias para potencializar experiências de cliente relevantes.
* **Adicionar as Cloud Services do Adobe Target (necessário para o cenário Fragmentos de experiência)**
Para AEM clientes, que gostaria de usar as ofertas de Fragmento de experiência para criar uma atividade no Adobe Target, será necessário integrar o Adobe Target com o AEM usando os Cloud Services herdados. Essa integração é necessária para enviar Fragmentos de experiência do AEM para o Target como ofertas HTML/JSON e para manter as ofertas sincronizadas com o AEM. 
*Essa integração é necessária para implementar o cenário 1.*

## Pré-requisitos

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*o Service Pack mais recente é recomendado*)
   * Faça o download AEM pacotes de site de referência WKND
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componentes principais](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Camada de dados digitais](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console Adobe I/O](https://console.adobe.io)

* **Autor**
   * Java 1.8 ou Java 11 (somente AEM 6.5+)
   * Apache Maven (3.3.9 ou mais recente)
   * Chrome

>[!NOTE]
>
> O cliente precisa ser provisionado com Experience Platform Launch e Adobe I/O de [Suporte a Adobe](https://helpx.adobe.com/br/contact/enterprise-support.ec.html) ou entre em contato com o administrador do sistema

### Configurar AEM{#set-up-aem}

AEM instância de criação e publicação é necessária para concluir este tutorial. Temos a instância do autor em execução em `http://localhost:4502` e publicar instância em execução em `http://localhost:4503`. Para obter mais informações, consulte: [Configurar um Ambiente de Desenvolvimento de AEM Local](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configurar instâncias de publicação e autor do AEM

1. Obtenha uma cópia do [AEM Quickstart Jar e uma licença.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Crie uma estrutura de pastas no computador como a seguinte:
   ![Estrutura da pasta](assets/implementation/aem-setup-1.png)
3. Renomeie o jar do Início Rápido para `aem-author-p4502.jar` e coloque-a abaixo da `/author` diretório. Adicione o `license.properties` abaixo do `/author` diretório.
   ![Instância de autor do AEM](assets/implementation/aem-setup-author.png)
4. Faça uma cópia do jar do Quickstart, renomeie-o para `aem-publish-p4503.jar` e coloque-a abaixo da `/publish` diretório. Adicione uma cópia do `license.properties` abaixo do `/publish` diretório.
   ![Instância de publicação do AEM](assets/implementation/aem-setup-publish.png)
5. Clique duas vezes na `aem-author-p4502.jar` para instalar a instância do autor. Isso iniciará a instância do autor, executando na porta 4502 no computador local.
6. Faça logon usando as credenciais abaixo e, após o logon bem-sucedido, você será direcionado para a Tela da Página Inicial do AEM.
nome de usuário : **administrador**
senha : **administrador**
   ![Instância de publicação do AEM](assets/implementation/aem-author-home-page.png)
7. Clique duas vezes na `aem-publish-p4503.jar` para instalar uma instância de publicação. Você pode notar uma nova guia aberta no navegador para a sua instância de publicação, executando na porta 4503 e exibindo a página inicial do WeRetail. Estamos usando o site de referência WKND para este tutorial e vamos instalar os pacotes na instância do autor.
8. Navegue até o AEM Author em seu navegador da Web em `http://localhost:4502`. Na tela inicial AEM, navegue até *[Ferramentas > Implantação > Pacotes](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Baixe e faça upload dos pacotes para AEM (listado acima em *[Pré-requisitos > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Depois de instalar os pacotes no AEM Author, selecione cada pacote carregado AEM Gerenciador de pacotes e selecione **Mais > Replicar** para garantir que os pacotes sejam implantados no AEM Publish.
11. Neste ponto, você instalou com sucesso o site de referência WKND e todos os pacotes adicionais necessários para este tutorial.

[PRÓXIMO CAPÍTULO](./using-launch-adobe-io.md): No próximo capítulo, você integrará o Launch com o AEM.
