---
title: Adicionar itens ao componente do grupo de opções
description: Adicionar itens ao componente do grupo de opções dinamicamente
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 350
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# Adicionar itens dinamicamente ao componente do grupo de opções

O AEM Forms 6.5 introduziu a capacidade de adicionar itens dinamicamente a um componente de grupo de opções do Adaptive Forms, como CheckBox, Botão de opção e Lista de imagens.


É possível adicionar itens usando o editor visual, bem como o editor de código, dependendo do caso de uso.

**Uso do editor visual:** Você pode preencher os itens do grupo de opções a partir dos resultados de uma chamada de função ou de serviço. Por exemplo, você pode definir os itens do grupo de opções consumindo a resposta de uma chamada à API REST.

Na captura de tela abaixo, estamos definindo as opções de Período de empréstimo (anos) para os resultados de uma chamada de serviço chamada getLoanPeriods.

![Editor de regras](assets/ruleeditor.png)

**Uso do editor de código**: quando quiser definir os itens no grupo de opções dinamicamente com base nos valores inseridos no formulário. Por exemplo, o trecho de código a seguir define os itens da caixa de seleção para os valores inseridos nos campos nome do candidato e cônjuge do Formulário adaptável.

No trecho de código, estamos definindo os itens de WorkingMembers, que é um componente de caixa de seleção. A matriz dos itens está sendo criada dinamicamente, buscando os valores dos campos de texto applicationName e spouse dos formulários adaptáveis

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

Para experimentar isso no seu sistema:

**Utilização do editor de código para adicionar itens**

* [Baixar os ativos](assets/usingthecodeeditor.zip)
* [Abrir Forms E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em &quot;Criar | Upload de arquivo&quot; e faça upload do arquivo baixado na etapa anterior
* [Pré-visualizar os formulários](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Informe o Nome do Candidato e selecione o Estado Civil para Casado
* Insira o nome do cônjuge
* Clique em Avançar
* Você deve ver a caixa de seleção preenchida com o nome do candidato e com o nome do cônjuge se o estado civil for casado

**Uso do editor visual para adicionar itens**

* [Baixar os ativos](assets/usingthevisualeditor.zip)
* Instale o Tomcat se você ainda não o tiver. [As instruções para instalar o tomcat estão disponíveis aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Implantar o arquivo SampleRest.war contido neste arquivo zip no Tomcat](assets/sample-rest.zip)
* [Abrir Forms E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em &quot;Criar | Upload de arquivo&quot; e faça upload do arquivo baixado na etapa anterior
* [Pré-visualizar os formulários](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Insira o valor do empréstimo e saia do campo. Isso acionará a regra que exibe o campo loan period.
* Selecionar o período de empréstimo apropriado (os itens do período de empréstimo são preenchidos a partir da chamada restante)
* Selecione a taxa de juros e clique em &quot;Obter Programação de Amortização&quot;
* A tabela de amortização deve ser preenchida. O calendário de amortização é obtido utilizando uma chamada REST.

>[!NOTE]
> Pressupõe-se que o tomcat esteja em execução na porta 8080 e o AEM na porta 4502
