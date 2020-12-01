---
title: Integração do Adobe Experience Manager com o Adobe Target
seo-title: Um artigo que aborda diferentes maneiras de integrar o Adobe Experience Manager(AEM) à Adobe Target para fornecer conteúdo personalizado.
description: Um artigo que aborda como configurar o Adobe Experience Manager com o Adobe Target para diferentes cenários.
seo-description: Um artigo que aborda como configurar o Adobe Experience Manager com o Adobe Target para diferentes cenários.
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 4%

---


# Integração do Adobe Experience Manager com o Adobe Target

Nesta seção, discutiremos como configurar o Adobe Experience Manager com a Adobe Target para diferentes cenários. Com base no seu cenário e nos requisitos organizacionais.

* **Adicionar biblioteca JavaScript da Adobe Target (necessário para todos os cenários)**
Para sites hospedados em AEM, você pode adicionar bibliotecas de Públicos alvos ao seu site usando o  [Launch](https://docs.adobe.com/content/help/en/launch/using/overview.html). O Launch fornece uma maneira simples de implantar e gerenciar todas as tags necessárias para potencializar as experiências relevantes do cliente.
* **Adicione os Cloud Services Adobe Target (necessários para o cenário Fragmentos de experiência)**
Para clientes AEM que desejam usar as ofertas de fragmento de experiência para criar uma atividade no Adobe Target, é necessário integrar a Adobe Target com AEM usando os Cloud Services herdados. Essa integração é necessária para enviar Fragmentos de experiência de AEM para Público alvo como ofertas HTML/JSON e para manter as ofertas sincronizadas com AEM. 
*Essa integração é necessária para a implementação do cenário 1.*

## Pré-requisitos

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*o Service Pack mais recente é recomendado*)
   * Baixar AEM pacotes de site de referência WKND
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [Componentes principais](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Camada de dados digitais](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - <https://>`<yourcompany>`.experience.ecloud.adobe.com
   * Experience Cloud fornecido com as seguintes soluções
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O Console](https://console.adobe.io)

* **Autor**
   * Java 1.8 ou Java 11 (somente AEM 6.5+)
   * Apache Maven (3.3.9 ou mais recente)
   * Cromo

>[!NOTE]
>
> O cliente precisa ser provisionado com o Experience Platform Launch e o Adobe I/O a partir de [suporte ao Adobe](https://helpx.adobe.com/br/contact/enterprise-support.ec.html) ou entre em contato com o administrador do sistema

### Configurar AEM{#set-up-aem}

AEM instância de autor e publicação é necessária para concluir este tutorial. Temos a instância do autor em execução em `http://localhost:4502` e a instância de publicação em execução em `http://localhost:4503`. Para obter mais informações, consulte: [Configure um Ambiente de Desenvolvimento de AEM Local](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### Configurar instâncias de autor e publicação do AEM

1. Obtenha uma cópia do [AEM Quickstart Jar e uma licença.](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. Crie uma estrutura de pastas em seu computador como a seguinte:
   ![Estrutura da pasta](assets/implementation/aem-setup-1.png)
3. Renomeie o jar do Quickstart para `aem-author-p4502.jar` e coloque-o abaixo do diretório `/author`. Adicione o arquivo `license.properties` abaixo do diretório `/author`.
   ![Instância do autor do AEM](assets/implementation/aem-setup-author.png)
4. Faça uma cópia do jar do Quickstart, renomeie-o para `aem-publish-p4503.jar` e coloque-o sob o diretório `/publish`. Adicione uma cópia do arquivo `license.properties` abaixo do diretório `/publish`.
   ![Instância de publicação do AEM](assets/implementation/aem-setup-publish.png)
5. Duplo clique no arquivo `aem-author-p4502.jar` para instalar a instância do autor. Isso start a instância do autor, que é executada na porta 4502 no computador local.
6. Faça logon usando as credenciais a seguir e, depois do login bem-sucedido, você será direcionado para a tela AEM do Home page.
nome de usuário: **admin**
senha : **admin**
   ![Instância de publicação do AEM](assets/implementation/aem-author-home-page.png)
7. Duplo clique no arquivo `aem-publish-p4503.jar` para instalar uma instância de publicação. Você pode notar uma nova guia aberta no navegador para a instância de publicação, executando na porta 4503 e exibindo o home page WeRetail. Usaremos o site de referência WKND para este tutorial e instalaremos os pacotes na instância do autor.
8. Navegue até Autor de AEM em seu navegador da Web em `http://localhost:4502`. Na tela AEM Start, navegue até *[Ferramentas > Implantação > Pacotes](http://localhost:4502/crx/packmgr/index.jsp)*.
9. Baixe e carregue os pacotes para AEM (listados acima em *[Pré-requisitos > AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. Depois de instalar os pacotes no AEM Author, selecione cada pacote carregado AEM Gerenciador de pacotes e selecione **Mais > Replicar** para garantir que os pacotes sejam implantados no AEM Publish.
11. Nesse ponto, você instalou com êxito seu site de referência WKND e todos os pacotes adicionais necessários para este tutorial.

[PRÓXIMO CAPÍTULO](./using-launch-adobe-io.md): No próximo capítulo, você integrará o Launch com AEM.
