---
title: Autenticação de dois fatores do SMS
description: Adicione uma camada extra de segurança para ajudar a confirmar a identidade de um usuário quando ele quiser realizar determinadas atividades
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6317
translation-type: tm+mt
source-git-commit: 4c08b09f59be0eb6644aaec729807b92bc339e82
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 0%

---



# Verificar usuários usando seus números de telefone celular

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

Para integrar o AEM/AEM Forms a aplicativos de terceiros, precisamos [criar a fonte de dados](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem.

## Criar modelo de dados do formulário

A integração de dados da AEM Forms fornece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Um modelo de dados de formulário depende de fontes de dados para troca de dados.
O modelo de dados de formulário concluído pode ser [baixado daqui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Criar formulário adaptável

Integre as invocações POST do Modelo de dados de formulário ao formulário adaptável para verificar o número de telefone celular inserido pelo usuário no formulário. Você pode criar seu próprio formulário adaptável e usar a chamada de POST do modelo de dados de formulário para enviar e verificar o código OTP de acordo com seus requisitos.

Se você quiser usar os ativos de amostra com suas chaves de API, siga as seguintes etapas:

* [Baixe o ](assets/sms-2fa-fdm.zip) modelo de dados de formulário e importe para AEM usando o gerenciador de  [pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* O download do formulário adaptável de amostra pode ser [baixado daqui](assets/sms-2fa-verification-af.zip). Este formulário de amostra usa as invocações de serviço do modelo de dados de formulário fornecido como parte deste artigo.
* Importe o formulário para o AEM a partir da interface de usuário do Forms e Documento [](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra o formulário no modo de edição. Abrir o editor de regras para o seguinte campo

![sms-send](assets/check-sms.PNG)

* Edite a regra associada ao campo. Forneça suas chaves de API apropriadas
* Salvar o formulário
* [Pré-visualização do ](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) formulário e teste a funcionalidade


