---
title: Configuração do modelo de dados do formulário
description: Criar modelo de dados de formulário com base na fonte de dados RDBMS
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 1%

---

# Configuração do modelo de dados do formulário

## Fonte de dados agrupada da conexão Apache Sling

A primeira etapa na criação do modelo de dados de formulário RDBMS é configurar a fonte de dados agrupada da conexão Apache Sling. Para configurar a fonte de dados, siga as etapas listadas abaixo:

* Aponte seu navegador para [configMgr](http://localhost:4502/system/console/configMgr)
* Pesquisar por **Fonte de dados agrupada da conexão Apache Sling**
* Adicione uma nova entrada e forneça os valores, como mostrado na captura de tela.
* ![fonte de dados](assets/data-source.png)
* Salve as alterações

>[!NOTE]
>O URI da conexão JDBC, o nome de usuário e a senha serão alterados dependendo da configuração do banco de dados MySQL.


## Criação do modelo de dados do formulário

* Aponte seu navegador para [Integrações de dados](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Clique em _Criar_->_Modelo de dados do formulário_
* Forneça nome e título significativos para o modelo de dados de formulário, como **Funcionário**
* Clique em _Avançar_
* Selecione a fonte de dados criada na seção anterior (fóruns)
* Clique em _Criar_->Editar para abrir o modelo de dados de formulário recém-criado no modo de edição
* Expanda a _fóruns_ para ver o esquema de funcionário. Expanda o nó employee para ver as 2 tabelas

## Adicionar entidades ao seu modelo

* Verifique se o nó do funcionário está expandido
* Selecione as novas entidades de recebimento e beneficiários e clique em _Adicionar selecionado_

## Adicionar Serviço de Leitura a uma nova entidade

* Selecionar nova entidade
* Clique em _Editar propriedades_
* Selecione get na lista suspensa Serviço de leitura
* Clique no ícone + para adicionar parâmetro ao serviço get
* Especifique os valores conforme mostrado na captura de tela
* ![get-service](assets/get-service.png)
>[!NOTE]
> O serviço get espera um valor mapeado para a coluna empID da entidade nova. Há várias maneiras de transmitir esse valor e, neste tutorial, a empID é passada pelo parâmetro de solicitação chamado empID.
* Clique em _Concluído_ para salvar os argumentos do serviço get
* Clique em _Concluído_ para salvar as alterações no modelo de dados de formulário

## Adicionar associação entre 2 entidades

As associações definidas entre entidades de banco de dados não são criadas automaticamente no modelo de dados de formulário. As associações entre entidades precisam ser definidas usando o editor de modelo de dados de formulário. Cada entidade nova pode ter um ou mais beneficiários, precisamos definir uma associação um para muitos entre as entidades nova e beneficiária.
As etapas a seguir guiarão você pelo processo de criação da associação um para muitos

* Selecione uma nova entidade e clique em _Adicionar associação_
* Forneça um Título e Identificador significativos para a associação e outras propriedades, conforme mostrado na captura de tela abaixo
  ![associação](assets/association-entities-1.png)

* Clique no link _editar_ ícone na seção Argumentos

* Especificar valores como mostrado nesta captura de tela
* ![associação-2](assets/association-entities.png)
* **Estamos vinculando as duas entidades usando a coluna empID de beneficiários e novas entidades.**
* Clique em _Concluído_ para salvar as alterações

## Testar o modelo de dados do formulário

Nosso modelo de dados de formulário agora tem **_obter_** serviço que aceita empID e retorna os detalhes do novo proprietário e seus beneficiários. Para testar o serviço de obtenção, siga as etapas listadas abaixo.

* Selecionar nova entidade
* Clique em _Testar objeto de modelo_
* Forneça um empID válido e clique em _Teste_
* Você deve obter os resultados conforme mostrado na captura de tela abaixo
* ![test-fdm](assets/test-form-data-model.png)

## Próximas etapas

[Obter empID do URL](./get-request-parameter.md)