---
title: Autenticação de dois fatores do SMS
description: Adicione uma camada extra de segurança para ajudar a confirmar a identidade de um usuário quando ele quiser executar determinadas atividades
feature: Adaptive Forms
version: 6.4,6.5
kt: 6317
topic: Development
role: Developer
level: Experienced
exl-id: c2c55406-6da6-42be-bcc0-f34426b3291a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 1%

---

# Verificar usuários usando seus números de telefone celular

A Autenticação de dois fatores do SMS (Autenticação de fator duplo) é um procedimento de verificação de segurança acionado por um usuário que faz logon em um site, software ou aplicativo. No processo de logon, o usuário recebe automaticamente um SMS para o número de celular que contém um código numérico exclusivo.

Há várias organizações fornecendo esse serviço e, desde que tenham APIs REST bem documentadas, você pode integrar facilmente o AEM Forms usando os recursos de integração de dados do AEM Forms. Para a finalidade deste tutorial, eu usei [Nexmo](https://developer.nexmo.com/verify/overview) para demonstrar o caso de uso do SMS 2FA.

As etapas a seguir foram seguidas para implementar o SMS 2FA com o AEM Forms usando o serviço Nexmo Verify.

## Criar conta do desenvolvedor

Crie uma conta de desenvolvedor com [Nexmo](https://dashboard.nexmo.com/sign-in). Anote a Chave da API e a Chave secreta da API. Essas chaves são necessárias para invocar APIs REST do serviço do Nexmo.

## Criar arquivo Swagger/OpenAPI

A Especificação OpenAPI (antiga Especificação Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a sua API, incluindo:

* Pontos de extremidade (/users) disponíveis e operações em cada ponto de extremidade (GET/users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser gravadas em YAML ou JSON. O formato é fácil de aprender e legível para seres humanos e máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga as [Documentação do OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms suporta a especificação OpenAPI versão 2.0 (Fka Swagger).

Use o [editor de swagger](https://editor.swagger.io/) para criar seu arquivo swagger para descrever as operações que enviam e verificam o código OTP enviado usando SMS. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo swagger concluído pode ser baixado de [here](assets/two-factore-authentication-swagger.zip)

## Criar fonte de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [criar fonte de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem.

## Criar modelo de dados do formulário

A integração de dados do AEM Forms oferece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Um modelo de dados de formulário depende de fontes de dados para troca de dados.
O modelo de dados de formulário preenchido pode ser [baixado aqui](assets/sms-2fa-fdm.zip)

![fdm](assets/2FA-fdm.PNG)

## Criar formulário adaptável

Integre as invocações POST do Modelo de Dados de Formulário a seu formulário adaptável para verificar o número de telefone celular inserido pelo usuário no formulário. Você pode criar seu próprio formulário adaptável e usar a invocação POST do modelo de dados de formulário para enviar e verificar o código OTP de acordo com seus requisitos.

Se quiser usar os ativos de exemplo com as chaves de API, siga as seguintes etapas:

* [Baixe o modelo de dados de formulário](assets/sms-2fa-fdm.zip) e importar para AEM usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* O download do formulário adaptável de amostra pode ser [baixado aqui](assets/sms-2fa-verification-af.zip). Esse formulário de amostra usa as invocações de serviço do modelo de dados de formulário fornecido como parte deste artigo.
* Importe o formulário no AEM a partir do [Interface do usuário do Forms e do documento](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra o formulário no modo de edição. Abra o editor de regras para o seguinte campo

![sms-send](assets/check-sms.PNG)

* Edite a regra associada ao campo . Forneça suas chaves de API apropriadas
* Salvar o formulário
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/sms-2fa-verification/jcr:content?wcmmode=disabled) e testar a funcionalidade
