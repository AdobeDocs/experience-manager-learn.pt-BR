---
title: AEM Forms com Marketo (Parte 4)
description: Tutorial para integrar o AEM Forms com o Marketo usando o Modelo de dados do formulário do AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Criação de um formulário adaptável usando o modelo de dados de formulário

A próxima etapa é criar um Formulário adaptável e baseá-lo no Modelo de dados de formulário criado na etapa anterior.
O usuário insere a ID do lead e, ao tabular o serviço Marketo para obter os leads por ID, é chamado. Os resultados da operação de serviço são mapeados para os campos apropriados do Forms adaptável.

1. Crie um formulário adaptável e baseie-o no &quot;Modelo de formulário em branco&quot;, associe-o ao Modelo de dados de formulário criado na etapa anterior.
1. Abrir o formulário no modo de edição
1. Arraste e solte um componente TextField e um componente Painel no Formulário adaptável. Defina o título do componente TextField &quot;Inserir ID do lead&quot; e defina seu nome como &quot;LeadId&quot;
1. Arraste e solte 2 componentes TextField no componente Painel
1. Defina o Nome e o Título dos 2 componentes Textfield como FirstName e LastName
1. Configure o componente do Painel para ser um componente repetível, definindo o Mínimo para 1 e o Máximo para -1. Isso é necessário, pois o serviço Marketo retorna uma matriz de objetos principais e você precisa ter um componente repetível para exibir os resultados. No entanto, nesse caso, estamos recebendo apenas um objeto de lead de volta porque estamos pesquisando em objetos de lead pela respectiva ID.
1. Crie uma regra no campo LeadId como mostrado na imagem abaixo
1. Pré-visualize o formulário, insira uma ID de cliente potencial válida no campo LeadID e pressione a tecla tab. Os campos Nome e Sobrenome devem ser preenchidos com os resultados da chamada de serviço.

A captura de tela a seguir explica as configurações do editor de regras

![editor de regras](assets/ruleeditor.jfif)

## Depuração

Se estiver usando os pacotes fornecidos com este artigo, talvez você queira ativar [logs de depuração](http://localhost:4502/system/console/slinglog) para as seguintes classes:

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## Parabéns

Você integrou com êxito o AEM Forms ao Marketo usando o Modelo de dados de formulário do AEM Forms.