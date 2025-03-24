---
title: Configurar a API de comunicação do AEM Forms
description: Configurar APIs de comunicação do AEM Forms com base em OpenAPI para autenticação de servidor para servidor
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Configurar APIs de comunicação do AEM Forms com base em OpenAPI no AEM Forms as a Cloud Service

## Pré-requisitos

* Última instância do AEM Forms as a Cloud Service.
* Todos os [perfis de produto necessários são adicionados ao ambiente.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)

* Ative o acesso da API do AEM ao perfil do produto, conforme mostrado abaixo
  ![perfil_do_produto1](assets/product-profiles1.png)
  ![perfil_do_produto](assets/product-profiles.png)

## Criar projeto do Adobe Developer Console

Faça logon no [Adobe Developer Console](https://developer.adobe.com/console/) usando sua Adobe ID.
Crie um novo projeto clicando no ícone apropriado
![novo-projeto](assets/new-project.png)

Dê um nome significativo ao projeto e clique no ícone Adicionar API
![novo-projeto](assets/new-project2.png)

Selecionar Experience Cloud
![novo-projeto3](assets/new-project3.png)
Selecione API de comunicações do AEM Forms e clique em Avançar
![novo-projeto4](assets/new-project4.png)

Verifique se você selecionou a autenticação de servidor para servidor e clique em Avançar
![novo-projeto5](assets/new-project5.png)
Selecione os perfis e clique no botão Salvar API configurada para salvar suas configurações
![novo-projeto6](assets/new-project6.png)
Clique no campo Servidor para servidor do OAuth
![novo-projeto7](assets/new-project7.png)
Copie a ID do cliente, o segredo do cliente e os escopos
![novo-projeto8](assets/new-project8.png)

## Configurar instância do AEM para habilitar a comunicação do Projeto ADC

Se você já tiver um projeto do AEM Forms, [siga estas instruções](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) para habilitar a credencial de servidor para servidor OAuth do projeto do Adobe Developer Console para comunicação com a instância do AEM

Se você não tiver um projeto do AEM Forms, crie um [Projeto do AEM Forms seguindo esta documentação.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) e habilite a credencial ClientID do servidor para servidor OAuth do Adobe Developer Console Project para se comunicar com a instância do AEM [usando esta documentação.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## Próximas etapas

[Gerar token de acesso](./generate-access-token.md)
