---
title: Implantar a amostra no servidor local
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas na consulta de envios de formulários armazenados no portal do Azure
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Implantar a amostra no servidor local

Para que esse caso de uso funcione no servidor local, siga as etapas listadas abaixo. Presume-se que a instância do AEM esteja em execução no host local, porta 4502.

* [Instalar o pacote](assets/azuredemo.all-1.0.0-SNAPSHOT.zip) usando o gerenciador de pacotes.

* Forneça as credenciais do portal do Azure usando o OSGi configMgr
  ![azure-portal](assets/azure-portal-config.png)
Certifique-se de que o URI de armazenamento termine em uma barra e o token SAS comece com um ?
* Navegue até [AzureDemo](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/azuredemo)

* Edite as configurações de autenticação das 3 fontes de dados a seguir para corresponder ao seu ambiente
  ![fontes de dados](assets/fdm-data-sources.png)

* Visualizar e enviar [Formulário ContactUs](http://localhost:4502/content/dam/formsanddocuments/azureportal/contactus/jcr:content?wcmmode=disabled)

* [Consultar o envio do formulário](http://localhost:4502/content/dam/formsanddocuments/azureportal/queryformsubmissions/jcr:content?wcmmode=disabled)

