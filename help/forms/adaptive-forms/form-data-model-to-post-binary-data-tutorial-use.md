---
title: Utilização Do Modelo De Dados De Formulário Para Publicar Dados Binários
description: Publicação de dados binários no DAM do AEM usando o modelo de dados de formulário
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 106
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Utilização Do Modelo De Dados De Formulário Para Publicar Dados Binários{#using-form-data-model-to-post-binary-data}

A partir do AEM Forms 6.4, agora podemos chamar o Serviço de modelo de dados de formulário como uma etapa no fluxo de trabalho do AEM. Este artigo o guiará por um caso de uso de exemplo para publicar um Documento de registro usando o Serviço de modelo de dados de formulário.

O caso de uso é o seguinte:

1. Um usuário preenche e envia o Formulário adaptável.
1. O formulário adaptável é configurado para gerar o Documento de registro.
1. No envio desses formulários adaptáveis, o fluxo de trabalho do AEM é acionado, e ele usará o serviço de invocar modelo de dados de formulário para POST do documento de registro para o AEM DAM.

![posttodam](assets/posttodamshot1.png)

Guia Modelo de dados de formulário - Propriedades

Na guia Entrada de serviço, mapeamos o seguinte

* file(O objeto binário que precisa ser armazenado) com a propriedade DOR.pdf em relação à carga útil. Isso significa que, quando o Formulário adaptável é enviado, o Documento de registro gerado é armazenado em um arquivo chamado DOR.pdf em relação à carga do fluxo de trabalho.**Verifique se esse DOR.pdf é o mesmo que você fornece ao configurar a propriedade de envio do Formulário adaptável.**

* fileName - Esse é o nome pelo qual o objeto binário é armazenado no DAM. Assim, você deseja que essa propriedade seja gerada dinamicamente, para que cada fileName seja exclusivo por envio. Com essa finalidade, usamos a etapa do processo no fluxo de trabalho para criar a propriedade de metadados chamada filename e definimos seu valor para a combinação de Nome do membro e Número da conta da pessoa que está enviando o formulário. Por exemplo, se o nome do membro da pessoa for John Jacobs e seu número de conta for 9846, o nome do arquivo será John Jacobs_9846.pdf

![fdmserviceinput](assets/fdminputservice.png)

Entrada do serviço

>[!NOTE]
>
>Dicas de solução de problemas - Se, por algum motivo, o DOR.pdf não for criado no DAM, redefina as configurações de autenticação da fonte de dados clicando em [aqui](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). Essas são as configurações de autenticação do AEM, que por padrão são admin/admin.

Para testar esse recurso no servidor, siga as etapas mencionadas abaixo:

1.[Implantar o pacote Developingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [Baixe e implante o pacote setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Esse pacote OSGI personalizado é usado para criar a propriedade de metadados e definir seu valor a partir dos dados de formulário enviados.

1. [Importar os ativos](assets/postdortodam.zip) associado a este artigo no AEM usando o gerenciador de pacotes. Você obterá o seguinte

   1. Modelo de fluxo de trabalho
   1. Formulário adaptável configurado para enviar ao fluxo de trabalho do AEM
   1. Fonte de dados configurada para usar o arquivo PostToDam.JSON
   1. Modelo de dados do formulário que usa a fonte de dados

1. Aponte seu [navegador para abrir o Formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. Preencha o formulário e envie.
1. Verifique o aplicativo Assets se o documento de registro for criado e armazenado.


[Arquivo Swagger](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) usado na criação da fonte de dados está disponível para sua referência
