---
title: Adicionar itens ao componente do grupo de opções
seo-title: Adicionar itens ao componente do grupo de opções
description: Adicionar itens ao componente do grupo de opções dinamicamente
seo-description: Adicionar itens ao componente do grupo de opções dinamicamente
feature: adaptive-forms
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---



# Adicionar itens dinamicamente ao componente de grupo de escolha

O AEM Forms 6.5 introduziu a capacidade de adicionar itens dinamicamente a um componente de grupo de escolha da Forms Adaptive, como CheckBox, Radio Button e Image Lista.

[Esse recurso está disponível ao vivo no Samples Server](https://forms.enablementadobe.com/content/samples/samples.html?query=0). Procure o cartão de itens da caixa de seleção dinâmica e clique em &quot;Tentar&quot;


Você pode adicionar itens usando o editor visual, bem como o editor de código, dependendo do caso de uso.

**Uso do editor visual:** Você pode preencher os itens do grupo de escolha a partir dos resultados de uma chamada de função ou chamada de serviço. Por exemplo, você pode definir os itens do grupo de escolha usando a resposta de uma chamada REST API.

Na captura de tela abaixo, estamos definindo as opções de Período de empréstimo (anos) com os resultados de uma chamada de serviço chamada getLoanPeriods.

![Editor de regras](assets/ruleeditor.png)

**Usando o editor** de código: Quando você deseja definir os itens no grupo de opções dinamicamente com base nos valores inseridos no formulário. Por exemplo, o trecho de código a seguir define os itens da caixa de seleção para os valores inseridos nos campos de nome do candidato e cônjuge do Formulário Adaptável.

No snippet de código, estamos definindo os itens de WorkingMembers, que é um componente de caixa de seleção. A matriz para os itens está sendo construída dinamicamente buscando os valores dos campos de texto do candidatoName e do cônjuge dos formulários adaptativos

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

Os dados apresentados são os seguintes

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**Adicionar itens usando o editor de regras**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Adicionar itens usando o editor de código**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Para experimentar isso no sistema:

**Uso do editor de código para adicionar itens**

* [Baixar os ativos](assets/usingthecodeeditor.zip)
* [Abrir Forms E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em &quot;Criar | Carregamento de arquivo&quot; e carregue o arquivo que você baixou na etapa anterior
* [Pré-visualização dos formulários](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Digite o nome do candidato e selecione o status civil para casar
* Insira o nome do cônjuge
* Clique em Avançar
* Você deverá ver a caixa de seleção preenchida com o nome do candidato e com o nome do cônjuge se o estado civil for casado

**Uso do editor visual para adicionar itens**

* [Baixar os ativos](assets/usingthevisualeditor.zip)
* Instale o Tomcat se você ainda não o tiver. [Instruções para instalar o tomcat estão disponíveis aqui](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Implantar arquivo SampleRest.war no Tomcat](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Abrir Forms E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em &quot;Criar | Carregamento de arquivo&quot; e carregue o arquivo que você baixou na etapa anterior
* [Pré-visualização dos formulários](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Informe a quantia do empréstimo e a guia para fora do campo. Isso acionará a regra que exibe o campo do período do empréstimo.
* Selecionar o período de empréstimo apropriado(Os itens do período de empréstimo são preenchidos a partir da chamada restante)
* Selecione a taxa de juros e clique em &quot;Obter agendamento de amamentação&quot;
* A tabela de amortização deve ser preenchida. O agendamento de amortização é obtido usando uma chamada REST.

>[!NOTE]
> Pressupõe-se que o tomcat esteja funcionando no porto 8080 e AEM no porto 4502
