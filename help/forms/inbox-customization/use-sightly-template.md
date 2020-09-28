---
title: Personalização da caixa de entrada
description: Adicione colunas personalizadas para exibir dados adicionais do fluxo de trabalho usando modelo inteligente
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 2%

---

# Uso de modelo inteligente para exibir dados da caixa de entrada

Você pode usar um modelo simples para formatar os dados que serão exibidos nas colunas da caixa de entrada. Neste exemplo, exibiremos ícones coral-ui dependendo do valor da coluna de renda. A captura de tela a seguir mostra o uso de ícones na coluna![renda dos ícones de renda](assets/income-column.PNG)

[O modelo](assets/sightly-template.zip) sightly usado para exibir os ícones personalizados de interface do usuário do coral é fornecido como parte deste artigo.

## Modelo Sightly

Veja a seguir o modelo suave. O código no modelo exibe o ícone dependendo da renda. Os ícones estão disponíveis como parte da biblioteca [de ícones de](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) interface de usuário de coral fornecida com AEM.

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

O código a seguir é a implementação do serviço para exibir a coluna de renda.

A linha 12 associa a coluna ao modelo com brilho

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

## Teste no servidor

>[!NOTE]
>
>Este artigo supõe que você tenha instalado o fluxo de trabalho [de](assets/review-workflow.zip) amostra e o formulário [de](assets/snap-form.zip) amostra do artigo [](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md) anterior nesta série.

* [Logon no crx como usuário administrador](http://localhost:4502/crx/de/index.jsp)
* [importar modelo inteligente](assets/sightly-template.zip)
* [Fazer logon no console da Web AEM](http://localhost:4502/system/console/bundles)
* [Pacote de personalização da caixa de entrada de implantação e start](assets/income-column-customization.jar)
* [Abrir sua caixa de entrada](http://localhost:4502/aem/inbox)
* Abra o Admin Control clicando na Visualização da Lista ao lado do botão Criar
* Adicionar coluna de renda à Caixa de entrada e salvar suas alterações
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecione o status _civil_ e envie o formulário
* [Caixa de entrada visualização](http://localhost:4502/aem/inbox)

O envio do formulário acionará o fluxo de trabalho e uma tarefa será atribuída ao usuário &quot;admin&quot;. Você deve ver o ícone apropriado na coluna de renda
