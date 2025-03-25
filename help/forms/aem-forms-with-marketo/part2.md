---
title: AEM Forms com Marketo (Parte 2)
description: Tutorial para integrar o AEM Forms com o Marketo usando o Modelo de dados do formulário do AEM Forms.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
duration: 137
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '211'
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
