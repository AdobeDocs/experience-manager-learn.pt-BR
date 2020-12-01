---
title: Depuração AEM SDK usando o console da Web OSGi
description: O Início rápido local do SDK do AEM tem um console da Web OSGi que fornece várias informações e introspecções no tempo de execução AEM local, úteis para entender como seu aplicativo é reconhecido e funciona dentro do AEM.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 1%

---


# Depuração AEM SDK usando o console da Web OSGi

O Início rápido local do SDK do AEM tem um console da Web OSGi que fornece várias informações e introspecções no tempo de execução AEM local, úteis para entender como seu aplicativo é reconhecido e funciona dentro do AEM.

AEM fornece vários consoles OSGi, cada um fornecendo insights importantes sobre diferentes aspectos da AEM, no entanto, os seguintes são normalmente os mais úteis na depuração do seu aplicativo.

## Pacotes

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

O console Pacotes é um catálogo dos pacotes OSGi, e seus detalhes, implantados no AEM, juntamente com a capacidade ad hoc de start e pará-los.

O console Pacotes está localizado em:

+ Ferramentas > Operações > Console da Web > OSGi > Pacotes
+ Ou diretamente em: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Clicar em cada pacote fornece detalhes que ajudam na depuração do aplicativo.

+ A validação do pacote OSGi está presente
+ Validando se um pacote OSGi está ativo
+ Determinar se um pacote OSGi possui importações insatisfeitas que impedem o seu início

## Componentes

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

O console Componentes é um catálogo de todos os componentes OSGi implantados para AEM e fornece todas as informações sobre eles, desde seu ciclo de vida do componente OSGi definido até os serviços OSGi aos quais eles podem se referir

O console Componentes está localizado em:

+ Ferramentas > Operações > Console da Web > OSGi > Componentes
+ Ou diretamente em: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Principais aspectos que ajudam nas atividades de depuração:

+ A validação do pacote OSGi está presente
+ Validando se um pacote OSGi está ativo
+ Determinar se um pacote OSGi possui importações insatisfeitas que impedem o seu início
+ Obtendo o PID do componente, para criar configurações OSGi para eles no Git
+ Identificação de valores de propriedade OSGi vinculados à configuração OSGi ativa

## Modelos Sling

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

O console Sling Models está localizado em:

+ Ferramentas > Operações > Console da Web > Status > Modelos Sling
+ Ou diretamente em: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Principais aspectos que ajudam nas atividades de depuração:

+ A validação de modelos Sling está registrada no tipo de recurso adequado
+ A validação de modelos Sling é adaptável a partir dos objetos corretos (Resource ou SlingHttpRequestServlet)
+ A validação dos exportadores do modelo Sling está devidamente registrada
