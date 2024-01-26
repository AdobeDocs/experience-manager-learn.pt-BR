---
title: Criar formulário adaptável
description: Criar e configurar o formulário adaptável para usar o serviço de preenchimento do modelo de dados de formulário
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
duration: 164
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 2%

---

# Criar formulário adaptável

Até agora criamos o seguinte

* Banco de dados com 2 tabelas - `newhire` e `beneficiaries`
* Fonte de dados agrupada da conexão Apache Sling
* Modelo de dados de formulário baseado no RDBMS

A próxima etapa é criar e configurar um Formulário adaptável para usar o modelo de dados de formulário.  Para começar, você pode [baixar e importar](assets/fdm-demo-af.zip) formulário de exemplo. O formulário de amostra tem uma seção para exibir os detalhes do funcionário e outra seção para listar os beneficiários do funcionário.

## Associar formulário com modelo de dados de formulário

O formulário de amostra fornecido com este curso não está associado a nenhum modelo de dados de formulário. Para configurar o formulário para usar o modelo de dados de formulário, precisamos fazer o seguinte:

* Selecionar o formulário FDMDemo
* Clique em _Propriedades_->_Modelo de formulário_
* Selecione Modelo de dados do formulário na lista suspensa
* Pesquise e selecione o modelo de dados de formulário criado na lição anterior.
* Clique em _Salvar e fechar_

## Configurar o serviço de Preenchimento prévio

A primeira etapa é associar o serviço de preenchimento prévio ao formulário. Para associar o serviço de preenchimento prévio, siga as etapas mencionadas abaixo

* Selecione o `FDMDemo` formulário
* Clique em _Editar_ para abrir o formulário no modo de edição
* Selecione Container de formulário na hierarquia de conteúdo e clique no ícone de chave inglesa para abrir a folha de propriedades
* Selecionar _Serviço de preenchimento do modelo de dados de formulário_ na lista suspensa Serviço de preenchimento prévio
* Clique em azul ù para salvar as alterações

* ![serviço de preenchimento](assets/fdm-prefill.png)

## Configurar Detalhes do Funcionário

A próxima etapa é vincular os campos de texto do Formulário adaptável aos elementos do Modelo de dados de formulário. Será necessário abrir a folha de propriedades dos campos a seguir e definir seu bindRef como mostrado abaixo


| Nome do campo | Vincular Ref |
|------------|--------------------|
| Nome | /newhire/FirstName |
| Sobrenome | /newhire/lastName |

>[!NOTE]
>
>Fique à vontade para adicionar campos de texto adicionais e vinculá-los aos elementos apropriados do modelo de dados de formulário

## Configurar Tabela de Beneficiários

A próxima etapa é exibir os beneficiários do funcionário em forma de tabela. O formulário de amostra fornecido tem uma tabela com 4 colunas e uma única linha. Precisamos configurar a tabela para crescer dependendo do número de beneficiários.

* Abra o formulário no modo de edição.
* Expanda O Painel Raiz->Seus Beneficiários->Tabela
* Selecione Row1 e clique no ícone da chave inglesa para abrir sua folha de propriedades.
* Definir a referência de vinculação para **/newhire/GetEmployeeBeneficiaries**
* Defina as Configurações de repetição - Contagem mínima para 1 e Contagem máxima para 5.
* A configuração da Linha 1 deve ter a aparência da captura de tela abaixo
  ![row-configure](assets/configure-row.PNG)
* Clique no ícone azul de ù para salvar as alterações

## Associar células de linha

Por fim, precisamos vincular as células de Linha aos elementos do Modelo de dados de formulário.

* Expanda Painel Raiz->Seus Beneficiários->Tabela->Linha1
* Defina a Referência de associação de cada célula de linha de acordo com a tabela abaixo

| Célula de linha | Referência de vinculação |
|------------|----------------------------------------------|
| Nome | /newhire/GetEmployeeBeneficiaries/firstname |
| Sobrenome | /newhire/GetEmployeeBeneficiaries/lastname |
| Relação | /newhire/GetEmployeeBeneficiaries/relation |
| Porcentagem | /newhire/GetEmployeeBeneficiaries/percentage |

* Clique no ícone azul de ù para salvar as alterações

## Testar o formulário

Agora precisamos abrir o formulário com empID apropriado no url. Os 2 links a seguir preencherão os formulários com informações do banco de dados
[Formulário com empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulário com empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Resolução de problemas

Meu formulário está em branco e não tem dados

* Verifique se o modelo de dados de formulário está retornando os resultados corretos.
* O formulário está associado ao modelo de dados de formulário correto
* Verificar as associações de campo
* Verifique o arquivo de log stdout. Você deve ver a empID sendo gravada no arquivo.Se esse valor não aparecer, talvez o formulário não esteja usando o modelo personalizado fornecido.

A tabela não está preenchida

* Verifique a associação da Linha 1
* Verifique se as configurações de repetição da Linha 1 estão definidas corretamente (Mín =1 e Máx = 5 ou mais)
