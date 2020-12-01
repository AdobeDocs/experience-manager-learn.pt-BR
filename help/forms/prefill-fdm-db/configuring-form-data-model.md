---
title: Configuração do modelo de dados de formulário
description: Criar modelo de dados de formulário com base na fonte de dados RDBMS
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---



# Configuração do modelo de dados de formulário

## Fonte de dados agrupada da conexão Apache Sling

A primeira etapa na criação do modelo de dados de formulário RDBMS é configurar a Apache Sling Connection Pooling DataSource. Para configurar a fonte de dados, siga as etapas listadas abaixo:

* Aponte seu navegador para [configMgr](http://localhost:4502/system/console/configMgr)
* Procure **Apache Sling Connection Pooling Data Source**
* Adicione uma nova entrada e forneça os valores conforme mostrado na captura de tela.
* ![fonte de dados](assets/data-source.png)
* Salvar suas alterações

>[!NOTE]
>O URI, o nome de usuário e a senha da conexão JDBC serão alterados dependendo da configuração do banco de dados MySQL.


## Criando Modelo de Dados de Formulário

* Aponte seu navegador para [Integrações de dados](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Clique em _Criar_->_Modelo de Dados de Formulário_
* Forneça um nome significativo e um título para o modelo de dados de formulário, como **Funcionário**
* Clique em _Avançar_
* Selecione a fonte de dados criada na seção anterior (fóruns)
* Clique em _Criar_->Editar para abrir o modelo de dados de formulário recém-criado no modo de edição
* Expanda o nó _forums_ para ver o schema do funcionário. Expanda o nó do funcionário para ver as 2 tabelas

## Adicionar entidades ao seu modelo

* Verifique se o nó do funcionário está expandido
* Selecione as entidades atuais e de beneficiários e clique em _Adicionar Selecionados_

## Adicionar Serviço de Leitura à entidade nova

* Selecionar entidade nova
* Clique em _Editar propriedades_
* Selecione obter na lista suspensa Serviço de leitura
* Clique no ícone + para adicionar um parâmetro ao serviço get
* Especifique os valores como mostrado na captura de tela
* ![get-service](assets/get-service.png)
>[!NOTE]
> O serviço get espera um valor mapeado para a coluna empID da entidade nova.Há várias maneiras de passar esse valor e neste tutorial, a empID será transmitida pelo parâmetro de solicitação chamado empID.
* Clique em _Concluído_ para salvar os argumentos para o serviço get
* Clique em _Concluído_ para salvar as alterações no modelo de dados de formulário

## Adicionar associação entre duas entidades

As associações definidas entre entidades de banco de dados não são criadas automaticamente no modelo de dados de formulário. As associações entre entidades precisam ser definidas usando o editor de modelo de dados de formulário. Todas as entidades envolvidas podem ter um ou mais beneficiários, precisamos de definir uma associação entre as entidades receptoras e as entidades beneficiárias.
As etapas a seguir o guiarão pelo processo de criação da associação um para muitos

* Selecione a entidade nova e clique em _Adicionar Associação_
* Forneça um Título significativo e um identificador para a associação e outras propriedades, conforme mostrado na captura de tela abaixo
   ![associação](assets/association-entities-1.png)

* Clique no ícone _edit_ na seção Argumentos

* Especificar valores como mostrado nesta captura de tela
* ![associação-2](assets/association-entities.png)
* **Estamos ligando as duas entidades usando a coluna empID de beneficiários e entidades novas.**
* Clique em _Concluído_ para salvar as alterações

## Testar seu modelo de dados de formulário

Nosso modelo de dados de formulário agora tem o serviço **_get_** que aceita empID e retorna os detalhes da empresa e seus beneficiários. Para testar o serviço get, siga as etapas listadas abaixo.

* Selecionar entidade nova
* Clique em _Testar objeto de modelo_
* Forneça empID válido e clique em _Testar_
* Deve obter os resultados apresentados na captura de ecrã abaixo
* ![test-fdm](assets/test-form-data-model.png)
