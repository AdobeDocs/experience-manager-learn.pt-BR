---
title: Adicionar itens ao componente de grupo de opções
description: Adicionar itens ao componente de grupo de opções dinamicamente
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Adicionar itens dinamicamente ao componente de grupo de escolha

O AEM Forms 6.5 introduziu a capacidade de adicionar itens dinamicamente a um componente de grupo de escolha do Adaptive Forms, como CheckBox, botão de opção e lista de imagens.


Você pode adicionar itens usando o editor visual, bem como o editor de códigos, dependendo do seu caso de uso.

**Usando o editor visual:** Você pode preencher os itens do grupo de escolha a partir dos resultados de uma chamada de função ou chamada de serviço. Por exemplo, é possível definir os itens do grupo de opções consumindo a resposta de uma chamada de API REST.

Na captura de tela abaixo, estamos definindo as opções de Período de empréstimo (anos) para os resultados de uma chamada de serviço chamada getLoanPeriods.

![Editor de regras](assets/ruleeditor.png)

**Uso do editor de código**: Quando quiser definir os itens no grupo de opções dinamicamente com base nos valores inseridos no formulário. Por exemplo, o trecho de código a seguir define os itens da caixa de seleção de acordo com os valores inseridos nos campos Nome do requerente e cônjuge do Formulário adaptativo.

No snippet do código, estamos configurando os itens de membros de trabalho, que é um componente de caixa de seleção. A matriz para os itens está sendo construída dinamicamente buscando os valores dos campos de texto RequerName e do cônjuge dos formulários adaptáveis

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

**Adição de itens usando o editor de código**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Para experimentar isso em seu sistema:

**Uso do editor de códigos para adicionar itens**

* [Baixar os ativos](assets/usingthecodeeditor.zip)
* [Abrir Forms E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em &quot;Criar | Upload de arquivo&quot; e faça upload do arquivo baixado na etapa anterior
* [Visualizar formulários](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Insira o Nome do Candidato e selecione o Status do Marital para Casar
* Insira o nome do cônjuge
* Clique em Avançar
* Você deve ver a caixa de seleção preenchida com o nome do candidato e com o nome do cônjuge se o estado civil for casado

**Usar o editor visual para adicionar itens**

* [Baixar os ativos](assets/usingthevisualeditor.zip)
* Instale o Tomcat se ainda não o tiver. [Instruções para instalar o tomcat estão disponíveis aqui](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Implante o arquivo SampleRest.war contido neste arquivo zip em seu Tomcat](assets/sample-rest.zip)
* [Abrir Forms E Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Clique em &quot;Criar | Upload de arquivo&quot; e faça upload do arquivo baixado na etapa anterior
* [Visualizar formulários](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Insira Loan amount e tab fora do campo. Isso acionará a regra que exibe o campo do período de empréstimo.
* Selecionar o período de empréstimo apropriado (Os itens para o período de empréstimo são preenchidos a partir do chamado &quot;restante&quot;)
* Selecione a taxa de juros e clique em &quot;Obter Programação de Amortização&quot;
* A tabela de amortização deve ser preenchida. O agendamento de amortização é obtido usando uma chamada REST.

>[!NOTE]
> Pressupõe-se que o tomcat esteja funcionando na porta 8080 e AEM na porta 4502
