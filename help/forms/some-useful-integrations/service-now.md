---
title: Integração com [!DNL ServiceNow]
description: Crie e exiba todos os incidentes usando o modelo de dados de formulário.
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
duration: 47
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# Integrar o AEM Forms com o [!DNL ServiceNow]

Criar e exibir incidente em [!DNL ServiceNow] usar o modelo de dados de formulário no AEM Forms.

## Pré-requisitos

* [!DNL ServiceNow] conta.
* Familiarizar com [criação de fontes de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiarizar com [Modelo de dados do formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Ativos de amostra

Os ativos de amostra fornecidos com este artigo incluem o seguinte

* Configuração do serviço em nuvem
* Arquivos do Swagger para criar um incidente e buscar todos os incidentes
* Modelo de dados de formulário com base nos arquivos swagger
* Formulário adaptável para criar e listar [!DNL ServiceNow] incidentes

## Implantar os ativos no servidor

* Baixe o [ativos de amostra](assets/service-now.zip)
* Importar os ativos para o AEM usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* O arquivo swagger usado para essa integração está localizado sob o ```/conf/9957/settings/cloudconfigs/fdm``` pasta no repositório crx
* Edite o [Criar configuração do serviço de nuvem Incidente](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)para corresponder à sua instância do ServiceNow.
* Edite o [Configuração do serviço de nuvem GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) para corresponder à sua instância do ServiceNow. Você precisará alterar o host, o nome de usuário e a senha para corresponder às credenciais da instância do ServiceNow.

## Acessar credenciais da instância ServiceNow

* Clique no seu perfil de usuário
  ![clique no perfil do usuário](assets/snow-1.png)

* Clique em Gerenciar senha da instância
* Os detalhes da instância são mostrados abaixo
  ![detalhes da instância](assets/snow-3.png)

## Testar a integração

* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Insira alguns valores no campo de descrição e comentários e clique no botão Criar incidente
* A ID do incidente recém-criado deve ser preenchida no campo de texto e a tabela abaixo deve listar todos os incidentes.
