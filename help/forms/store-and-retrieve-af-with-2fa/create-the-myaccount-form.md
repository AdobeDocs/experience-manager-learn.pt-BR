---
title: Criar o formulário MyAccount
description: Crie o formulário myaccount para recuperar o formulário parcialmente preenchido na verificação bem-sucedida do ID do aplicativo e do número de telefone.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 1%

---



# Criar o formulário MyAccount

O formulário **MyAccountForm** é usado para recuperar o formulário adaptável parcialmente preenchido depois que o usuário tiver verificado a ID do aplicativo e o número de celular associado à ID do aplicativo.

![meu formulário de conta](assets/6599.JPG)

Quando o usuário insere a ID do aplicativo e clica no botão **FetchApplication**, o número de celular associado à ID do aplicativo é buscado no banco de dados usando a operação Get do modelo de dados de formulário.

Este formulário usa a invocação POST do Modelo de Dados de Formulário para verificar o número de celular usando OTP. A ação de envio do formulário é acionada na verificação bem-sucedida do número do celular usando o código a seguir. Estamos acionando o evento click do botão de envio chamado **submitForm**.

>[!NOTE]
> Você precisará fornecer a Chave da API e os valores do Segredo da API específicos para sua conta [Nexmo](https://dashboard.nexmo.com/) nos campos apropriados de MyAccountForm

![trigger-submit](assets/trigger-submit.JPG)



Este formulário está associado à ação de envio personalizado que encaminha o envio do formulário para o servlet montado em **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

O código no servlet montado em **/bin/renderaf** encaminha a solicitação para renderizar o formulário adaptável storeafwithattachments pré-preenchido com os dados salvos.


* O MyAccountForm pode ser [baixado aqui](assets/my-account-form.zip)

* Os formulários de amostra são baseados no [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisa ser importado para o AEM para que os formulários de amostra sejam renderizados corretamente.

* [O ](assets/custom-submit-my-account-form.zip) manipulador de envio personalizado associado ao envio de MyAccountForm precisa ser importado para o AEM.
