---
title: EXTRAÇÃO de dados OCR
description: Extraia dados de documentos emitidos pelo governo para preencher formulários.
feature: integrations
topics: adaptive forms
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
kt: 6679
translation-type: tm+mt
source-git-commit: c0db84f25334106c798d555c754d550113e91eac
workflow-type: tm+mt
source-wordcount: '647'
ht-degree: 0%

---



# EXTRAÇÃO de dados OCR

Extraia automaticamente dados de uma grande variedade de documentos emitidos pelo governo para preencher seus formulários adaptáveis.

Há várias organizações que fornecem esse serviço e, desde que tenham APIs REST bem documentadas, você pode facilmente se integrar à AEM Forms usando o recurso de integração de dados. Para a finalidade deste tutorial, usei [Analisador de ID](https://www.idanalyzer.com/) para demonstrar a extração de dados de OCR de documentos carregados.

As etapas a seguir foram seguidas para implementar a extração de dados de OCR com a AEM Forms usando o serviço de Analisador de ID.

## Criar conta de desenvolvedor

Crie uma conta de desenvolvedor com [Analisador de ID](https://portal.idanalyzer.com/signin.html). Anote a chave da API. Essa chave será necessária para chamar APIs REST do serviço do Analisador de ID.

## Criar arquivo Swagger/OpenAPI

A especificação OpenAPI (antiga especificação Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite que você descreva toda a sua API, incluindo:

* Pontos de extremidade (/users) e operações disponíveis em cada ponto de extremidade (GET /usuários, POST /usuários)
* Parâmetros de operação Entrada e saída para cada operação
Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser escritas em YAML ou JSON. O formato é fácil de aprender e legível para humanos e máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga a [documentação do OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> A AEM Forms suporta a especificação OpenAPI versão 2.0 (fka Swagger).

Use o [editor de swagger](https://editor.swagger.io/) para criar seu arquivo swagger para descrever as operações que enviam e verificam o código OTP enviado por SMS. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo swagger concluído pode ser baixado de [here](assets/drivers-license-swagger.zip)

## Criar fonte de dados

Para integrar o AEM/AEM Forms a aplicativos de terceiros, precisamos [criar a fonte de dados](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem. Use o arquivo [swagger](assets/drivers-license-swagger.zip) para criar sua fonte de dados.

## Criar modelo de dados do formulário

A integração de dados da AEM Forms fornece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://docs.adobe.com/content/help/en/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseie o modelo de dados de formulário na fonte de dados criada na etapa anterior.

![fdm](assets/test-dl-fdm.PNG)

## Criar biblioteca do cliente

Precisaríamos obter a string codificada em base64 do documento carregado. Essa string codificada em base64 é transmitida como um dos parâmetros de nossa invocação REST.
A biblioteca do cliente pode ser baixada [daqui.](assets/drivers-license-client-lib.zip)

## Criar formulário adaptável

Integre as invocações POST do Modelo de dados de formulário ao formulário adaptável para extrair dados do documento carregado pelo usuário no formulário. Você pode criar seu próprio formulário adaptável e usar a invocação POST do modelo de dados de formulário para enviar a string codificada base64 do documento carregado.

## Implantar em seu servidor

Se você quiser usar os ativos de amostra com sua chave de API, siga as seguintes etapas:

* [Baixe a ](assets/drivers-license-source.zip) fonte de dados e importe para AEM usando o gerenciador de  [pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Baixe o ](assets/drivers-license-fdm.zip) modelo de dados de formulário e importe para AEM usando o gerenciador de  [pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Baixar a biblioteca do cliente](assets/drivers-license-client-lib.zip)
* O download do formulário adaptável de amostra pode ser [baixado daqui](assets/adaptive-form-dl.zip). Este formulário de amostra usa as invocações de serviço do modelo de dados de formulário fornecido como parte deste artigo.
* Importe o formulário para o AEM a partir da interface de usuário do Forms e Documento [](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra o formulário no [modo de edição.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Especifique sua chave de API como o valor padrão no campo apikey e salve suas alterações
* Abra o editor de regras para o campo String base 64. Observe a chamada de serviço quando o valor desse campo é alterado.
* Salvar o formulário
* [Pré-visualização o formulário](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), carregue a imagem frontal da sua licença de driver


