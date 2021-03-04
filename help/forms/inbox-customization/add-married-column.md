---
title: Personalização da caixa de entrada
description: Adicionar colunas personalizadas para exibir dados adicionais do fluxo de trabalho
feature: Formulários adaptáveis
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Desenvolvimento
role: Desenvolvedor
level: Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 3%

---


# Adicionar colunas personalizadas

Para exibir dados de fluxo de trabalho na caixa de entrada, precisamos definir e preencher variáveis no fluxo de trabalho. O valor da variável precisa ser definido antes que uma tarefa seja atribuída a um usuário. Para dar a você um início importante, fornecemos um fluxo de trabalho de amostra que está pronto para ser implantado em seu servidor AEM.

* [Faça logon no AEM](http://localhost:4502/crx/de/index.jsp)
* [Importe o fluxo de trabalho de revisão](assets/review-workflow.zip)
* [Revisar o fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Esse workflow tem duas variáveis definidas (isCasado e renda) e seus valores são definidos usando o componente de variável definido. Essas variáveis serão disponibilizadas como colunas a serem adicionadas à caixa de entrada do AEM

## Criar serviço

Para cada coluna que precisamos exibir em nossa caixa de entrada, precisaríamos gravar um serviço. O serviço a seguir permite adicionar uma coluna para exibir o valor da variável isMarried

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
>Você precisa incluir o AEM 6.5.5 Uber.jar em seu projeto para que o código acima funcione

![uber-jar](assets/uber-jar.PNG)

## Testar em seu servidor

* [Faça logon no console da Web do AEM](http://localhost:4502/system/console/bundles)
* [Implantar e iniciar o pacote de personalização da caixa de entrada](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Abrir sua caixa de entrada](http://localhost:4502/aem/inbox)
* Abra o Controle de Admin clicando no ícone _Exibição de Lista_ ao lado do botão _Criar_
* Adicionar coluna Casado à Caixa de entrada e salvar suas alterações
* [Ir para a interface do usuário do FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importe o ](assets/snap-form.zip) formulário de amostra selecionando  _File_ Upload no  __ menu Create
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecione o _estado civil_ e envie o formulário
   [exibir caixa de entrada](http://localhost:4502/aem/inbox)

O envio do formulário acionará o fluxo de trabalho e uma tarefa será atribuída ao usuário &quot;administrador&quot;. Você deve ver um valor na coluna Casado , como mostrado nesta captura de tela

![coluna casada](assets/married-column.PNG)
