---
title: Implantar a amostra
description: Obter caso de uso em execução na instância local do AEM Forms
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 2%

---



# Implantar a amostra

Para que este caso de uso funcione em seu sistema, siga as seguintes instruções:

>[!NOTE]
>Pressupõe-se que você esteja executando o AEM Forms na porta 4502.


## Criar banco de dados

Este exemplo usa o banco de dados MySQL para armazenar os dados do formulário adaptável. Você precisará criar o schema de banco de dados [importando o arquivo de esquema](assets/data-base-schema.sql) para o Workbench MySQL.

## Criar fonte de dados

Você precisa criar uma fonte de dados chamada **StoreAndRetrieveAfData**. O código no pacote OSGi usa esse nome de fonte de dados

## Criar modelo de dados do formulário

O Modelo de Dados de Formulário precisa ser criado com base nessa fonte de dados chamada **StoreAndRetrieveAfData**. Este modelo de dados de formulário é usado para buscar o número do telefone celular associado à ID do aplicativo. O modelo de dados de formulário pode ser [baixado aqui.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Criar conta do desenvolvedor com o nó

Crie uma conta de desenvolvedor com [Nexmo](https://dashboard.nexmo.com/) para enviar e verificar códigos OTP. Anote a Chave da API e a Chave secreta da API. A fonte de dados e o modelo de dados de formulário já foram criados para você em relação a esse serviço e estão incluídos com os ativos mencionados na etapa anterior.

## Implante os seguintes pacotes OSGi

Implante o pacote que tem o código [para armazenar e buscar dados do banco de dados](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
Implante o pacote [DevelopingWithServiceUser](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).

## Implantar a biblioteca do cliente

A amostra usa duas bibliotecas de clientes. Importe essas [bibliotecas do cliente](assets/client-libraries.zip) no AEM.

## Importar o modelo de formulário adaptável personalizado

Os formulários de amostra usados nessa demonstração são baseados em um modelo personalizado. Importe o modelo personalizado [para o AEM](assets/custom-template-with-page-component.zip)

## Importar formulários adaptáveis de amostra

Os 2 formulários que compõem essa amostra precisam ser importados para o AEM. Os formulários de amostra podem ser [baixados aqui](assets/sample-forms.zip)

Abra o [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) no modo de edição. Especifique os valores Chave da API e Segredo da API nos campos apropriados no formulário adaptável.

## Testar a solução

Visualize o [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Insira seu número de celular, incluindo o código do país , preencha os detalhes do usuário e adicione alguns anexos. Clique no botão &quot;Salvar e sair&quot; para salvar o formulário adaptável e seus anexos


## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
