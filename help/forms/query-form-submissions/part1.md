---
title: Tornar determinados campos pesquisáveis
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas na consulta de envios de formulários armazenados no portal do Azure
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 1fb7ca83-0ba6-48a3-b3d3-079d0ef89245
duration: 32
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 1%

---

# Tornar determinados campos pesquisáveis

Os campos pesquisáveis em um formulário geralmente se referem aos campos dentro do formulário que podem ser usados como critérios para pesquisar ou filtrar dados enviados.
Para o propósito deste caso de uso, os seguintes tipos de campos foram estendidos para torná-los pesquisáveis

* checkboxgroup
* lista suspensa
* botão de opção

Os autores do formulário podem marcar esses tipos de campos como pesquisáveis, conforme mostrado abaixo
![campo-pesquisável](assets/searchable-fields.png)

Os campos foram estendidos criando a seguinte estrutura

![campos estendidos](assets/extend-component.png)

O conteúdo do arquivo .content.xml é o seguinte

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

[Criar manipulador de envio personalizado](./part2.md)
