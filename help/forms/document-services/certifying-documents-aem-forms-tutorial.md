---
title: Certificar documento no AEM Forms
description: Utilização do serviço Docassurance para certificar documentos de PDF no AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 75
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# Certificar documento no AEM Forms

Um Documento certificado fornece aos destinatários do documento de PDF e dos formulários garantias adicionais de sua autenticidade e integridade.

Para certificar um documento, você pode usar o Acrobat DC no desktop ou os Serviços de documento da AEM Forms como parte de um processo automatizado em um servidor.

Este artigo fornece um exemplo de pacote OSGI para certificar documentos PDF usando os Serviços de documento da AEM Forms. O código usado na amostra está [disponível aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos usando o AEM Forms, as seguintes etapas precisam ser seguidas

## Adicionando certificado ao armazenamento confiável {#adding-certificate-to-trust-store}

Siga as etapas mencionadas abaixo para adicionar o certificado ao keystore no AEM

* [Inicializar repositório de confiança global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Procurar usuário fd-service](http://localhost:4502/security/users.html)
* **Será necessário rolar a página de resultados para carregar todos os usuários para localizar o usuário do serviço fd**
* Clique duas vezes no usuário do serviço fd para abrir a janela de configurações do usuário
* Clique em &quot;Adicionar chave de privacidade do arquivo de armazenamento de chaves&quot;. Especifique o alias e a senha específicos do certificado
  ![adicionar-certificado](assets/adding-certificate-keystore.PNG)
* Salve as alterações

## Criação do serviço OSGI

Você pode criar seu próprio pacote OSGi e usar o AEM Forms Client SDK para implementar um serviço para certificar documentos PDF. Os links a seguir seriam úteis para escrever seu próprio pacote OSGi

* [Criando seu primeiro pacote OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Usar API de serviço de documento](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Ou você pode usar o pacote de amostra incluído como parte deste tutorial de ativos.

>[!NOTE]
>
>O pacote de amostra usa um alias chamado &quot;ares&quot; para certificar os documentos. Portanto, certifique-se de que seu alias seja chamado de &quot;ares&quot; ao usar esse pacote

## Teste da amostra no seu sistema local

* Baixe e instale o [Pacote de Serviços de Documentos Personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Baixe e instale o [Desenvolvimento com o Conjunto de Usuários de Serviço](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Verifique se você adicionou a seguinte entrada no Serviço de Mapeador de Usuários do Apache Sling Service](http://localhost:4502/system/console/configMgr)
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** como mostrado na captura de tela abaixo
  ![Mapeador de Usuários](assets/user-mapper-service.PNG)
* [Importar exemplo de formulário adaptável](assets/certify-pdf-af.zip)
* [Importe e instale o envio personalizado](assets/custom-submit-certify.zip)
* [Abrir o Formulário Adaptável](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Carregar documento de PDF que precisa ser certificado
  **opcional** - especifique o campo de assinatura que deseja usar ao certificar o documento
* Clique em enviar.
* O PDF certificado deverá ser devolvido a você.
