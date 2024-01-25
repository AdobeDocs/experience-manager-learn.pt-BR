---
title: Implantar a amostra
description: Execute o caso de uso em sua instância local do AEM Forms
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 87
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---

# Implantar a amostra

Para fazer com que esse caso de uso funcione em seu sistema, siga as seguintes instruções:

>[!NOTE]
>Presume-se que você esteja executando o AEM Forms na porta 4502.


## Criar banco de dados

Esta amostra usa o banco de dados MySQL para armazenar os dados de formulário adaptáveis. Será necessário criar o [esquema de banco de dados importando o arquivo de esquema](assets/data-base-schema.sql) no MySQL workbench.

## Criar fonte de dados

Você precisa criar uma fonte de dados agrupada da conexão Apache Sling chamada **ArmazenarExecutarDadosAf** apontando para o schema de banco de dados criado na etapa anterior. O código no pacote OSGi usa esse nome de fonte de dados.

## Criar modelo de dados do formulário

O modelo de dados de formulário precisa ser criado com base nessa fonte de dados chamada **ArmazenarExecutarDadosAf**. Este modelo de dados de formulário é usado para buscar o número do telefone celular associado à ID do aplicativo. O modelo de dados de formulário pode ser [baixado aqui.](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Criar conta de desenvolvedor com nexmo

Crie uma conta de desenvolvedor com [Nexmo](https://dashboard.nexmo.com/) para enviar e verificar códigos OTP. Anote a chave da API e a chave secreta da API. A fonte de dados e o modelo de dados de formulário já foram criados para você com base nesse serviço e estão incluídos nos ativos mencionados na etapa anterior.

## Implante os seguintes pacotes OSGi

Implante o pacote que tem o [código para armazenar e buscar dados do banco de dados](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)
Baixe e descompacte o [developing withserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Implante o arquivo DevelopingWithServiceUser.jar usando o console da Web Felix.

## Implantar a biblioteca do cliente

O exemplo usa duas bibliotecas de clientes. Importar estes [bibliotecas de clientes](assets/store-af-with-attachments-client-lib.zip) no AEM.

## Importar o modelo de formulário adaptável personalizado

Os formulários de amostra usados nesta demonstração são baseados em um modelo personalizado. Importe o [modelo personalizado no AEM](assets/custom-template-with-page-component.zip)

## Importar os exemplos de formulários adaptáveis

Os 2 formulários que compõem essa amostra precisam ser importados para o AEM. Os formulários de amostra podem ser [baixado aqui](assets/sample-forms.zip)

Abra o [FormuláriodeMinhaConta](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) no modo de edição. Especifique os valores da Chave da API Vonage e do Segredo da API nos campos apropriados no formulário adaptável.

## Testar a solução

Visualize o [ArmazenarAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
Insira seu número de celular, incluindo o código do país, preencha seus detalhes de usuário e adicione alguns anexos. Clique no botão &quot;Salvar e sair&quot; para salvar o formulário adaptável e seus anexos


## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
