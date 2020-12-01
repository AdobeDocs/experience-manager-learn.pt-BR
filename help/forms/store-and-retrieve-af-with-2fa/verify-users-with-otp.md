---
title: Verificar usuários com OTP
description: Verifique o número do dispositivo móvel associado ao número do aplicativo usando OTP.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6594
thumbnail: 6594.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---



# Verificar usuários com OTP

A Autenticação de Dois Fatores do SMS (Autenticação de Fator Duplo) é um procedimento de verificação de segurança, acionado por meio de um usuário que faz logon em um site, software ou aplicativo. No processo de logon, o usuário é automaticamente enviado um SMS para seu número móvel contendo um código numérico exclusivo.

Há várias organizações que fornecem esse serviço e, desde que tenham APIs REST bem documentadas, você pode integrar facilmente a AEM Forms usando os recursos de integração de dados da AEM Forms. Para a finalidade deste tutorial, usei [Nexmo](https://developer.nexmo.com/verify/overview) para demonstrar o caso de uso do SMS 2FA.

As etapas a seguir foram seguidas para implementar o SMS 2FA com a AEM Forms usando o serviço Nexmo Verify.

## Criar conta de desenvolvedor

Crie uma conta de desenvolvedor com [Nexmo](https://dashboard.nexmo.com/sign-in). Anote a chave da API e a chave secreta da API. Essas teclas serão necessárias para chamar REST APIs do serviço do Nexmo.

## Criar arquivo Swagger/OpenAPI

A especificação OpenAPI (antiga especificação Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite que você descreva toda a sua API, incluindo:

* Pontos de extremidade (/users) e operações disponíveis em cada ponto de extremidade (GET /usuários, POST /usuários)
* Parâmetros de operação Entrada e saída para cada operação
Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser escritas em YAML ou JSON. O formato é fácil de aprender e legível para humanos e máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga a [documentação do OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> A AEM Forms suporta a especificação OpenAPI versão 2.0 (fka Swagger).

Use o [editor de swagger](https://editor.swagger.io/) para criar seu arquivo swagger para descrever as operações que enviam e verificam o código OTP enviado por SMS. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo swagger concluído pode ser baixado de [here](assets/two-factore-authentication-swagger.zip)

## Criar fonte de dados

Para integrar o AEM/AEM Forms a aplicativos de terceiros, precisamos [REST com base na fonte de dados usando o arquivo swagger](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração de serviços em nuvem. A fonte de dados completa é fornecida a você como parte dos ativos do curso.

## Criar modelo de dados do formulário

A integração de dados da AEM Forms fornece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Um modelo de dados de formulário depende de fontes de dados para troca de dados.
O modelo de dados de formulário concluído pode ser [baixado daqui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)
