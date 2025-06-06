---
title: Autenticação de dois fatores do SMS
description: Adicione uma camada extra de segurança para ajudar a confirmar a identidade de um usuário quando ele quiser executar determinadas atividades
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
last-substantial-update: 2021-07-07T00:00:00Z
duration: 115
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '557'
ht-degree: 0%

---

# Verificar usuários usando seus números de telefone celular

A Autenticação de dois fatores do SMS (Autenticação de dois fatores) é um procedimento de verificação de segurança, que é acionado por meio de um usuário que faz logon em um site, software ou aplicativo. No processo de logon, o usuário recebe automaticamente um SMS para seu número de celular contendo um código numérico exclusivo.

Há várias organizações que fornecem esse serviço e, desde que tenham APIs REST bem documentadas, é possível integrar facilmente o AEM Forms usando os recursos de integração de dados do AEM Forms. Para fins deste tutorial, usei o [Nexmo](https://developer.nexmo.com/verify/overview) para demonstrar o caso de uso de SMS 2FA.

As etapas a seguir foram seguidas para implementar o SMS 2FA com o AEM Forms usando o serviço Nexmo Verify.

## Criar conta de desenvolvedor

Crie uma conta de desenvolvedor com [Nexmo](https://dashboard.nexmo.com/sign-in). Anote a chave da API e a chave secreta da API. Essas chaves são necessárias para chamar as APIs REST do serviço do Nexmo.

## Criar arquivo Swagger/OpenAPI

A Especificação de OpenAPI (antiga Especificação do Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a API, incluindo:

* Pontos de extremidade disponíveis (/users) e operações em cada ponto de extremidade (GET /users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação
Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser escritas em YAML ou JSON. O formato é fácil de aprender e legível tanto para seres humanos quanto para máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga a [documentação sobre OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms é compatível com a especificação OpenAPI versão 2.0 (fka Swagger).

Use o [editor do swagger](https://editor.swagger.io/) para criar seu arquivo do swagger e descrever as operações que enviam e verificam o código OTP enviado por SMS. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo Swagger completo pode ser baixado de [aqui](assets/two-factore-authentication-swagger.zip)

## Criar Source de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [criar a fonte de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=pt-BR) na configuração dos serviços em nuvem.

## Criar modelo de dados do formulário

A integração de dados do AEM Forms fornece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=pt-BR). Um modelo de dados de formulário depende de fontes de dados para o intercâmbio de dados.
O modelo de dados do formulário preenchido pode ser [baixado daqui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Criar formulário adaptável

Integre as invocações POST do Modelo de dados de formulário ao seu formulário adaptável para verificar o número de telefone celular inserido pelo usuário no formulário. Você pode criar seu próprio formulário adaptável e usar a invocação POST do modelo de dados de formulário para enviar e verificar o código OTP de acordo com seus requisitos.

Se quiser usar os ativos de amostra com suas chaves de API, siga as seguintes etapas:

* [Baixe o modelo de dados de formulário](assets/sms-2fa-fdm.zip) e importe para o AEM usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* Baixe o formulário adaptável de exemplo pode ser [baixado daqui](assets/sms-2fa-verification-af.zip). Este formulário de amostra usa as invocações de serviço do modelo de dados de formulário fornecido como parte deste artigo.
* Importar o formulário para o AEM da [Forms e da Interface do Usuário do Documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra o formulário no modo de edição. Abra o editor de regras do seguinte campo

![sms-send](assets/check-sms.PNG)

* Edite a regra associada ao campo. Forneça as chaves de API apropriadas
* Salve o formulário
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) e testar a funcionalidade
