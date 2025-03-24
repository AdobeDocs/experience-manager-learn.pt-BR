---
title: Integrar o AEM Forms as a Cloud Service e o Marketo (Parte 2)
description: Saiba como integrar o AEM Forms e o Marketo usando o Modelo de dados de formulário do AEM Forms.
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 75e589fa-f7fc-4d0b-98c8-ce4d603ef2f7
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 3%

---

# Criar Source de dados

As REST APIs do Marketo são autenticadas com OAuth 2.0 de duas pernas. Podemos criar facilmente uma fonte de dados usando o arquivo swagger baixado na etapa anterior

## Criar contêiner de configuração

* Faça logon no AEM.
* Clique no menu Ferramentas e, em seguida, em **Navegador de Configuração**, conforme mostrado abaixo

* ![menu de ferramentas](assets/datasource3.png)

* Clique em **Criar** e forneça um nome significativo, conforme mostrado abaixo. Selecione a opção Configurações de nuvem como mostrado abaixo

* ![contêiner de configuração](assets/datasource4.png)

## Criar serviços em nuvem

* Navegue até o menu Ferramentas e clique em serviços em nuvem -> Fontes de dados

* ![serviços na nuvem](assets/datasource5.png)

* Selecione o contêiner de configuração criado na etapa anterior e clique em **Criar** para criar uma nova fonte de dados.Forneça um nome significativo e selecione o serviço RESTful na lista suspensa Tipo de serviço e clique em **Avançar**
* ![nova-fonte-de-dados](assets/datasource6.png)

* Faça upload do arquivo swagger e especifique o tipo de concessão, a ID do cliente, o segredo do cliente e o URL do token de acesso específicos para sua instância do Marketo, como mostrado na captura de tela abaixo.

* Teste a conexão e, se a conexão for bem-sucedida, clique no botão azul **Criar** para concluir o processo de criação da fonte de dados.

* ![data-source-config](assets/datasource1.png)


## Próximas etapas

[Criar modelo de dados do formulário](./part3.md)
