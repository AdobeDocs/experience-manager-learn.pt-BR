---
title: Acionar AEM fluxo de trabalho no envio de formulário HTML5 - Obter caso de uso para funcionar
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
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
ht-degree: 0%

---

# Este caso de uso funciona no sistema

>[!NOTE]
>
>Para que os ativos de amostra funcionem no seu sistema, presume-se que você tenha a instância Autor e Publicação do AEM em execução nas portas 4502 e 4503, respectivamente. Também é considerado que o autor do AEM está acessível por meio de `admin`/`admin`. Se os números da porta ou a senha do administrador tiverem sido alterados, esses ativos de exemplo não funcionarão. Será necessário criar seus próprios ativos usando o código de amostra fornecido.

Para que esse caso de uso funcione em seu sistema local, siga estas etapas:

* Instale a instância do autor do AEM na porta 4502 e a instância de publicação do AEM na porta 4503
* [Siga as instruções especificadas em desenvolvimento com o usuário do serviço no AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Crie o usuário do serviço e implante o pacote em sua instância de Autor e Publicação do AEM.
* [Abra a configuração do osgi ](http://localhost:4503/system/console/configMgr).
* Procurar por  **Filtro de referenciador do Apache Sling**. Certifique-se de que a caixa de seleção Permitir vazio esteja marcada.
* [Implantar o pacote AEMFormDocumentService personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Este pacote precisa ser implantado na instância de publicação do AEM. Esse pacote tem o código para gerar o PDF interativo a partir de um formulário móvel.
* [Baixe e descompacte os ativos relacionados a este artigo.](assets/offline-pdf-submission-assets.zip) Você terá o seguinte
   * **offline-submit-profile.zip** - Este pacote de AEM contém o perfil personalizado que permite baixar o pdf interativo para o sistema de arquivos local. Implante este pacote na instância de publicação do AEM.
   * **xdp-form-and-workflow.zip** - Este pacote de AEM contém XDP, exemplo de fluxo de trabalho, iniciador configurado no conteúdo do nó/pdfenvios. Implante este pacote em sua instância de Autor e Publicação do AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - Este é o pacote AEM que faz a maior parte do trabalho. Este pacote contém o servlet montado em `/bin/startworkflow`. Este servlet salva os dados de formulário enviados em `/content/pdfsubmissions` no repositório AEM. Implante esse pacote em suas instâncias de Autor e Publicação do AEM.
* [Visualizar o formulário móvel](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Preencha vários campos e clique no botão na barra de ferramentas para baixar o PDF interativo.
* Preencha o PDF baixado usando o Acrobat e pressione o botão Enviar.
* Você deve receber uma mensagem de sucesso
* Faça logon na instância do autor do AEM como administrador
* [Marque a Caixa de entrada AEM](http://localhost:4502/aem/inbox)
* Você deve ter um item de trabalho para revisar o PDF enviado

>[!NOTE]
>
>Em vez de enviar o PDF para servlet em execução na instância de publicação, alguns clientes implantaram o servlet no container do servlet, como Tomcat. Tudo depende da topologia com a qual o cliente está familiarizado.para a finalidade deste tutorial, vamos usar o servlet implantado na instância de publicação para lidar com os envios de pdf.
