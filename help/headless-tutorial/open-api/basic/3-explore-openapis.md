---
title: Explorar a documentação da OpenAPI | AEM Headless Parte 3
description: Introdução ao Adobe Experience Manager (AEM) e OpenAPI. Explore as APIs de entrega de fragmento de conteúdo baseadas em OpenAPI do AEM usando a documentação da API integrada. Saiba como o AEM gera automaticamente esquemas de OpenAPI com base em modelos de Fragmento de conteúdo. Experimente a construção de consultas básicas usando a documentação da API.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 400
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 0%

---


# Explorar APIs de entrega de fragmentos de conteúdo baseadas em OpenAPI do AEM

A [Entrega de fragmento de conteúdo do AEM com APIs OpenAPI](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) no AEM fornece uma maneira poderosa de fornecer conteúdo estruturado a qualquer aplicativo ou canal. Neste capítulo, exploramos como usar as OpenAPIs para recuperar fragmentos de conteúdo por meio da funcionalidade **Experimente** da documentação.

## Pré-requisitos {#prerequisites}

Este é um tutorial de várias partes e presume que as etapas descritas em [Criação de fragmentos de conteúdo](./2-author-content-fragments.md) foram concluídas.

Certifique-se de ter o seguinte:

* O nome de host do serviço de Publicação do AEM (por exemplo, `https://publish-<PROGRAM_ID>-e<ENVIRONMENT_ID >.adobeaemcloud.com/`) em que os [Fragmentos de conteúdo são publicados](./2-author-content-fragments.md#publish-content-fragments). Se você estiver publicando um serviço de Visualização do AEM, tenha esse nome de host disponível (por exemplo, `https://preview-<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/`).

## Objetivos {#objectives}

* Familiarize-se com a [Entrega de fragmento de conteúdo do AEM com APIs OpenAPI](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/).
* Invoque as APIs usando o recurso **Experimentar** da API Docs.

## APIs de entrega

A entrega de fragmento de conteúdo do AEM com APIs OpenAPI fornece uma interface RESTful para recuperar fragmentos de conteúdo. As APIs discutidas neste tutorial estão disponíveis apenas nos serviços de Publicação e visualização do AEM, e não no serviço do Autor. Existem outras OpenAPIs para [interagir com Fragmentos de conteúdo no serviço de Autor do AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/).

## Explore as APIs

[A documentação de Entrega de fragmentos de conteúdo do AEM com APIs OpenAPI](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) tem um recurso &quot;Experimente&quot; que permite explorar as APIs e testá-las diretamente do navegador. Essa é uma ótima maneira de se familiarizar com os endpoints da API e seus recursos.

Abra os [documentos da API do AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) em seu navegador.

As APIs estão listadas na navegação à esquerda na seção **Entrega de fragmento**. É possível expandir esta seção para ver as APIs disponíveis. Selecionar uma API exibe os detalhes da API no painel principal e uma seção **Experimente** no painel direito que permite testar e explorar a API diretamente do navegador.

![Entrega de fragmento de conteúdo do AEM com documentos de APIs OpenAPI](./assets/3/docs-overview.png)

## Listar fragmentos de conteúdo

1. Abra a [Entrega de fragmento de conteúdo do AEM com documentos do desenvolvedor do OpenAPI](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) no navegador.
1. Na navegação à esquerda, expanda a seção **Entrega de fragmento** e selecione a API **Listar todos os fragmentos de conteúdo**

Essa API permite recuperar uma lista paginada de todos os fragmentos de conteúdo do AEM por pasta. A maneira mais simples de usar essa API é fornecer o caminho para a pasta que contém os fragmentos de conteúdo.

1. Selecione **Experimente** na parte superior do painel direito.
1. Insira o identificador do serviço do AEM ao qual a API se conectará para recuperar os fragmentos de conteúdo. O bucket é a primeira parte da URL do serviço de Publicação (ou Visualização) do AEM, normalmente no formato: `publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>` ou `preview-p<PROGRAM_ID>-e<ENVIRONMENT_ID>`.

Como estamos usando o serviço de Publicação do AEM, defina o bucket para o identificador do serviço de Publicação do AEM. Por exemplo:

* **compartimento**: `publish-p138003-e1400351`

![Experimente este bucket](./assets/3/try-it-bucket.png)

Quando o bucket é definido, o campo **Servidor de destino** é atualizado automaticamente para a URL completa da API do serviço de Publicação do AEM, como: `https://publish-p138003-e1400351.adobeaemcloud.com/adobe/contentFragments`

1. Expanda a seção **Segurança** e defina **Esquema de segurança** como **Nenhum**. Isso ocorre porque o serviço de Publicação do AEM (e o serviço de Visualização) não requer autenticação para a Entrega de fragmento de conteúdo do AEM com APIs OpenAPI.

1. Expanda a seção **Parâmetros** para fornecer os detalhes do Fragmento de conteúdo a ser obtido.

* **cursor**: deixe vazio, ele é usado para paginação e esta é uma solicitação inicial.
* **limite**: deixe vazio, isso é usado para limitar o número de resultados retornados por página de resultados.
* **caminho**: `/content/dam/my-project/en`

  >[!TIP]
  > Ao inserir um caminho, verifique se o prefixo é `/content/dam/` e se **não** termina com uma barra à direita `/`.

  ![Experimente os parâmetros](assets/3/try-it-parameters.png)

1. Selecione o botão **Enviar** para executar a chamada de API.
1. Na guia **Resposta** do painel **Experimente**, você deve ver uma resposta JSON contendo uma lista de Fragmentos de Conteúdo na pasta especificada. A resposta será semelhante ao seguinte:

   ![Tente sua resposta](./assets/3/try-it-response.png)

1. A resposta contém todos os Fragmentos de Conteúdo na pasta `path` do parâmetro `/content/dam/my-project`, incluindo subpastas, incluindo os Fragmentos de Conteúdo de **Pessoa** e **Equipe**.
1. Clique na matriz `items` e localize o valor `Team Alpha` do item `id`. A ID é usada na próxima seção para recuperar os detalhes de um único Fragmento de conteúdo.
1. Selecione **Editar solicitação** na parte superior do painel **Testar** e os vários parâmetros na chamada de API para ver como a resposta é alterada. Por exemplo, você pode alterar o caminho para uma pasta diferente contendo Fragmentos de conteúdo ou adicionar parâmetros de consulta para filtrar os resultados. Por exemplo, altere o parâmetro `path` para `/content/dam/my-project/teams` para somente Fragmentos de conteúdo nessa pasta (e subpastas).

## Obter detalhes do fragmento de conteúdo

Semelhante à API **Listar todos os fragmentos de conteúdo**, a API **Obter um fragmento de conteúdo** recupera um único fragmento de conteúdo por sua ID, juntamente com todas as referências opcionais. Para explorar essa API, solicitaremos o Fragmento de conteúdo da equipe que faz referência a vários Fragmentos de conteúdo de pessoas.

1. Expanda a seção **Entrega de fragmento** no painel esquerdo e selecione a API **Obter um fragmento de conteúdo**.
1. Selecione **Experimente** na parte superior do painel direito.
1. Verifique se o `bucket` aponta para o serviço de Publicação ou Visualização do AEM as a Cloud Service.
1. Expanda a seção **Segurança** e defina **Esquema de segurança** como **Nenhum**. Isso ocorre porque o serviço de publicação do AEM não requer autenticação para a entrega de fragmentos de conteúdo do AEM com APIs OpenAPI.
1. Expanda a seção **Parâmetros** para fornecer os detalhes do Fragmento de conteúdo a serem obtidos:

Neste exemplo, use a ID do Fragmento de conteúdo da equipe recuperada na seção anterior. Por exemplo, para esta resposta de Fragmento de Conteúdo em **Listar todos os Fragmentos de Conteúdo**, use o valor no campo `id` de `b954923a-0368-4fa2-93ea-2845f599f512`. (Seu `id` será diferente do valor usado no tutorial.)

```json
{
    "path": "/content/dam/my-project/teams/team-alpha",
    "name": "",
    "title": "Team Alpha",
    "id": "50f28a14-fec7-4783-a18f-2ce2dc017f55", // This is the Content Fragment ID
    "description": "",
    "model": {},
    "fields": {} 
}
```

* **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
* **referências**: `none`
* **profundidade**: deixe vazio, o parâmetro **referências** determinará a profundidade dos fragmentos referenciados.
* **hidratado**: se deixado em branco, o parâmetro **references** ditará a hidratação dos fragmentos referenciados.
* **If-None-Match**: Deixe em branco

1. Selecione o botão **Enviar** para executar a chamada de API.
1. Revise a resposta na guia **Resposta** do painel **Experimente**. Você deve ver uma resposta JSON contendo os detalhes do fragmento de conteúdo, incluindo suas propriedades e quaisquer referências que ele tenha.
1. Selecione **Editar solicitação** na parte superior do painel **Experimentar** e, nas seções **Parâmetros**, ajuste o parâmetro `references` para `all-hydrated`, fazendo com que todo o conteúdo do Fragmento de conteúdo referenciado seja incluído na chamada de API.

   * **fragmentId**: `50f28a14-fec7-4783-a18f-2ce2dc017f55`
   * **referências**: `all-hydrated`
   * **profundidade**: deixe vazio, o parâmetro **referências** determinará a profundidade dos fragmentos referenciados.
   * **hidratado**: se deixado em branco, o parâmetro **references** ditará a hidratação dos fragmentos referenciados.
   * **If-None-Match**: Deixe em branco

1. Selecione o botão **Reenviar** para executar a chamada de API novamente.
1. Revise a resposta na guia **Resposta** do painel **Experimente**. Você deve ver uma resposta JSON contendo os detalhes do fragmento de conteúdo, incluindo suas propriedades e as dos fragmentos de conteúdo de pessoa referenciados.

Observe que a matriz `teamMembers` agora inclui os detalhes dos Fragmentos de conteúdo de pessoa referenciados. As referências de hidratação permitem recuperar todos os dados necessários em uma única chamada de API, o que é particularmente útil para reduzir o número de solicitações feitas por aplicativos clientes.

## Parabéns!

Parabéns, você criou e executou várias entregas de fragmento de conteúdo do AEM com chamadas da API OpenAPI usando o recurso **Experimente** da documentação do AEM.

## Próximas etapas

No próximo capítulo, [Criar um aplicativo React](./4-react-app.md), você explora como um aplicativo externo pode interagir com a Entrega de fragmentos de conteúdo do AEM com APIs OpenAPI.

