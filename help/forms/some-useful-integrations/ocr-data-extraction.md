---
title: Extração de dados OCR
description: Extrair dados de documentos emitidos pelo governo para preencher formulários.
feature: Barcoded Forms
version: 6.4,6.5
jira: KT-6679
topic: Development
role: Developer
level: Intermediate
exl-id: 1532a865-4664-40d9-964a-e64463b49587
last-substantial-update: 2019-07-07T00:00:00Z
duration: 194
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 0%

---

# Extração de dados OCR

Extrair dados automaticamente de uma grande variedade de documentos emitidos pelo governo para preencher seus formulários adaptáveis.

Há várias organizações que fornecem esse serviço e, desde que tenham APIs REST bem documentadas, é possível integrar facilmente ao AEM Forms usando o recurso de integração de dados. Para o propósito deste tutorial, usei [Analisador de ID](https://www.idanalyzer.com/) para demonstrar a extração de dados OCR de documentos carregados.

As etapas a seguir foram seguidas para implementar a extração de dados de OCR com o AEM Forms usando o serviço do Analisador de ID.

## Criar conta de desenvolvedor

Crie uma conta de desenvolvedor com [Analisador de ID](https://portal.idanalyzer.com/signin.html). Anote a chave de API. Essa chave é necessária para chamar as APIs REST do serviço do Analisador de ID.

## Criar arquivo Swagger/OpenAPI

A Especificação de OpenAPI (antiga Especificação do Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a API, incluindo:

* Pontos de extremidade disponíveis (/users) e operações em cada ponto de extremidade (GET /users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser escritas em YAML ou JSON. O formato é fácil de aprender e legível tanto para seres humanos quanto para máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga o [Documentação da OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms é compatível com a especificação OpenAPI versão 2.0 (fka Swagger).

Use o [editor swagger](https://editor.swagger.io/) para criar seu arquivo swagger para descrever as operações que enviam e verificam o código OTP enviado usando SMS. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo Swagger completo pode ser baixado de [aqui](assets/drivers-license-swagger.zip)

## Considerações ao definir o arquivo swagger

* As definições são obrigatórias
* $ref precisa ser usado para definições de método
* Preferir ter consume produz seções definidas
* Não defina parâmetros de corpo de solicitação em linha ou parâmetros de resposta. Tente modular o máximo possível. Por exemplo, a definição a seguir não é suportada

```json
 "name": "body",
            "in": "body",
            "required": false,
            "schema": {
              "type": "object",
              "properties": {
                "Rollnum": {
                  "type": "string",
                  "description": "Rollnum"
                }
              }
            }
```

O seguinte é suportado com uma referência à definição requestBody

```json
 "name": "requestBody",
            "in": "body",
            "required": false,
            "schema": {
              "$ref": "#/definitions/requestBody"
            }
```

* [Arquivo Swagger de amostra para sua referência](assets/sample-swagger.json)

## Criar fonte de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [criar fonte de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem. Use o [arquivo swagger](assets/drivers-license-swagger.zip) para criar sua fonte de dados.

## Criar modelo de dados do formulário

A integração de dados do AEM Forms fornece uma interface intuitiva para criar e trabalhar com [modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseie o modelo de dados do formulário na fonte de dados criada na etapa anterior.

![fdm](assets/test-dl-fdm.PNG)

## Criar biblioteca do cliente

Precisaríamos obter a string codificada em base64 do documento carregado. Essa string codificada em base64 é passada como um dos parâmetros de nossa invocação REST.
A biblioteca do cliente pode ser baixada [daqui.](assets/drivers-license-client-lib.zip)

## Criar formulário adaptável

Integre as invocações POST do modelo de dados de formulário ao formulário adaptável para extrair dados do documento carregado pelo usuário no formulário. Você pode criar seu próprio formulário adaptável e usar a invocação POST do modelo de dados de formulário para enviar a cadeia de caracteres codificada base64 do documento carregado.

## Implantar no servidor

Se quiser usar os ativos de amostra com sua chave de API, siga as seguintes etapas:

* [Baixar a fonte de dados](assets/drivers-license-source.zip) e importar para AEM usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Baixar o modelo de dados do formulário](assets/drivers-license-fdm.zip) e importar para AEM usando [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Baixar a biblioteca do cliente](assets/drivers-license-client-lib.zip)
* Baixe o formulário adaptável de exemplo [baixado aqui](assets/adaptive-form-dl.zip). Este formulário de amostra usa as invocações de serviço do modelo de dados de formulário fornecido como parte deste artigo.
* Importe o formulário para o AEM do [Forms e interface do usuário de documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Abra o formulário no [modo de edição.](http://localhost:4502/editor.html/content/forms/af/driverslicenseandpassport.html)
* Especifique sua Chave de API como o valor padrão no campo apikey e salve as alterações
* Abra o editor de regras para o campo String Base 64. Observe a invocação do serviço quando o valor desse campo for alterado.
* Salve o formulário
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/driverslicenseandpassport/jcr:content?wcmmode=disabled), carregue a foto da frente da sua licença de motorista
