---
title: Certificando documento no AEM Forms
description: Uso do serviço DocAssurance para certificar documentos do PDF no AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 1%

---

# Certificando documento no AEM Forms

Um Documento certificado fornece documento PDF e forma destinatários com mais garantias de autenticidade e integridade.

Para certificar um documento, você pode usar o Acrobat DC no desktop ou nos AEM Forms Document Services como parte de um processo automatizado em um servidor.

Este artigo fornece um pacote OSGI de amostra para certificar documentos pdf usando o AEM Forms Document Services. O código usado na amostra é [disponível aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos usando o AEM Forms, as etapas a seguir precisam ser seguidas

## Adicionar certificado ao repositório de confiança {#adding-certificate-to-trust-store}

Siga as etapas mencionadas abaixo para adicionar o certificado ao keystore no AEM

* [Inicializar repositório de confiança global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Pesquisar por fd-service](http://localhost:4502/security/users.html) usuário
* **Será necessário rolar a página de resultados para carregar todos os usuários e encontrar o usuário fd-service**
* Clique duas vezes no usuário fd-service para abrir a janela de configurações do usuário
* Clique em &quot;Adicionar chave privada do arquivo do repositório de chaves&quot;. Especifique o alias e a senha específicos do certificado
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Salve as alterações

## Criação do serviço OSGI

Você pode escrever seu próprio pacote OSGi e usar o SDK do cliente da AEM Forms para implementar um serviço para certificar documentos do PDF. Os links a seguir seriam úteis para escrever seu próprio pacote OSGi

* [Criação do seu primeiro pacote OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Usar API de Serviço de Documento](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Ou você pode usar o pacote de amostra incluído como parte desses ativos tutoriais.

>[!NOTE]
>
>O pacote de amostra usa alias chamado &quot;ares&quot; para certificar os documentos. Portanto, certifique-se de que seu alias seja chamado de &quot;ares&quot; ao usar este pacote

## Teste da amostra em seu sistema local

* Baixe e instale [Pacote de serviços de documento personalizado](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Baixe e instale [Desenvolvimento com o pacote de usuários de serviço](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Certifique-se de ter adicionado a seguinte entrada no serviço Mapeador de Usuário do Apache Sling Service](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** como mostrado na captura de tela abaixo
   ![Mapeador de usuários](assets/user-mapper-service.PNG)
* [Importar formulário adaptável de amostra](assets/certify-pdf-af.zip)
* [Importe e instale o Envio personalizado](assets/custom-submit-certify.zip)
* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Fazer upload do documento PDF que precisa ser certificado
   **opcional** - Especifique o campo de assinatura que deseja usar na certificação do documento
* Clique em enviar.
* O PDF certificado deve ser devolvido a você.
