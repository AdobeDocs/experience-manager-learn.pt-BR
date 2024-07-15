---
title: Tutorial para adicionar tags de metadados especificadas pelo usuário
description: Saiba como armazenar e recuperar dados de formulário adaptáveis da conta de armazenamento do Azure.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 32
exl-id: 94454327-86d9-468e-9f08-50b8a9c530f3
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 1%

---

# Estender componentes do grupo de opções

Os componentes principais do grupo de caixas de seleção, lista suspensa e botão de opção foram estendidos para incluir uma guia Propriedades adicionais. A guia de propriedades adicionais tem uma caixa de seleção para indicar se o campo deve ser usado como uma guia de índice de blob
![propriedades-adicionais](assets/drop-down-additonal-properties.png). Quando a caixa de seleção é selecionada, uma propriedade chamada Pesquisável é criada e seu valor é definido como verdadeiro no repositório jcr, como mostrado na seguinte captura de tela
![pesquisável](assets/searchable-true.png).

O seguinte .content.xml foi criado na pasta _cq_dialog.

![exibição de projeto suspensa](assets/drop-down-project-view.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
          jcr:primaryType="nt:unstructured"
          jcr:title="Check box group"
          sling:resourceType="cq/gui/components/authoring/dialog">
    <content
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container">
        <items jcr:primaryType="nt:unstructured">
            <tabs
                    jcr:primaryType="nt:unstructured"
                    sling:resourceType="granite/ui/components/coral/foundation/tabs"
                    maximized="{Boolean}false">
                <items jcr:primaryType="nt:unstructured">

                    <properties
                            jcr:primaryType="nt:unstructured"
                            jcr:title="Additional Properties"
                            sling:resourceType="granite/ui/components/coral/foundation/container"
                            margin="{Boolean}true">
                        <items jcr:primaryType="nt:unstructured">
                            <columns
                                    jcr:primaryType="nt:unstructured"
                                    sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                    margin="{Boolean}true">
                                <items jcr:primaryType="nt:unstructured">
                                    <column
                                            jcr:primaryType="nt:unstructured"
                                            sling:resourceType="granite/ui/components/coral/foundation/container">
                                        <items jcr:primaryType="nt:unstructured">
                                            <Searchable
                                                    jcr:primaryType="nt:unstructured"
                                                    sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                                    emptyText="Want to include in search?"
                                                    fieldDescription="Indicate if you want to use in search"
                                                    text="Want to use this field in query"
                                                    value="{Boolean}true"
                                                    uncheckedValue="{Boolean}false"

                                                    name="./Searchable"
                                                    checked="{Boolean}false"
                                                    required="{Boolean}false"/>


                                        </items>
                                    </column>
                                </items>
                            </columns>
                        </items>
                    </properties>
                </items>
            </tabs>
        </items>
    </content>
</jcr:root>
```

## Próximas etapas

[Criar configuração do portal do Azure](./create-osgi-configuration.md)
