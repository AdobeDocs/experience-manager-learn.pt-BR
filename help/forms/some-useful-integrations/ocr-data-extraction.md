---
title: Extração de Dados OCR
description: Extraia dados de documentos emitidos pelo governo para preencher formulários.
feature: Formulários com códigos de barras
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 2%

---



# Extração de Dados OCR

Extraia automaticamente dados de uma grande variedade de documentos emitidos pelo governo para preencher seus formulários adaptáveis.

Há várias organizações que fornecem esse serviço e, desde que tenham APIs REST bem documentadas, você pode se integrar facilmente ao AEM Forms usando o recurso de integração de dados. Para o objetivo deste tutorial, usei o [ID Analyzer](https://www.idanalyzer.com/) para demonstrar a extração de dados OCR de documentos carregados.

As etapas a seguir foram seguidas para implementar a extração de dados OCR com AEM Forms usando o serviço ID Analyzer.

## Criar conta do desenvolvedor

Crie uma conta de desenvolvedor com o [ID Analyzer](https://portal.idanalyzer.com/signin.html). Anote a Chave da API. Essa chave será necessária para invocar APIs REST do serviço do ID Analyzer.

## Criar arquivo Swagger/OpenAPI

A Especificação OpenAPI (antiga Especificação Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a sua API, incluindo:

* Pontos de extremidade (/users) disponíveis e operações em cada ponto de extremidade (GET/users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação
Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser gravadas em YAML ou JSON. O formato é fácil de aprender e legível para seres humanos e máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga a [documentação do OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms suporta a especificação OpenAPI versão 2.0 (Fka Swagger).

Use o [editor de swagger](https://editor.swagger.io/) para criar seu arquivo de swagger para descrever as operações que enviam e verificam o código OTP enviado usando SMS. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo de swagger concluído pode ser baixado de [aqui](assets/drivers-license-swagger.zip)

## Criar fonte de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [criar fonte de dados](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem. Use o [arquivo swagger](assets/drivers-license-swagger.zip) para criar sua fonte de dados.

## Criar modelo de dados do formulário

A integração de dados do AEM Forms fornece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseie o modelo de dados de formulário na fonte de dados criada na etapa anterior.

![fdm](assets/test-dl-fdm.PNG)

## Criar biblioteca do cliente

Precisaríamos obter a string codificada em base64 do documento carregado. Essa string codificada em base64 é então passada como um dos parâmetros da nossa invocação REST.
A biblioteca do cliente pode ser baixada [daqui.](assets/drivers-license-client-lib.zip)

## Criar formulário adaptável

Integre as invocações POST do Modelo de dados de formulário ao formulário adaptável para extrair dados do documento carregado pelo usuário no formulário. Você pode criar seu próprio formulário adaptável e usar a invocação POST do modelo de dados de formulário para enviar a string codificada em base64 do documento carregado.

## Implantar no servidor

Se quiser usar os ativos de exemplo com sua chave de API, siga as seguintes etapas:

* [Baixe a ](assets/drivers-license-source.zip) fonte de dados e importe para o AEM usando o gerenciador de  [pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Baixe o ](assets/drivers-license-fdm.zip) modelo de dados de formulário e importe para o AEM usando o gerenciador de  [pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Baixe a biblioteca do cliente](assets/drivers-license-client-lib.zip)
* Baixe o formulário adaptável de amostra pode ser [baixado aqui](assets/adaptive-form-dl.zip). Esse formulário de amostra usa as invocações de serviço do modelo de dados de formulário fornecido como parte deste artigo.
* Importe o formulário para o AEM a partir de [Forms e Document UI](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra o formulário no [modo de edição.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Especifique sua Chave de API como o valor padrão no campo apikey e salve as alterações
* Abra o editor de regras para o campo String Base 64. Observe a chamada de serviço quando o valor desse campo é alterado.
* Salvar o formulário
* [Visualize o formulário](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), faça upload da imagem frontal da sua licença de drivers


