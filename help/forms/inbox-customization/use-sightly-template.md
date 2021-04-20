---
title: Personalização da caixa de entrada
description: Adicionar colunas personalizadas para exibir dados adicionais de fluxo de trabalho usando modelo sightly
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 4%

---

# Uso de modelo inteligente para exibir dados da caixa de entrada

Você pode usar um modelo sightly para formatar os dados que devem ser exibidos nas colunas da caixa de entrada. Neste exemplo, exibiremos ícones de coral-ui dependendo do valor da coluna de renda. A captura de tela a seguir mostra o uso de ícones na coluna renda
![ícones de rendimento](assets/income-column.PNG)

[O ](assets/sightly-template.zip) modelo sightly usado para exibir os ícones personalizados da interface do usuário do coral é fornecido como parte deste artigo.

## Modelo Sightly

A seguir, o modelo sightly. O código no modelo exibe um ícone dependendo da renda. Os ícones estão disponíveis como parte da [biblioteca de ícones da interface do coral](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) que vem com o AEM.

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## Implementação do serviço

O código a seguir é a implementação de serviço para exibir a coluna de renda.

A linha 12 associa a coluna ao modelo com sightly

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## Testar em seu servidor

>[!NOTE]
>
>Este artigo supõe que você tenha instalado o [workflow de amostra](assets/review-workflow.zip) e [formulário de amostra](assets/snap-form.zip) de [artigo anterior](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md) nesta série.

* [Faça logon no crx como usuário administrador](http://localhost:4502/crx/de/index.jsp)
* [importar modelo sightly](assets/sightly-template.zip)
* [Faça logon no console da Web do AEM](http://localhost:4502/system/console/bundles)
* [Implantar e iniciar o pacote de personalização da caixa de entrada](assets/income-column-customization.jar)
* [Abrir sua caixa de entrada](http://localhost:4502/aem/inbox)
* Abra o Controle de administrador clicando em Exibição de lista ao lado do botão Criar
* Adicionar coluna de renda à Caixa de entrada e salvar suas alterações
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecione o _estado civil_ e envie o formulário
* [Exibir caixa de entrada](http://localhost:4502/aem/inbox)

O envio do formulário acionará o fluxo de trabalho e uma tarefa será atribuída ao usuário &quot;administrador&quot;. Você deve ver o ícone apropriado abaixo da coluna de renda
