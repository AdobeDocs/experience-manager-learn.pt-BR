---
title: Depuração AEM SDK usando o console da Web OSGi
description: O início rápido local do SDK do AEM tem um console da Web OSGi que fornece uma variedade de informações e introduções no tempo de execução AEM local, úteis para entender como o aplicativo é reconhecido pelo e funciona dentro do AEM.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 2%

---

# Depuração AEM SDK usando o console da Web OSGi

O início rápido local do SDK do AEM tem um console da Web OSGi que fornece uma variedade de informações e introduções no tempo de execução AEM local, úteis para entender como o aplicativo é reconhecido pelo e funciona dentro do AEM.

O AEM fornece vários consoles OSGi, cada um fornecendo insights importantes sobre diferentes aspectos do AEM, no entanto, os seguintes são normalmente os mais úteis na depuração do seu aplicativo.

## Pacotes

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

O console Pacotes é um catálogo dos pacotes OSGi e seus detalhes, implantados em AEM, juntamente com a capacidade ad hoc de iniciá-los e pará-los.

O console Pacotes está localizado em:

+ Ferramentas > Operações > Console da Web > OSGi > Pacotes
+ Ou diretamente em: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Clicar em cada pacote fornece detalhes que ajudam na depuração do aplicativo.

+ A validação do pacote OSGi está presente
+ Validando se um pacote OSGi está ativo
+ Determinar se um pacote OSGi tem importações insatisfeitas que o impedem de iniciar

## Componentes

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

O console Componentes é um catálogo de todos os componentes OSGi implantados em AEM e fornece todas as informações sobre eles, desde o ciclo de vida do componente OSGi definido até aos serviços OSGi aos quais eles podem fazer referência

O console Componentes está localizado em:

+ Ferramentas > Operações > Console da Web > OSGi > Componentes
+ Ou diretamente em: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Principais aspectos que ajudam nas atividades de depuração:

+ A validação do pacote OSGi está presente
+ Validando se um pacote OSGi está ativo
+ Determinar se um pacote OSGi tem importações insatisfeitas que o impedem de iniciar
+ Obter o PID do componente, para criar configurações OSGi para eles no Git
+ Identificação de valores de propriedade OSGi vinculados à configuração OSGi ativa

## Modelos sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

O console Modelos do Sling está localizado em:

+ Ferramentas > Operações > Console da Web > Status > Modelos do Sling
+ Ou diretamente em: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Principais aspectos que ajudam nas atividades de depuração:

+ Validar Modelos do Sling são registrados no tipo de recurso adequado
+ Validar Modelos do Sling são adaptáveis a partir dos objetos corretos (Resource ou SlingHttpRequestServlet)
+ A validação dos exportadores do Modelo Sling está devidamente registrada
