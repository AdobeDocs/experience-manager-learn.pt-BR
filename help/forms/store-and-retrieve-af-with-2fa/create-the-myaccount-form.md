---
title: Criar o formulário MyAccount
description: Crie o formulário myaccount para recuperar o formulário parcialmente preenchido na verificação bem-sucedida do ID do aplicativo e do número de telefone.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Criar o formulário MyAccount

O formulário **MyAccountForm** é usado para recuperar o formulário adaptável parcialmente preenchido depois que o usuário verificou a ID do aplicativo e o número do celular associado à ID do aplicativo.

![meu formulário de conta](assets/6599.JPG)

Quando o usuário insere a ID do aplicativo e clica no botão **FetchApplication** , o número de celular associado à id do aplicativo é obtido do banco de dados usando a operação Get do modelo de dados de formulário.

Este formulário usa a invocação POST do Modelo de Dados de Formulário para verificar o número de celular usando OTP. A ação de envio do formulário é acionada na verificação bem-sucedida do número do celular usando o código a seguir. Estamos acionando o evento click do botão Enviar chamado **submitForm**.

>[!NOTE]
> Você precisará fornecer a chave da API e os valores do segredo da API específicos para a sua [Nexmo](https://dashboard.nexmo.com/) nos campos apropriados de MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Este formulário está associado à ação de envio personalizado que encaminha o envio do formulário para o servlet montado em **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

O código no servlet montado em **/bin/renderaf** O encaminha a solicitação para renderizar o formulário adaptável storeafwithattachment com os dados salvos.


* O MyAccountForm pode ser [baixado aqui](assets/my-account-form.zip)

* Os formulários de amostra são baseados em [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisam ser importadas para AEM para que os formulários de amostra sejam renderizados corretamente.

* [Manipulador de envio personalizado](assets/custom-submit-my-account-form.zip) associado ao envio de MyAccountForm precisa ser importado para o AEM.

## Próximas etapas

[Teste a solução implantando os ativos de amostra](./deploy-this-sample.md)
