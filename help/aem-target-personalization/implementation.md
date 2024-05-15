---
title: Integração do AEM Sites com o Adobe Target
description: Um artigo que aborda como configurar o Adobe Experience Manager com o Adobe Target para diferentes cenários.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 1%

---

# Integração do AEM Sites com o Adobe Target

Nesta seção, discutiremos como configurar o Adobe Experience Manager Sites com o Adobe Target para diferentes cenários. Com base em seu cenário e requisitos organizacionais.

* **Adicionar biblioteca JavaScript do Adobe Target (obrigatório para todos os cenários)**
Para sites hospedados no AEM, é possível adicionar bibliotecas do Target ao seu site usando, [tags na Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). As tags oferecem uma forma simples de implantar e gerenciar todas as tags necessárias para potencializar experiências de cliente relevantes.
* **Adicionar os Cloud Service do Adobe Target (necessário para o cenário de Fragmentos de experiência)**
Para clientes do AEM, que gostariam de usar as ofertas de Fragmento de experiência para criar uma atividade no Adobe Target, será necessário integrar o Adobe Target ao AEM usando os Cloud Service herdados. Essa integração é necessária para enviar Fragmentos de experiência do AEM para o Target como ofertas HTML/JSON e manter as ofertas em sincronia com o AEM. *Essa integração é necessária para a implementação do cenário 1.*

## Pré-requisitos

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*O Service Pack mais recente é recomendado*)
   * Baixar pacotes de site de referência WKND para AEM
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componentes principais](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Camada de dados digitais](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Coleta de dados](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Console Adobe I/O](https://console.adobe.io)

* **Ambiente**
   * Java 1.8 ou Java 11 (somente AEM 6.5+)
   * Apache Maven (3.3.9 ou mais recente)
   * Cromo

>[!NOTE]
>
> O cliente precisa ser provisionado com a coleta de dados e o Adobe I/O da [suporte para Adobe](https://helpx.adobe.com/br/contact/enterprise-support.ec.html) ou entre em contato com o administrador do sistema

### Configurar AEM{#set-up-aem}

A instância de autor e publicação do AEM é necessária para concluir este tutorial. A instância do autor está em execução `http://localhost:4502` e instância de publicação em execução em `http://localhost:4503`. Para obter mais informações, consulte: [Configurar um ambiente de desenvolvimento do AEM local](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configurar instâncias de publicação e criação do AEM

1. Obtenha uma cópia do [AEM Quickstart Jar e uma licença.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Crie uma estrutura de pastas no seu computador da seguinte maneira:
   ![Estrutura de pastas](assets/implementation/aem-setup-1.png)
3. Renomeie o Quickstart jar para `aem-author-p4502.jar` e coloque-o abaixo de `/author` diretório. Adicione o `license.properties` arquivo abaixo de `/author` diretório.
   ![Instância de autor AEM](assets/implementation/aem-setup-author.png)
4. Faça uma cópia do Quickstart jar, renomeie-a para `aem-publish-p4503.jar` e coloque-o abaixo de `/publish` diretório. Adicione uma cópia do `license.properties` arquivo abaixo de `/publish` diretório.
   ![Instância de publicação do AEM](assets/implementation/aem-setup-publish.png)
5. Clique duas vezes na `aem-author-p4502.jar` arquivo para instalar a instância do Author. Isso iniciará a instância do autor, executando na porta 4502 no computador local.
6. Faça logon usando as credenciais abaixo e, depois de fazer logon, você será direcionado para a tela da página inicial do AEM.
nome de usuário : **administrador**
senha: **administrador**
   ![Instância de publicação do AEM](assets/implementation/aem-author-home-page.png)
7. Clique duas vezes na `aem-publish-p4503.jar` arquivo para instalar uma instância de publicação. Você pode notar que uma nova guia é aberta no navegador para a instância de publicação, sendo executada na porta 4503 e exibindo a página inicial do WeRetail. Estamos usando o site de referência WKND para este tutorial e vamos instalar os pacotes na instância do autor.
8. Navegue até AEM Author em seu navegador da Web, em `http://localhost:4502`. Na tela inicial do AEM, acesse *[Ferramentas > Implantação > Pacotes](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Baixe e carregue os pacotes para AEM (listados acima em *[Pré-requisitos > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Depois de instalar os pacotes no AEM Author, selecione cada pacote carregado no Gerenciador de pacotes AEM e selecione **Mais > Replicar** para garantir que os pacotes sejam implantados no AEM Publish.
11. Neste ponto, você instalou com êxito seu site de referência WKND e todos os pacotes adicionais necessários para este tutorial.

[PRÓXIMO CAPÍTULO](./using-launch-adobe-io.md): No próximo capítulo, você integrará tags ao AEM.
