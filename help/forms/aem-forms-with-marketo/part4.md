---
title: AEM Forms com Marketo (Parte 4)
seo-title: AEM Forms com Marketo (Parte 4)
description: Tutorial para integrar o AEM Forms ao Marketing usando o Modelo de dados de formulário AEM Forms.
seo-description: Tutorial para integrar o AEM Forms ao Marketing usando o Modelo de dados de formulário AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# Criação de formulário adaptável usando o modelo de dados de formulário

A próxima etapa é criar um Formulário adaptável e baseá-lo no Modelo de dados de formulário criado na etapa anterior.
O usuário inserirá a ID de cliente potencial e ao sair do serviço de marketing para obter os clientes potenciais por ID será chamado. Os resultados da operação do serviço são mapeados para os campos apropriados do Forms Adaptive.

1. Crie um formulário adaptável e baseie-o em &quot;Modelo de formulário em branco&quot;, associe-o ao Modelo de dados de formulário criado na etapa anterior.
1. Abrir o formulário no modo de edição
1. Arraste e solte um componente TextField e um componente Painel no Formulário adaptável. Defina o título do componente TextField &quot;Enter Lead Id&quot; e defina seu nome como &quot;LeadId&quot;
1. Arraste e solte 2 componentes TextField no componente Painel
1. Defina o Nome e o Título dos 2 componentes do campo de texto como Nome e Sobrenome
1. Configure o componente Painel para ser um componente repetível definindo o Mínimo para 1 e Máximo para -1. Isso é necessário, pois o serviço de marketing retorna uma matriz de objetos de venda e é necessário ter um componente repetível para exibir os resultados. No entanto, nesse caso, obteremos apenas um objeto de cliente potencial de volta, pois estamos pesquisando objetos de cliente potencial por sua ID.
1. Crie uma regra no campo LeadId como mostrado na imagem abaixo
1. Pré-visualização o formulário e insira uma ID de cliente potencial válida no campo ID de cliente potencial e na guia para fora. Os campos Nome e Sobrenome devem ser preenchidos com os resultados da chamada de serviço.

A captura de tela a seguir explica as configurações do editor de regras

![editor de regras](assets/ruleeditor.jfif)
