---
title: Acionar o fluxo de trabalho de AEM no envio de formulário HTM5 - fazendo com que o caso de uso funcione
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continue preenchendo o formulário para publicação de conteúdo para dispositivos móveis no modo offline e envie o formulário para publicação de conteúdo para dispositivos móveis para acionar o fluxo de trabalho do AEM
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 1%

---

# Fazendo com que este caso de uso funcione em seu sistema

>[!NOTE]
>
>Para que os ativos de amostra funcionem em seu sistema, presume-se que você tenha a instância de Autor e Publicação do AEM em execução nas portas 4502 e 4503, respectivamente. Também presume-se que o autor do AEM possa ser acessado por meio de `admin`/`admin`. Se os números de porta ou a senha do administrador tiverem sido alterados, esses ativos de amostra não funcionarão. Você terá que criar seus próprios ativos usando o código de amostra fornecido.

Para que esse caso de uso funcione no sistema local, siga estas etapas:

* Instale a instância do AEM Author na porta 4502 e a instância de publicação do AEM na porta 4503
* [Siga as instruções especificadas em desenvolvimento com usuário de serviço no AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Crie o usuário de serviço e implante o pacote na instância de Autor e Publicação do AEM.
* [Abra a configuração osgi ](http://localhost:4503/system/console/configMgr).
* Pesquisar por  **Filtro referenciador do Apache Sling**. Certifique-se de que a caixa de seleção Permitir vazio esteja marcada.
* [Implantar o pacote AEMFormDocumentService personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Esse pacote precisa ser implantado em sua instância de publicação do AEM. Este pacote tem o código para gerar PDF interativo de formulários móveis.
* [Baixe e descompacte os ativos relacionados a este artigo.](assets/offline-pdf-submission-assets.zip) Você terá o seguinte
   * **offline-submit-profile.zip** - Este pacote AEM contém o perfil personalizado que permite baixar o pdf interativo para o seu sistema de arquivos local. Implante esse pacote na instância de publicação do AEM.
   * **xdp-form-and-workflow.zip** - Esse pacote de AEM contém XDP, exemplo de fluxo de trabalho, iniciador configurado nos envios de conteúdo/pdfdo nó. Implante esse pacote na instância do autor e de publicação do AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - Esse é o pacote AEM que faz a maior parte do trabalho. Esse pacote contém o servlet montado em `/bin/startworkflow`. Esse servlet salva os dados de formulário enviados em `/content/pdfsubmissions` no repositório AEM. Implante esse pacote na instância do autor e de publicação do AEM.
* [Pré-visualizar o formulário para dispositivos móveis](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Preencha vários campos e clique no botão na barra de ferramentas para baixar o PDF interativo.
* Preencha o PDF baixado usando o Acrobat e pressione o botão enviar.
* Você deve receber uma mensagem de sucesso
* Faça logon na instância do autor do AEM como administrador
* [Marque a caixa de entrada do AEM](http://localhost:4502/aem/inbox)
* Você deve ter um item de trabalho para revisar o PDF enviado

>[!NOTE]
>
>Em vez de enviar o PDF para o servlet em execução na instância de publicação, alguns clientes implantaram o servlet no container de servlet, como o Tomcat. Tudo depende da topologia com a qual o cliente está familiarizado.para o propósito deste tutorial, vamos usar o servlet implantado na instância de publicação para lidar com os envios de pdf.
