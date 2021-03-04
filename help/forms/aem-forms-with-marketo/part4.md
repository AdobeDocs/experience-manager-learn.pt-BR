---
title: AEM Forms com Marketo (Parte 4)
seo-title: AEM Forms com Marketo (Parte 4)
description: Tutorial para integrar o AEM Forms com o Marketo usando o AEM Forms Form Data Model.
seo-description: Tutorial para integrar o AEM Forms com o Marketo usando o AEM Forms Form Data Model.
feature: '"Formulários adaptáveis, Modelo de dados de formulário"'
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---


# Como criar um formulário adaptável usando o Modelo de dados de formulário

A próxima etapa é criar um formulário adaptável e baseá-lo no Modelo de dados de formulário criado na etapa anterior.
O usuário inserirá a ID do cliente potencial e, ao sair do serviço Marketo para obter os leads por id, será chamado. Os resultados da operação de serviço são mapeados para os campos apropriados dos Formulários Adaptativos.

1. Crie um formulário adaptável e baseie-o em &quot;Modelo de formulário em branco&quot;, associe-o ao Modelo de dados de formulário criado na etapa anterior.
1. Abra o formulário no modo de edição
1. Arraste e solte um componente TextField e um componente Painel no Formulário adaptável. Defina o título do componente TextField &quot;Enter Lead Id&quot; e defina seu nome como &quot;LeadId&quot;
1. Arraste e solte 2 componentes TextField no componente Painel
1. Defina o Nome e o Título dos 2 componentes de Campo de texto como Nome e Sobrenome
1. Configure o componente Painel para ser um componente repetível, definindo o Mínimo para 1 e Máximo para -1. Isso é necessário, pois o serviço Marketo retorna uma matriz de objetos de lead e você precisa ter um componente repetível para exibir os resultados. No entanto, nesse caso, obteremos apenas um objeto de lead de volta porque estamos pesquisando objetos de lead por sua ID.
1. Crie uma regra no campo LeadId como mostrado na imagem abaixo
1. Visualize o formulário e insira uma ID de lead válida no campo ID de lead e na guia. Os campos Nome e Sobrenome devem ser preenchidos com os resultados da chamada de serviço.

A captura de tela a seguir explica as configurações do editor de regras

![editor de regras](assets/ruleeditor.jfif)
