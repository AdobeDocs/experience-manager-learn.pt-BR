---
title: Depuração do SDK do AEM usando o console da Web do OSGi
description: A inicialização rápida local do SDK do AEM tem um console da Web OSGi que fornece uma variedade de informações e introduções no tempo de execução local do AEM, úteis para entender como seu aplicativo é reconhecido pelo e funciona no AEM.
feature: Ferramentas do desenvolvedor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante, Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 3%

---


# Depuração do SDK do AEM usando o console da Web do OSGi

A inicialização rápida local do SDK do AEM tem um console da Web OSGi que fornece uma variedade de informações e introduções no tempo de execução local do AEM, úteis para entender como seu aplicativo é reconhecido pelo e funciona no AEM.

O AEM fornece vários consoles OSGi, cada um fornecendo insights importantes sobre diferentes aspectos do AEM, no entanto, os itens a seguir normalmente são os mais úteis na depuração do seu aplicativo.

## Pacotes

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

O console Pacotes é um catálogo dos pacotes OSGi e seus detalhes, implantados no AEM, juntamente com a capacidade ad hoc de iniciá-los e pará-los.

O console Pacotes está localizado em:

+ Ferramentas > Operações > Console da Web > OSGi > Pacotes
+ Ou diretamente em: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

Clicar em cada pacote fornece detalhes que ajudam na depuração do aplicativo.

+ A validação do pacote OSGi está presente
+ Validando se um pacote OSGi está ativo
+ Determinar se um pacote OSGi tem importações insatisfeitas que o impedem de iniciar

## Componentes

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

O console Componentes é um catálogo de todos os componentes OSGi implantados no AEM e fornece todas as informações sobre eles, desde o ciclo de vida do componente OSGi definido até aos serviços OSGi aos quais eles podem fazer referência

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
