---
title: Certificando documento no AEM Forms
description: Uso do serviço DocAssurance para certificar documentos PDF no AEM Forms
feature: Segurança de documentos
version: 6.4,6.5
topic: Desenvolvimento
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---


# Certificando documento no AEM Forms

Um Documento certificado fornece documentos PDF e formulários a recipients com mais garantias de autenticidade e integridade.

Para certificar um documento, você pode usar o Acrobat DC na área de trabalho ou os AEM Forms Document Services como parte de um processo automatizado em um servidor.

Este artigo fornece um pacote OSGI de amostra para certificar documentos pdf usando o AEM Forms Document Services. O código usado na amostra é [disponível aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos usando o AEM Forms, as etapas a seguir precisam ser seguidas

## Adicionar certificado ao repositório de confiança {#adding-certificate-to-trust-store}

Siga as etapas mencionadas abaixo para adicionar o certificado ao keystore no AEM

* [Inicializar repositório de confiança global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Pesquisar fd-](http://localhost:4502/security/users.html) serviceuser
* **Será necessário rolar a página de resultados para carregar todos os usuários e encontrar o usuário fd-service**
* Clique duas vezes no usuário fd-service para abrir a janela de configurações do usuário
* Clique em &quot;Adicionar chave privada do arquivo do repositório de chaves&quot;. Especifique o alias e a senha específicos do certificado
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Salve as alterações

## Criação do serviço OSGI

Você pode gravar seu próprio pacote OSGi e usar o SDK do cliente da AEM Forms para implementar um serviço para certificar documentos PDF. Os links a seguir seriam úteis para escrever seu próprio pacote OSGi

* [Criação do seu primeiro pacote OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Usar API de Serviço de Documento](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Ou você pode usar o pacote de amostra incluído como parte desses ativos tutoriais.

>[!NOTE]
>
>O pacote de amostra usa alias chamado &quot;ares&quot; para certificar os documentos. Portanto, certifique-se de que seu alias seja chamado de &quot;ares&quot; ao usar este pacote

## Teste da amostra em seu sistema local

* Baixe e instale o [Pacote de serviços de documento personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Baixe e instale [Desenvolvendo com o Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Certifique-se de ter adicionado a seguinte entrada no serviço Mapeador de Usuário do Apache Sling Service](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** service, como mostrado na captura de tela abaixo
   ![Mapeador de usuários](assets/user-mapper-service.PNG)
* [Importar formulário adaptável de amostra](assets/certify-pdf-af.zip)
* [Importe e instale o Envio personalizado](assets/custom-submit-certify.zip)
* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Carregar documento PDF que precisa ser certificado
   **opcional**  - especifique o campo de assinatura que deseja usar na certificação do documento
* Clique em enviar.
* O PDF certificado deve ser retornado para você.


