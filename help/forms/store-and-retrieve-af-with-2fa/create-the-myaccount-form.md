---
title: Criar o formulário MyAccount
description: Crie o formulário myaccount para recuperar o formulário parcialmente preenchido na verificação bem-sucedida da ID do aplicativo e do número de telefone.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Criar o formulário MyAccount

O formulário **MyAccountForm** é usado para recuperar o formulário adaptável parcialmente preenchido depois que o usuário verifica a ID do aplicativo e o número de celular associado à ID do aplicativo.

![formulário da minha conta](assets/6599.JPG)

Quando o usuário insere a ID do aplicativo e clica no botão **FetchApplication**, o número de celular associado à ID do aplicativo é obtido do banco de dados usando a operação Get do modelo de dados de formulário.

Este formulário usa a invocação POST do Modelo de dados de formulário para verificar o número de celular usando OTP. A ação de envio do formulário é acionada na verificação bem-sucedida do número de celular usando o código a seguir. Estamos acionando o evento de clique do botão de envio chamado **submitForm**.

>[!NOTE]
> Será necessário fornecer a Chave da API e os valores do Segredo da API específicos da sua conta [Nexmo](https://dashboard.nexmo.com/) nos campos apropriados do Formulário da Minha Conta

![acionador-envio](assets/trigger-submit.JPG)



Este formulário está associado à ação de envio personalizada que encaminha o envio do formulário para o servlet montado em **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

O código no servlet montado em **/bin/renderaf** encaminha a solicitação para renderizar o formulário adaptável storeafwithattachments pré-preenchido com os dados salvos.


* O Formulário de Minha Conta pode ser [baixado daqui](assets/my-account-form.zip)

* Os formulários de exemplo são baseados no [modelo de formulário adaptável personalizado](assets/custom-template-with-page-component.zip) que precisa ser importado para o AEM para que os formulários de exemplo sejam renderizados corretamente.

* [O manipulador de envio personalizado](assets/custom-submit-my-account-form.zip) associado ao envio de MyAccountForm precisa ser importado para o AEM.

## Próximas etapas

[Testar a solução implantando os ativos de amostra](./deploy-this-sample.md)
