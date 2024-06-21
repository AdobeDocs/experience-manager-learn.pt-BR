---
title: Criando componente de endereço
description: Criação do novo componente principal de endereço no AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# Criar componente de endereço

Faça logon no CRXDE da instância do AEM Forms pronta para nuvem local.

Faça uma cópia do ``/apps/bankingapplication/components/adaptiveForm/button`` e renomeie-o para addressblock. Selecione o nó addressblock e defina suas propriedades conforme mostrado abaixo.

>[!NOTE]
>
> ``bankingapplication`` é a appId fornecida ao criar o projeto Maven. Esta appId pode ser diferente no seu ambiente. Você pode fazer uma cópia de qualquer componente; por acaso, fiz uma cópia do componente de botão


![address-bloc](assets/address-properties.png)

## propriedades do nó cq-template

Selecione o ``cq-template`` sob o nó ``addressblock`` e defina suas propriedades conforme mostrado abaixo. Observe que fieldType está definido como panel
![cq-template](assets/cq-template.png)

## Adicionar nós no cq-template

Adicione os seguintes nós do tipo ``nt:unstructured`` em ``cq-template``

* streetaddress
* cidade
* zip
* estado

Esses nós representam os campos do componente de bloco de endereço. Os campos streetaddress, city e zip serão um campo de entrada de texto e o campo state será um campo suspenso.

## Definir as propriedades do nó streetaddress

>[!NOTE]
>
> A variável **_bankingapplication_** no caminho se refere ao appId do projeto maven. Pode ser diferente no seu ambiente

Selecione o ``streetaddress`` e defina suas propriedades conforme mostrado abaixo.
![street-address](assets/streetaddress.png)

## Definir as propriedades do nó city

Selecione o ``city`` e defina suas propriedades conforme mostrado abaixo.
![city](assets/city.png)

## Definir as propriedades do nó zip

Selecione o ``zip`` e defina suas propriedades conforme mostrado abaixo.
![zip](assets/zip.png)

## Definir as propriedades do nó de estado

Selecione o ``state`` e defina suas propriedades conforme mostrado abaixo. Observe o fieldType do estado - ele está definido como uma lista suspensa
![state](assets/state.png)

O componente de bloco de endereço final terá esta aparência

![final-address](assets/crx-address-block.png)

## Próximas etapas

[Implantar o projeto](./deploy-your-project.md)




