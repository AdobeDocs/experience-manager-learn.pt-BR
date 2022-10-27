---
title: Integração com [!DNL ServiceNow]
description: Crie e exiba todos os incidentes usando o modelo de dados de formulário.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
exl-id: 93a177b0-7852-44da-89cc-836d127be4e7
last-substantial-update: 2022-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '288'
ht-degree: 2%

---

# Integrar o AEM Forms com o [!DNL ServiceNow]

Criar e exibir incidente em [!DNL ServiceNow] usando o Modelo de dados de formulário no AEM Forms.

## Pré-requisitos

* [!DNL ServiceNow] conta.
* Familiarizar com [criação de fontes de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiarizar com [Modelo de dados do formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Amostra de ativos

Os ativos de exemplo fornecidos com este artigo incluem o seguinte

* Configuração do serviço na nuvem
* Troque arquivos para criar um incidente e buscar todos os incidentes
* Modelo de dados de formulário com base nos arquivos do gerenciador
* Formulário adaptável para criar e listar [!DNL ServiceNow] incidentes

## Implante os ativos no seu servidor

* Baixe o [ativos de exemplo](assets/service-now.zip)
* Importe os ativos no AEM usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* O arquivo do swagger usado para essa integração está localizado na variável ```/conf/9957/settings/cloudconfigs/fdm``` pasta no repositório crx
* Edite o [Configuração do serviço em nuvem CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)para corresponder à sua instância ServiceNow.
* Edite o [Configuração do serviço em nuvem GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) para corresponder à sua instância ServiceNow. Você precisará alterar o host, o nome de usuário e a senha para corresponder às credenciais da instância do ServiceNow.

## Acessar credenciais de instância do ServiceNow

* Clique no seu perfil de usuário
   ![clicar no perfil do usuário](assets/snow-1.png)

* Clique em Gerenciar senha da instância
* Os detalhes da instância são mostrados conforme abaixo
   ![detalhes da instância](assets/snow-3.png)

## Testar a integração

* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Insira alguns valores no campo descrição e comentários e clique no botão Criar Incidente
* A ID de incidente do incidente recém-criado deve ser preenchida no campo de texto e a tabela abaixo deve listar todos os incidentes.
