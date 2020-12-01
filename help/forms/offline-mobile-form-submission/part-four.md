---
title: Acionar fluxo de trabalho AEM no envio de formulário HTM5
seo-title: Acionar fluxo de trabalho AEM no envio de formulário HTML5
description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
seo-description: Continue preenchendo o formulário móvel no modo offline e envie o formulário móvel para acionar AEM fluxo de trabalho
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---


# Como obter este caso de uso para funcionar no sistema

>[!NOTE]
>
>Para que os ativos de amostra funcionem em seu sistema, presume-se que você tenha uma instância de autor e publicação do AEM em execução nas portas 4502 e 4503, respectivamente. Também é pressuposto que o autor de AEM esteja acessível via `admin`/`admin`. Se os números das portas ou a senha do administrador tiverem sido alterados, esses ativos de amostra não funcionarão. Será necessário criar seus próprios ativos usando o código de amostra fornecido.

Para que esse caso de uso funcione no sistema local, siga estas etapas:

* Instale a instância de autor de AEM na porta 4502 e a instância de publicação de AEM na porta 4503
* [Siga as instruções especificadas ao desenvolver com o usuário do serviço no AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Certifique-se de criar o usuário do serviço e implantar o pacote em sua instância de autor e publicação do AEM.
* [Abra a configuração osgi  ](http://localhost:4503/system/console/configMgr).
* Procure **Filtro de Quem indicou Apache Sling**. Verifique se a caixa de seleção Permitir vazio está selecionada.
* [Implante o pacote](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) AEMFormDocumentService personalizado.Esse pacote precisa ser implantado na instância de publicação do AEM. Esse pacote tem o código para gerar um PDF interativo a partir de um formulário móvel.
* [Baixe e descompacte os ativos relacionados a este artigo.](assets/offline-pdf-submission-assets.zip) Você receberá o seguinte
   * **offline-submit-perfil.zip** - Este pacote de AEM contém o perfil personalizado que permite baixar o pdf interativo no sistema de arquivos local. Implante este pacote na sua instância de publicação de AEM.
   * **xdp-form-and-workflow.zip** - Este pacote AEM contém XDP, fluxo de trabalho de amostra, iniciador configurado no conteúdo do nó/pdfsubmit. Implante este pacote em sua instância de autor e publicação do AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  - Este é o pacote AEM que executa a maior parte do trabalho. Este pacote contém o servlet montado em `/bin/startworkflow`. Este servlet salva os dados de formulário enviados no nó `/content/pdfsubmissions` AEM repositório. Implante esse pacote em sua instância de autor e publicação do AEM.
* [Pré-visualização do formulário móvel](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Preencha vários campos e clique no botão na barra de ferramentas para baixar o PDF interativo.
* Preencha o PDF baixado usando o Acrobat e pressione o botão Enviar.
* Você deve receber uma mensagem de sucesso
* Fazer logon na instância de autor de AEM como administrador
* [Verifique a Caixa de entrada AEM](http://localhost:4502/aem/inbox)
* Você deve ter um item de trabalho para revisar o PDF enviado

>[!NOTE]
>
>Em vez de enviar o PDF para o servlet em execução na instância de publicação, alguns clientes implantaram o servlet no container do servlet, como Tomcat. Tudo depende da topologia com que o cliente está confortável.para a finalidade deste tutorial, vamos usar o servlet implantado na instância de publicação para lidar com os envios de pdf.

