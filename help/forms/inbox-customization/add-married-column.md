---
title: Adicionar colunas personalizadas
description: Adicionar colunas personalizadas para exibir dados adicionais do fluxo de trabalho
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 2%

---

# Adicionar colunas personalizadas

Para exibir os dados do workflow na caixa de entrada, precisamos definir e preencher variáveis no workflow. O valor da variável precisa ser definido antes que uma tarefa seja atribuída a um usuário. Para você ter uma vantagem inicial, fornecemos um fluxo de trabalho de amostra pronto para ser implantado em seu servidor AEM.

* [Fazer logon no AEM](http://localhost:4502/crx/de/index.jsp)
* [Importar o fluxo de trabalho de revisão](assets/review-workflow.zip)
* [Revisar o fluxo de trabalho](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Este workflow tem duas variáveis definidas (isMarried e revenue) e seus valores são definidos usando o componente set variable. Essas variáveis são disponibilizadas como colunas para serem adicionadas à caixa de entrada do AEM

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
>É necessário incluir AEM 6.5.5 Uber.jar em seu projeto para que o código acima funcione

![uber-jar](assets/uber-jar.PNG)

## Testar no servidor

* [Fazer logon no console da Web do AEM](http://localhost:4502/system/console/bundles)
* [Implantar e iniciar o pacote de personalização da caixa de entrada](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Abra sua caixa de entrada](http://localhost:4502/aem/inbox)
* Abra o Admin Control clicando em _Exibição de lista_ ícone ao lado de _Criar_ botão
* Adicione a coluna Casado à Caixa de entrada e salve as alterações
* [Ir para a interface do FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importar o formulário de amostra](assets/snap-form.zip) selecionando _Upload de arquivo_ de _Criar_ menu
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Selecione o _estado civil_ e enviar o formulário
   [exibir caixa de entrada](http://localhost:4502/aem/inbox)

O envio do formulário acionará o fluxo de trabalho e uma tarefa será atribuída ao usuário &quot;administrador&quot;. Você deve ver um valor na coluna Casado, como mostrado nesta captura de tela

![coluna-casada](assets/married-column.PNG)

## Próximas etapas

[Exibir Coluna Casada](./use-sightly-template.md)
