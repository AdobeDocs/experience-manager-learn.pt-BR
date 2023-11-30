---
title: Verificar usuários com OTP
description: Verifique o número do celular associado ao número do aplicativo usando OTP.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6594
thumbnail: 6594.jpg
topic: Development
role: Developer
level: Experienced
exl-id: d486d5de-efd9-4dd3-9d9c-1bef510c6073
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 0%

---

# Verificar usuários com OTP

A Autenticação de dois fatores do SMS (Autenticação de dois fatores) é um procedimento de verificação de segurança, que é acionado por meio de um usuário que faz logon em um site, software ou aplicativo. No processo de logon, o usuário recebe automaticamente um SMS para seu número de celular contendo um código numérico exclusivo.

Há várias organizações que fornecem esse serviço e, desde que tenham APIs REST bem documentadas, é possível integrar facilmente o AEM Forms usando os recursos de integração de dados do AEM Forms. Para o propósito deste tutorial, usei [Nexmo](https://developer.nexmo.com/verify/overview) para demonstrar o caso de uso de SMS 2FA.

As etapas a seguir foram seguidas para implementar o SMS 2FA com o AEM Forms usando o serviço Nexmo Verify.

## Criar conta de desenvolvedor

Crie uma conta de desenvolvedor com [Nexmo](https://dashboard.nexmo.com/sign-in). Anote a chave da API e a chave secreta da API. Essas chaves são necessárias para chamar as APIs REST do serviço do Nexmo.

## Criar arquivo Swagger/OpenAPI

A Especificação de OpenAPI (antiga Especificação do Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a API, incluindo:

* Pontos de extremidade disponíveis (/users) e operações em cada ponto de extremidade (GET /users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser escritas em YAML ou JSON. O formato é fácil de aprender e legível tanto para seres humanos quanto para máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga o [Documentação da OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms é compatível com a especificação OpenAPI versão 2.0 (fka Swagger).

Use o [editor swagger](https://editor.swagger.io/) para criar seu arquivo swagger para descrever as operações que enviam e verificam o código OTP enviado usando SMS. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo Swagger completo pode ser baixado de [aqui](assets/two-factore-authentication-swagger.zip)

## Criar fonte de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [Fonte de dados baseada em REST usando o arquivo swagger](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem. A fonte de dados preenchida é fornecida como parte dos ativos deste curso.

## Criar modelo de dados do formulário

A integração de dados do AEM Forms fornece uma interface intuitiva para criar e trabalhar com [modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Um modelo de dados de formulário depende de fontes de dados para o intercâmbio de dados.
O modelo de dados do formulário preenchido pode ser [baixado aqui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

[Criar o formulário principal](./create-the-main-adaptive-form.md)
