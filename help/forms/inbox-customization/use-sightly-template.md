---
title: Uso do modelo sightly para exibir os dados da caixa de entrada
description: Adicionar colunas personalizadas para exibir dados adicionais do fluxo de trabalho usando um modelo simples
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 78
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Uso do modelo sightly para exibir os dados da caixa de entrada

Você pode usar o modelo sightly para formatar os dados a serem exibidos nas colunas da caixa de entrada. Neste exemplo, exibiremos ícones de coral-ui dependendo do valor da coluna de renda. A captura de tela a seguir mostra o uso de ícones na coluna de receita
![ícones de renda](assets/income-column.PNG)

[O modelo sightly](assets/sightly-template.zip) usado para exibir os ícones personalizados da interface do coral é fornecido como parte deste artigo.

## Modelo do Sightly

A seguir está o template sightly. O código no modelo exibe o ícone dependendo da renda. Os ícones estão disponíveis como parte da [biblioteca de ícones da interface do coral](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) isso vem com AEM.

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

## Implementação de serviço

O código a seguir é a implementação do serviço para exibir a coluna de receita.

A linha 12 associa a coluna ao modelo sightly

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

## Testar no servidor

>[!NOTE]
>
>Este artigo supõe que você tenha instalado o [exemplo de fluxo de trabalho](assets/review-workflow.zip) e [exemplo de formulário](assets/snap-form.zip) de [artigo anterior](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html) nesta série.

* [Fazer logon no crx como usuário administrador](http://localhost:4502/crx/de/index.jsp)
* [importar modelo do sightly](assets/sightly-template.zip)
* [Fazer logon no console da Web do AEM](http://localhost:4502/system/console/bundles)
* [Implantar e iniciar o pacote de personalização da caixa de entrada](assets/income-column-customization.jar)
* [Abra sua caixa de entrada](http://localhost:4502/aem/inbox)
* Abra o Admin Control clicando em Exibição de lista ao lado do botão Criar
* Adicione a coluna de renda à Caixa de entrada e salve as alterações
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecione o _estado civil_ e enviar o formulário
* [Exibir caixa de entrada](http://localhost:4502/aem/inbox)

O envio do formulário acionará o fluxo de trabalho e uma tarefa será atribuída ao usuário &quot;administrador&quot;. Você deve ver o ícone apropriado abaixo da coluna de renda
