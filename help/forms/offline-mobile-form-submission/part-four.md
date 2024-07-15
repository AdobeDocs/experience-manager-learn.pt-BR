---
title: Acionar o fluxo de trabalho de AEM no envio de formulário HTM5 - fazendo com que o caso de uso funcione
description: Continue preenchendo o formulário para publicação de conteúdo para dispositivos móveis no modo offline e envie o formulário para publicação de conteúdo para dispositivos móveis para acionar o fluxo de trabalho do AEM
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 90
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---

# Fazendo com que este caso de uso funcione em seu sistema

>[!NOTE]
>
>Para que os ativos de amostra funcionem em seu sistema, presume-se que você tenha o AEM Author e a instância do Publish em execução nas portas 4502 e 4503, respectivamente. Também presume-se que o Autor do AEM possa ser acessado via `admin`/`admin`. Se os números de porta ou a senha do administrador tiverem sido alterados, esses ativos de amostra não funcionarão. Você terá que criar seus próprios ativos usando o código de amostra fornecido.

Para que esse caso de uso funcione no sistema local, siga estas etapas:

* Instale a instância AEM Author na porta 4502 e a instância AEM Publish na porta 4503
* [Siga as instruções especificadas em desenvolvimento com usuário de serviço no AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Crie o usuário de serviço e implante o pacote no seu autor de AEM e na instância do Publish.
* [Abra a configuração osgi](http://localhost:4503/system/console/configMgr).
* Procure por **Filtro referenciador Apache Sling**. Certifique-se de que a caixa de seleção Permitir vazio esteja marcada.
* [Implantar o Pacote AEMFormDocumentService personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Esse pacote precisa ser implantado em sua instância do AEM Publish. Este pacote tem o código para gerar PDF interativo de formulários móveis.
* [Baixe e descompacte os ativos relacionados a este artigo.](assets/offline-pdf-submission-assets.zip) Você obterá o seguinte
   * **offline-submit-profile.zip** - Este pacote de AEM contém o perfil personalizado que permite baixar o pdf interativo no sistema de arquivos local. Implante esse pacote na instância do Publish AEM.
   * **xdp-form-and-workflow.zip** - Este pacote de AEM contém XDP, exemplo de fluxo de trabalho, iniciador configurado no nó content/pdfsubmissions. Implante esse pacote no autor do AEM e na instância do Publish.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - Este é o pacote AEM que faz a maior parte do trabalho. Este pacote contém o servlet montado em `/bin/startworkflow`. Este servlet salva os dados de formulário enviados no nó `/content/pdfsubmissions` no repositório AEM. Implante esse pacote em seu autor AEM e na instância do Publish.
* [Visualizar o formulário móvel](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Preencha vários campos e clique no botão na barra de ferramentas para baixar o PDF interativo.
* Preencha o PDF baixado usando o Acrobat e pressione o botão enviar.
* Você deve receber uma mensagem de sucesso
* Fazer logon na instância do autor do AEM como administrador
* [Verificar a Caixa de Entrada do AEM](http://localhost:4502/aem/inbox)
* Você deve ter um item de trabalho para revisar o PDF enviado

>[!NOTE]
>
>Em vez de enviar o PDF para o servlet em execução na instância de publicação, alguns clientes implantaram o servlet no container de servlet, como o Tomcat. Tudo depende da topologia com a qual o cliente está familiarizado.para o propósito deste tutorial, vamos usar o servlet implantado na instância de publicação para lidar com os envios de pdf.
