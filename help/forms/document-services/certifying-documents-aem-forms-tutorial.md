---
title: Certificando Documento no AEM Forms
seo-title: Certificando Documento no AEM Forms
description: Uso do serviço DocAssurance para certificar documentos PDF no AEM Forms
seo-description: Uso do serviço DocAssurance para certificar documentos PDF no AEM Forms
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 0%

---


# Certificando Documento no AEM Forms

Um Documento certificado fornece aos recipient de formulários e documentos de PDF mais garantias de autenticidade e integridade.

Para certificar um documento, você pode usar o Acrobat DC no desktop ou os Serviços de Documento AEM Forms como parte de um processo automatizado em um servidor.

Este artigo fornece uma amostra do pacote OSGI para certificar documentos pdf usando o AEM Forms Documento Services. O código usado na amostra é [disponível aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Para certificar documentos usando o AEM Forms, siga as etapas a seguir

## Adicionando certificado ao repositório de confiança {#adding-certificate-to-trust-store}

Siga as etapas abaixo para adicionar o certificado ao keystore no AEM

* [Inicializar o repositório de confiança global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Procurar fd-](http://localhost:4502/security/users.html) serviceuser
* **Será necessário rolar a página de resultados para carregar todos os usuários para encontrar o usuário fd-service**
* Duplo clique no usuário fd-service para abrir a janela de configurações do usuário
* Clique em &quot;Adicionar chave privada do arquivo de armazenamento de chaves&quot;.Especifique o alias e a senha específicos do seu certificado
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Salvar suas alterações

## Criando Serviço OSGI

Você pode gravar seu próprio pacote OSGi e usar o AEM Forms Client SDK para implementar um serviço para certificar documentos PDF. Os links a seguir seriam úteis para escrever seu próprio pacote OSGi

* [Criando seu primeiro pacote OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Usar a API de serviço de Documento](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Ou você pode usar o conjunto de amostras incluído como parte dos ativos do tutorial.

>[!NOTE]
>
>O pacote de amostra usa alias chamado &quot;ares&quot; para certificar os documentos. Portanto, certifique-se de que o seu alias se chama &quot;ares&quot; ao utilizar este pacote

## Teste da amostra no sistema local

* Baixe e instale o [Pacote personalizado de serviços de Documento](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Baixe e instale [Desenvolvimento com o Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Verifique se você adicionou a seguinte entrada ao serviço Mapeador de Usuário do Apache Sling Service](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresouresolver=fd-** serviceconforme mostrado na captura de tela abaixo
   ![Mapeador de usuário](assets/user-mapper-service.PNG)
* [Importar formulário adaptável de amostra](assets/certify-pdf-af.zip)
* [Importe e instale o envio personalizado](assets/custom-submit-certify.zip)
* [Abra o formulário adaptativo](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Carregar documento PDF que precisa ser certificado
   **opcional**  - Especifique o campo de assinatura que deseja usar na certificação do documento
* Clique em enviar.
* O PDF certificado deve ser devolvido a você.


