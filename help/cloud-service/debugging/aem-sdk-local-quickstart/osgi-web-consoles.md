---
title: Depuração do SDK do AEM usando o console da Web OSGi
description: O quickstart local do SDK do AEM tem um console da Web OSGi que fornece uma variedade de informações e introspecções sobre o tempo de execução local do AEM, úteis para entender como seu aplicativo é reconhecido e funciona no AEM.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 486
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 1%

---

# Depuração do SDK do AEM usando o console da Web OSGi

O quickstart local do SDK do AEM tem um console da Web OSGi que fornece uma variedade de informações e introspecções sobre o tempo de execução local do AEM, úteis para entender como seu aplicativo é reconhecido e funciona no AEM.

O AEM fornece vários consoles OSGi, cada um deles fornecendo insights importantes sobre diferentes aspectos do AEM, no entanto, os seguintes são normalmente os mais úteis na depuração do aplicativo.

## Pacotes

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

O console Pacotes é um catálogo dos pacotes OSGi e seus detalhes, implantados no AEM, juntamente com a capacidade ad hoc de iniciá-los e interrompê-los.

O console Pacotes está localizado em:

+ Ferramentas > Operações > Console da Web > OSGi > Pacotes
+ Ou diretamente em: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Clicar em cada pacote fornece detalhes que ajudam na depuração do aplicativo.

+ A validação do pacote OSGi está presente
+ Validar se um pacote OSGi está ativo
+ Determinar se um pacote OSGi tem importações insatisfeitas que o impedem de iniciar

## Componentes

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

O console Componentes é um catálogo de todos os componentes OSGi implantados no AEM e fornece todas as informações sobre eles, desde o ciclo de vida do componente OSGi definido até os serviços OSGi aos quais eles podem fazer referência

O console Componentes está localizado em:

+ Ferramentas > Operações > Console da Web > OSGi > Componentes
+ Ou diretamente em: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

Principais aspectos que ajudam nas atividades de depuração:

+ A validação do pacote OSGi está presente
+ Validar se um pacote OSGi está ativo
+ Determinar se um pacote OSGi tem importações insatisfeitas que o impedem de iniciar
+ Obter o PID do componente para criar configurações OSGi para eles no Git
+ Identificação de valores de propriedade OSGi associados à configuração OSGi ativa

## Modelos sling

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

O console Modelos Sling está localizado em:

+ Ferramentas > Operações > Console da Web > Status > Modelos Sling
+ Ou diretamente em: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

Principais aspectos que ajudam nas atividades de depuração:

+ A validação de modelos do Sling está registrada no tipo de recurso correto
+ A validação de modelos do Sling é adaptável a partir dos objetos corretos (Resource ou SlingHttpRequestServlet)
+ Validação de exportadores de modelo do Sling estão registrados corretamente
