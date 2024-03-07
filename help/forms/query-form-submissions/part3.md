---
title: Criar interface de consulta
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas na consulta de envios de formulários armazenados no portal do Azure
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 1%

---

# Criar interface de consulta

Uma interface de consulta simples foi criada para permitir que o &quot;Administrador&quot; insira critérios de pesquisa para recuperar envios de formulários específicos. Os resultados são exibidos em um formato de tabela simples com a opção para visualizar o envio de um formulário específico.

![envios de consultas](assets/query-submissions.png)

Os menus suspensos nessa interface são uma lista suspensa em cascata. As opções disponíveis na lista suspensa mudam com base nas seleções feitas na lista suspensa anterior.

Os menus suspensos são preenchidos com o uso de fontes de dados RESTful.

Os resultados da pesquisa são exibidos em um componente personalizado chamado &quot;Resultados da pesquisa&quot;. Quando o usuário clica no botão de exibição, o formulário é preenchido previamente com os dados e anexos enviados.

## Próximas etapas

[Gravar o serviço de preenchimento prévio](./part4.md)
