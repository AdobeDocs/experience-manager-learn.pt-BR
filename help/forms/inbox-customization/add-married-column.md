---
title: Personalização da caixa de entrada
description: Adicionar colunas personalizadas para exibir dados adicionais do fluxo de trabalho
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 2%

---


# Adicionar colunas personalizadas

Para exibir dados de fluxo de trabalho na caixa de entrada, precisamos definir e preencher variáveis no fluxo de trabalho. O valor da variável precisa ser definido antes que uma tarefa seja atribuída a um usuário. Para fornecer um start principal, fornecemos um fluxo de trabalho de amostra pronto para ser implantado no servidor AEM.

* [Login no AEM](http://localhost:4502/crx/de/index.jsp)
* [Importar o fluxo de trabalho de revisão](assets/review-workflow.zip)
* [Revisar o fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Esse fluxo de trabalho tem duas variáveis definidas (isCasried e renda) e seus valores são definidos usando o componente variável set. Essas variáveis serão disponibilizadas como colunas a serem adicionadas a AEM caixa de entrada

## Criar serviço

Para cada coluna que precisamos exibir em nossa caixa de entrada, precisaríamos escrever um serviço. O serviço a seguir permite adicionar uma coluna para exibir o valor da variável isMarried

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>É necessário incluir AEM 6.5.5 Uber.jar no seu projeto para que o código acima funcione

![uber-jar](assets/uber-jar.PNG)

## Teste no servidor

* [Fazer logon no console da Web AEM](http://localhost:4502/system/console/bundles)
* [Pacote de personalização da caixa de entrada de implantação e start](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Abrir sua caixa de entrada](http://localhost:4502/aem/inbox)
* Abra o Admin Control clicando no ícone de Visualização _da_ Lista ao lado do botão _Criar_
* Adicionar coluna Casado à Caixa de entrada e salvar suas alterações
* [Ir para a interface do usuário do FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importe o formulário](assets/snap-form.zip) de amostra selecionando Carregar __ arquivo no menu _Criar_
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecione o status _civil_ e envie o formulário
   [Caixa de entrada visualização](http://localhost:4502/aem/inbox)

O envio do formulário acionará o fluxo de trabalho e uma tarefa será atribuída ao usuário &quot;admin&quot;. Você deve ver um valor na coluna Casado, como mostrado nesta captura de tela

![coluna casada](assets/married-column.PNG)
