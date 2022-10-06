---
title: AEM Forms com Marketo (Parte 4)
description: Tutorial para integrar o AEM Forms com o Marketo usando o Modelo de dados de formulário AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 1%

---

# Como criar um formulário adaptável usando o Modelo de dados de formulário

A próxima etapa é criar um formulário adaptável e baseá-lo no Modelo de dados de formulário criado na etapa anterior.
O usuário insere o ID de lead e, ao sair do serviço Marketo para obter os leads por id, é chamado. Os resultados da operação do serviço são mapeados para os campos apropriados do Adaptive Forms.

1. Crie um formulário adaptável e baseie-o em &quot;Modelo de formulário em branco&quot;, associe-o ao Modelo de dados de formulário criado na etapa anterior.
1. Abra o formulário no modo de edição
1. Arraste e solte um componente TextField e um componente Painel no Formulário adaptável. Defina o título do componente TextField &quot;Enter Lead Id&quot; e defina seu nome como &quot;LeadId&quot;
1. Arraste e solte 2 componentes TextField no componente Painel
1. Defina o Nome e o Título dos 2 componentes de Campo de texto como Nome e Sobrenome
1. Configure o componente Painel para ser um componente repetível, definindo o Mínimo para 1 e Máximo para -1. Isso é necessário, pois o serviço Marketo retorna uma matriz de objetos de lead e você precisa ter um componente repetível para exibir os resultados. No entanto, nesse caso, estamos obtendo apenas um objeto de lead de volta porque estamos pesquisando objetos de lead por sua ID.
1. Crie uma regra no campo LeadId como mostrado na imagem abaixo
1. Visualize o formulário e insira uma ID de lead válida no campo ID de lead e na guia. Os campos Nome e Sobrenome devem ser preenchidos com os resultados da chamada de serviço.

A captura de tela a seguir explica as configurações do editor de regras

![editor de regras](assets/ruleeditor.jfif)

## Depuração

Se estiver usando os pacotes fornecidos com este artigo, talvez você queira ativar [logs de depuração](http://localhost:4502/system/console/slinglog) para as seguintes classes:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
