---
title: Criar formulário adaptável
description: Criar e configurar formulários adaptáveis para usar o serviço de preenchimento prévio do modelo de dados de formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---


# Criar formulário adaptável

Até agora criamos o seguinte

* Banco de dados com 2 tabelas - `newhire` e `beneficiaries`
* Fonte de dados agrupada da conexão Apache Sling configurada
* Modelo de dados de formulário baseado em RDBMS

A próxima etapa é criar e configurar um formulário adaptável para usar o modelo de dados do formulário.  Para obter o start principal, você pode [baixar e importar](assets/fdm-demo-af.zip) formulário de amostra. O formulário de amostra tem uma seção para exibir os detalhes do funcionário e outra seção para a lista dos beneficiários do funcionário.

## Associar formulário ao modelo de dados de formulário

O formulário de amostra fornecido com esse curso não está associado a nenhum modelo de dados de formulário. Para configurar o formulário para usar o modelo de dados de formulário, é necessário fazer o seguinte:

* Selecione o formulário FDMDemo
* Clique em _Propriedades_->_Modelo de Formulário_
* Selecione Modelo de dados de formulário na lista suspensa
* Pesquise e selecione seu Modelo de dados de formulário criado na lição anterior.
* Clique em _Salvar e fechar_

## Configurar o serviço de Prefill

A primeira etapa é associar o serviço de preenchimento prévio ao formulário. Para associar o serviço de preenchimento prévio, siga as etapas mencionadas abaixo

* Selecione o formulário `FDMDemo`
* Clique em _Editar_ para abrir o formulário no modo de edição
* Selecione Container de formulário na hierarquia de conteúdo e clique no ícone de chave para abrir sua folha de propriedades
* Selecione _Serviço de Pré-preenchimento do Modelo de Dados de Formulário_ na lista suspensa Serviço de Pré-preenchimento
* Clique em azul ☑ para salvar suas alterações

* ![serviço de preenchimento prévio](assets/fdm-prefill.png)

## Configurar Detalhes do Funcionário

A próxima etapa é vincular os campos de texto do Formulário adaptável aos elementos do Modelo de dados de formulário. Será necessário abrir a folha de propriedades dos seguintes campos e definir bindRef como mostrado abaixo


| Nome do campo | Ref. de Ligação |
|------------|--------------------|
| Nome | /newhire/FirstName |
| Sobrenome | /newhire/lastName |

>[!NOTE]
>
>Sinta-se à vontade para adicionar campos de texto adicionais e vinculá-los aos elementos apropriados do modelo de dados de formulário

## Configurar tabela de beneficiários

O próximo passo é exibir os beneficiários do funcionário de forma tabular. O formulário de amostra fornecido tem uma tabela com 4 colunas e uma única linha. Precisamos configurar o quadro para que cresça dependendo do número de beneficiários.

* Abra o formulário no modo de edição.
* Expandir painel raiz->Seus beneficiários->Tabela
* Selecione Linha1 e clique no ícone de chave para abrir sua folha de propriedades.
* Defina a Referência de vínculo como **/newhire/GetEmployeeBenaries**
* Defina Repeat Settings (Repetir configurações) - Minimum Count (Contagem mínima) como 1 e Maximum Count (Contagem máxima) como 5.
* A configuração da sua linha1 deve ser parecida com a captura de tela abaixo
   ![configuração de linha](assets/configure-row.PNG)
* Clique na ☑ azul para salvar suas alterações

## Vincular células de linha

Por fim, precisamos vincular as células Linha aos elementos do Modelo de dados de formulário.

* Expanda o painel raiz->Seus beneficiários->Tabela->Linha1
* Defina a Referência de vínculo de cada célula de linha de acordo com a tabela abaixo

| Célula da linha | Referência de vinculação |
|------------|----------------------------------------------|
| Nome | /newhire/GetWorkersBeneficiaries/firstname |
| Sobrenome | /newhire/GetWorkersBeneficiaries/lastname |
| Relação | /newhire/GetWorkersBeneficiaries/relation |
| Porcentagem | /newhire/GetWorkersBeneficiaries/percentual |

* Clique na ☑ azul para salvar suas alterações

## Testar seu formulário

Agora, precisamos abrir o formulário com a empID apropriada no url. Os 2 links a seguir preencherão formulários com informações do banco de dados
[Formulário com empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulário com empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Resolução de problemas

Meu formulário está em branco e não possui dados

* Verifique se o modelo de dados de formulário está retornando os resultados corretos.
* O formulário está associado ao Modelo de dados de formulário correto
* Verificar os vínculos de campo
* Verifique o arquivo de log stdout. Você deve ver empID sendo gravado no arquivo.Se você não vir esse valor, o formulário pode não estar usando o modelo personalizado fornecido.

A tabela não está preenchida

* Verifique o vínculo de Linha1
* Verifique se as configurações de repetição para Row1 estão definidas corretamente (Min = 1 e Max = 5 ou mais)

