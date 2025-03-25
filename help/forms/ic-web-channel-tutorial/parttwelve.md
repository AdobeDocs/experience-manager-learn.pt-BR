---
title: Configuração da entrega do documento de canal da Web
description: Esta é a parte final de um tutorial em várias etapas para criar seu primeiro documento de comunicações interativas. Nesta parte, analisamos a entrega de documentos de canal da Web por email.
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
duration: 68
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Configuração da entrega do documento de canal da Web {#setting-up-the-delivery-of-web-channel-document}


Nesta parte, analisamos a entrega de documentos de canal da Web por email.

Depois de definir e testar o documento de comunicação interativa do canal da Web, é necessário um mecanismo de entrega para entregar o documento do canal da Web ao destinatário.

Para poder usar o email como um mecanismo de entrega para nosso documento de canal da Web, precisamos fazer uma pequena alteração no modelo de dados de formulário.

[Para saber mais sobre a entrega de canal da Web por email](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Faça logon no AEM Forms.

* Navegue até Forms ->Integrações de dados

* Abra o Modelo de Dados RetirementAccountStatement no modo de edição.

* Selecione o objeto de saldos e clique no botão de edição.

* Selecione o ícone &quot;lápis&quot; para abrir o argumento id no modo de edição.

* Altere a associação para &quot;RequestAttribute&quot;.

* Defina o número da conta no valor de vinculação como mostrado abaixo.

* Dessa forma, estamos transmitindo o número da conta pelo atributo de solicitação para o modelo de dados de formulário

* Salve as alterações.
  ![fdm](assets/requestattribute.gif)

## Testar entrega de email do documento de canal da Web {#test-email-delivery-of-web-channel-document}

* [Instalar os ativos de amostra usando o gerenciador de pacotes](assets/webchanneldelivery.zip)
* [Logon no crx](http://localhost:4502/crx/de/index.jsp#)

* Navegue até /home/users

* Procure o usuário administrador no nó do usuário.

* Selecione o nó de perfil do usuário administrador.

* Crie uma propriedade chamada &quot;accountnumber&quot;. Verifique se o tipo de propriedade é uma cadeia de caracteres.

* Defina o valor dessa propriedade accountnumber como &quot;3059827&quot;. Você pode definir esse valor para qualquer número aleatório que desejar.

* [Abrir getad.html](http://localhost:4502/content/getad.html)

* O código associado a este URL obterá o número da conta do usuário conectado. Esse número de conta é passado como requestattribute para o FDM. O FDM buscará os dados associados a esse número de conta e preencherá o documento de canal da Web.

>[!NOTE]
>
>Consulte o arquivo **/apps/AEMForms/fetchad/GET.jsp** no crx. Verifique se a variável de cadeia de caracteres webChannelDocument aponta para um caminho de documento de comunicação válido.

## Próximas etapas

[Configurar entrega de email](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)