---
title: AEM Forms com Marketo (Parte 4)
description: Tutorial to integrate AEM Forms with Marketo using AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: 020852f16de0cdb1e17e19ad989dabf37b7f61f5
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 1%

---

# Creating Adaptive Form using Form Data Model

A próxima etapa é criar um formulário adaptável e baseá-lo no Modelo de dados de formulário criado na etapa anterior.
O usuário inserirá a ID de lead e, ao sair do serviço Marketo para obter os leads por id, será chamado. Os resultados da operação do serviço são mapeados para os campos apropriados do Adaptive Forms.

1. Create an Adaptive Form and base it on &quot;Blank Form Template&quot;, associate it with the Form Data Model created in the earlier step.
1. Abra o formulário no modo de edição
1. Drag and drop a TextField component and a Panel component on to the Adaptive Form. Defina o título do componente TextField &quot;Enter Lead Id&quot; e defina seu nome como &quot;LeadId&quot;
1. Arraste e solte 2 componentes TextField no componente Painel
1. Defina o Nome e o Título dos 2 componentes de Campo de texto como Nome e Sobrenome
1. Configure o componente Painel para ser um componente repetível, definindo o Mínimo para 1 e Máximo para -1. This is required as the Marketo service returns an array of Lead Objects and you need to have a repeatable component to display the results. However, in this case, we will be getting only one Lead object back because we are searching on Lead objects by its ID.
1. Create a rule on the LeadId field as shown in the image below
1. Visualize o formulário e insira uma ID de lead válida no campo ID de lead e na guia. Os campos Nome e Sobrenome devem ser preenchidos com os resultados da chamada de serviço.

A captura de tela a seguir explica as configurações do editor de regras

![ruleeditor](assets/ruleeditor.jfif)

## Depuração

Se estiver usando os pacotes fornecidos com este artigo, talvez você queira ativar [debug logs](http://localhost:4502/system/console/slinglog) para as seguintes classes:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
