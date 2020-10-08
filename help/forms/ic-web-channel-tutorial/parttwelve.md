---
title: Configuração do delivery do documento do canal da Web
seo-title: Configuração do delivery do documento do canal da Web
description: Esta é a parte final de um tutorial de várias etapas para criar seu primeiro documento de comunicação interativo. Nesta parte, olhamos para o delivery do documento de canais da Web por email.
seo-description: Esta é a parte final de um tutorial de várias etapas para criar seu primeiro documento de comunicação interativo. Nesta parte, olhamos para o delivery do documento de canais da Web por email.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Configuração do delivery do documento do canal da Web {#setting-up-the-delivery-of-web-channel-document}


Nesta parte, olhamos para o delivery do documento de canais da Web por email.

Depois de definir e testar o documento de comunicação interativa do canal da Web, você precisará de um mecanismo de delivery para fornecer o documento do canal da Web ao recipient.

Para podermos usar o e-mail como um mecanismo de delivery para nosso documento de canal da Web, precisamos fazer uma pequena alteração no modelo de dados do formulário.

[Para saber mais sobre o delivery de canais da Web por email](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Faça logon no AEM Forms.

* Navegue até Forms ->Integrações de dados

* Abra o Modelo de dados RetiordineAccountStatement no modo de edição.

* Selecione o objeto de saldos e clique no botão editar.

* Selecione o ícone &quot;lápis&quot; para abrir o argumento id no modo de edição.

* Altere o vínculo para &quot;RequestAttribute&quot;.

* Defina o número da conta no valor de vínculo, como mostrado abaixo.

* Desta forma, estamos transmitindo o número da conta pelo atributo de solicitação para o modelo de dados do formulário

* Certifique-se de salvar suas alterações.
   ![fdm](assets/requestattribute.gif)

## Testar Delivery de e-mail do Documento de Canal da Web {#test-email-delivery-of-web-channel-document}

* [Instale os ativos de amostra usando o gerenciador de pacotes](assets/webchanneldelivery.zip)
* [Logon no crx](http://localhost:4502/crx/de/index.jsp#)

* Navegue até /home/users

* Procure o usuário administrador no nó do usuário.

* Selecione o nó do perfil do usuário administrador.

* Crie uma propriedade chamada &quot;accountnumber&quot;. Verifique se o tipo de propriedade é uma string.

* Defina o valor dessa propriedade account number como &quot;3059827&quot;. Você pode definir esse valor para qualquer número aleatório, conforme desejar.

* [Abrir getad.html](http://localhost:4502/content/getad.html)

* O código associado a esse URL obterá o número da conta do usuário conectado. Esse número de conta é então passado como requestattribute para o FDM. O FDM buscará os dados associados a esse número de conta e preencherá o documento do canal da Web.

>[!NOTE]
>
>Consulte o arquivo **/apps/AEMForms/fetchad/GET.jsp** no crx. Verifique se a variável String webChannelDocument está apontando para um caminho de documento de comunicação válido.
