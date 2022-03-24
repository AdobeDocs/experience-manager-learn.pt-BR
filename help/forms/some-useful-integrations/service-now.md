---
title: Integração com Serviço Agora
description: Crie e exiba todos os incidentes usando o modelo de dados de formulário.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9957
topic: Development
role: Developer
level: Intermediate
source-git-commit: b7ff98dccc1381abe057a80b96268742d0a0629b
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 2%

---

# Integrar o AEM Forms ao servicenow

Crie e exiba o incidente no ServiceNow usando o Modelo de dados de formulário no AEM Forms.

## Pré-requisitos

* Conta ServiceNow.
* Familiarizar com [criação de fontes de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html)
* Familiarizar com [Modelo de dados do formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html)

## Amostra de ativos

Os ativos de exemplo fornecidos com este artigo incluem o seguinte
* Configuração do serviço na nuvem
* Troque arquivos para criar um incidente e buscar todos os incidentes
* Modelo de dados de formulário com base nos arquivos do gerenciador
* Formulário adaptável para criar e listar incidentes de linha de serviço

## Implante os ativos no seu servidor

* Baixe o [ativos de exemplo](assets/service-now.zip)
* Importe os ativos no AEM usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Edite o [Configuração do serviço em nuvem CreateIncident](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fcreateincident)para corresponder à sua instância ServiceNow.
* Edite o [Configuração do serviço em nuvem GetAllIncidents](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2F9957%2Fsettings%2Fcloudconfigs%2Ffdm%2Fgetallincidents) para corresponder à sua instância ServiceNow


## Testar a integração

* [Abra o formulário adaptável](http://localhost:4502/content/dam/formsanddocuments/create-incident-in-service-now/jcr:content?wcmmode=disabled)
* Insira alguns valores no campo descrição e comentários e clique no botão Criar Incidente
* A ID de incidente do incidente recém-criado deve ser preenchida no campo de texto e a tabela abaixo deve listar todos os incidentes.

