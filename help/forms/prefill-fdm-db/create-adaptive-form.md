---
title: Criar formulário adaptável
description: Criar e configurar um formulário adaptável para usar o serviço de preenchimento prévio do modelo de dados de formulário
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: Desenvolvimento
role: Profissional
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 2%

---


# Criar formulário adaptável

Até agora criamos o seguinte

* Banco de dados com 2 tabelas - `newhire` e `beneficiaries`
* Fonte de dados agrupada da conexão Apache Sling configurada
* Modelo de dados de formulário baseado em RDBMS

A próxima etapa é criar e configurar um formulário adaptável para usar o modelo de dados de formulário.  Para obter o início do cabeçalho, você pode [baixar e importar](assets/fdm-demo-af.zip) formulário de amostra. O formulário de amostra tem uma seção para exibir os detalhes do funcionário e outra seção para listar os beneficiários do funcionário.

## Associar formulário ao modelo de dados de formulário

O formulário de amostra fornecido com esse curso não está associado a nenhum modelo de dados de formulário. Para configurar o formulário para usar o modelo de dados de formulário, é necessário fazer o seguinte:

* Selecione o formulário FDMDemo
* Clique em _Propriedades_->_Modelo de Formulário_
* Selecione Modelo de dados de formulário na lista suspensa
* Pesquise e selecione o Modelo de dados de formulário criado na lição anterior.
* Clique em _Salvar e fechar_

## Configurar o serviço de preenchimento prévio

A primeira etapa é associar o serviço de preenchimento prévio ao formulário. Para associar o serviço de preenchimento prévio, siga as etapas mencionadas abaixo

* Selecione o formulário `FDMDemo`
* Clique em _Editar_ para abrir o formulário no modo de edição
* Selecione Contêiner de formulário na hierarquia de conteúdo e clique no ícone de chave de fenda para abrir sua folha de propriedades
* Selecione _Serviço de preenchimento prévio do modelo de dados de formulário_ na lista suspensa Serviço de preenchimento prévio
* Clique em azul ☑ para salvar suas alterações

* ![serviço de preenchimento prévio](assets/fdm-prefill.png)

## Configurar Detalhes do Funcionário

A próxima etapa é vincular os campos de texto do Formulário adaptável aos elementos do Modelo de dados de formulário. Será necessário abrir a folha de propriedades dos seguintes campos e definir o bindRef como mostrado abaixo


| Nome do campo | Ref. de Ligação |
|------------|--------------------|
| Nome | /newhire/FirstName |
| Sobrenome | /newhire/lastName |

>[!NOTE]
>
>Você pode adicionar campos de texto adicionais e vinculá-los aos elementos apropriados do modelo de dados de formulário

## Configurar Tabela de Beneficiários

O próximo passo é exibir os beneficiários do funcionário de forma tabular. O formulário de amostra fornecido tem uma tabela com 4 colunas e uma única linha. Precisamos configurar a tabela para crescer dependendo do número de beneficiários.

* Abra o formulário no modo de edição.
* Expanda o painel raiz->Seus beneficiários->Tabela
* Selecione Linha1 e clique no ícone da chave de fenda para abrir sua folha de propriedades.
* Defina a Referência de vinculação para **/new/GetEmployeeBeneficiaries**
* Defina as Configurações de repetição - Contagem mínima para 1 e Contagem máxima para 5.
* A configuração da Linha1 deve ser parecida com a captura de tela abaixo
   ![row-configure](assets/configure-row.PNG)
* Clique no ☑ azul para salvar as alterações

## Vincular células da linha

Por fim, precisamos vincular as células de Linha aos elementos do Modelo de dados de formulário.

* Expanda o painel raiz->Seus beneficiários->Tabela->Linha1
* Definir a Referência de associação de cada célula de linha de acordo com a tabela abaixo

| Célula da linha | Referência de vinculação |
|------------|----------------------------------------------|
| Nome | /newhire/GetEmployeeBeneficiaries/firstname |
| Sobrenome | /newhire/GetEmployeeBeneficiaries/lastname |
| Relação | /new/GetEmployeeBeneficiaries/relation |
| Porcentagem | /new/GetEmployeeBeneficiaries/percentual |

* Clique no ☑ azul para salvar as alterações

## Teste seu formulário

Agora precisamos abrir o formulário com a empID apropriada no url. Os 2 links a seguir preencherão formulários com informações do Banco de Dados
[Formulário com empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulário com empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Resolução de problemas

Meu formulário está em branco e não tem dados

* Certifique-se de que o modelo de dados de formulário esteja retornando os resultados corretos.
* O formulário está associado ao Modelo de dados de formulário correto
* Verificar vínculos de campo
* Verifique o arquivo de log de stdout. Você deve ver o empID sendo gravado no arquivo.Se você não vir esse valor, o formulário pode não estar usando o modelo personalizado fornecido.

Tabela não preenchida

* Verificar o vínculo da Linha1
* Verifique se as configurações de repetição para Linha1 estão definidas corretamente (Min = 1 e Max = 5 ou mais)

