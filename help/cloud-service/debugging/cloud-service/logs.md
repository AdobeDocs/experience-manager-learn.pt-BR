---
title: Logs
description: Os registros atuam como linha de frente para depurar aplicativos de AEM no AEM as a Cloud Service, mas dependem do logon adequado no aplicativo AEM implantado.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5432
thumbnail: kt-5432.jpg
topic: Development
role: Developer
level: Beginner
exl-id: d0bd64bd-9e6c-4a28-a8d9-52bb37b27a09
duration: 229
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '948'
ht-degree: 0%

---

# Depuração do AEM as a Cloud Service usando logs

Os registros atuam como linha de frente para depurar aplicativos de AEM no AEM as a Cloud Service, mas dependem do logon adequado no aplicativo AEM implantado.

Todas as atividades de registro do serviço AEM de um determinado ambiente (Autor, Publish/Publish Dispatcher) são consolidadas em um único arquivo de registro, mesmo que diferentes pods nesse serviço gerem as declarações de registro.

As IDs de pod são fornecidas em cada declaração de log, permitindo a filtragem ou o agrupamento de declarações de log. As IDs de pod estão no formato de:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Exemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Arquivos de log personalizados

O AEM as a Cloud Service não é compatível com arquivos de log personalizados, no entanto, ele é compatível com registro personalizado.

Para que os logs Java fiquem disponíveis no AEM as a Cloud Service (via [Cloud Manager](#cloud-manager) ou [Adobe I/O CLI](#aio)), as instruções de log personalizadas devem ser gravadas em `error.log`. Logs gravados em logs nomeados personalizados, como `example.log`, não serão acessíveis no AEM as a Cloud Service.

Os logs podem ser gravados no `error.log` usando uma propriedade de configuração OSGi Sling LogManager nos arquivos `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` do aplicativo.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Registros de serviço do Publish e do autor do AEM

Os serviços AEM Author e Publish fornecem logs do servidor de tempo de execução do AEM:

+ `aemerror` é o registro de erros Java (encontrado em `/crx-quickstart/logs/error.log` na inicialização rápida local do SDK do AEM). A seguir estão os [níveis de log recomendados](#log-levels) para agentes de log personalizados por tipo de ambiente:
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemaccess` lista solicitações HTTP para o serviço AEM com detalhes
+ `aemrequest` lista as solicitações HTTP feitas ao serviço AEM e suas respostas HTTP correspondentes

## Logs do Publish Dispatcher no AEM

AEM Somente o Publish Dispatcher fornece o servidor Web Apache e os logs do Dispatcher AEM, pois esses aspectos só existem no nível do Publish AEM, e não no nível do Author.

+ `httpdaccess` lista solicitações HTTP feitas ao Apache Web Server/Dispatcher do serviço AEM.
+ `httperror` lista mensagens de log do Apache Web Server e ajuda com a depuração de módulos Apache compatíveis, como `mod_rewrite`.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemdispatcher` lista mensagens de log dos módulos do Dispatcher, incluindo filtragem e veiculação de mensagens do cache.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`

## Cloud Manager{#cloud-manager}

O Adobe Cloud Manager permite o download de logs, por dia, por meio da ação Baixar logs de um ambiente.

![Cloud Manager - Baixar Logs](./assets/logs/download-logs.png)

Esses registros podem ser baixados e inspecionados por meio de qualquer ferramenta de análise de registros.

## CLI do Adobe I/O com plug-in do Cloud Manager{#aio}

O Adobe Cloud Manager dá suporte ao acesso aos logs do AEM as a Cloud Service por meio da [CLI do Adobe I/O](https://github.com/adobe/aio-cli) com o [plug-in do Cloud Manager para a CLI do Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Primeiro, [configure o Adobe I/O com o plug-in do Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Verifique se a ID do Programa e a ID do Ambiente relevantes foram identificadas e use [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) para listar as opções de log usadas para [tail](#aio-cli-tail-logs) ou [baixar](#aio-cli-download-logs) logs.

```
$ aio cloudmanager:list-programs
Program Id Name      Enabled 
14304      Program 1 true    
11454      Program 2 true 
11502      Program 3 true    

$ aio config:set cloudmanager_programid <PROGRAM ID>

$ aio cloudmanager:list-environments        
Environment Id Name            Type  Description 
22295          program-3-dev   dev               
22310          program-3-prod  prod              
22294          program-3-stage stage   

$ aio cloudmanager:list-available-log-options <ENVIRONMENT ID>
Environment Id Service    Name          
22295          author     aemaccess     
22295          author     aemerror      
22295          author     aemrequest    
22295          publish    aemaccess     
22295          publish    aemerror      
22295          publish    aemrequest    
22295          dispatcher httpdaccess   
22295          dispatcher httpderror    
22295          dispatcher aemdispatcher 
```

### Rejeitar logs{#aio-cli-tail-logs}

A CLI do Adobe I/O fornece a capacidade de rastrear logs em tempo real do AEM as a Cloud Service usando o comando [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name). O rejeito é útil para assistir à atividade de log em tempo real, pois as ações são executadas no ambiente do AEM as a Cloud Service.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Outras ferramentas de linha de comando, como o `grep`, podem ser usadas em conjunto com o `tail-logs` para ajudar a isolar declarações de log de interesse, por exemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... só exibe as instruções de log geradas a partir de `com.example.MySlingModel` ou que contêm essa cadeia de caracteres nelas.

### Download de logs{#aio-cli-download-logs}

A CLI do Adobe I/O fornece a capacidade de baixar logs do AEM as a Cloud Service usando o comando [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Isso fornece o mesmo resultado final que o download dos logs na interface da Web do Cloud Manager, com a diferença sendo que o comando `download-logs` consolida os logs em dias, com base no número de dias de solicitação dos logs.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Noções básicas sobre logs

Os logs no AEM as a Cloud Service têm vários pods gravando instruções de log neles. Como várias instâncias do AEM gravam no mesmo arquivo de log, é importante entender como analisar e reduzir o ruído durante a depuração. Para explicar, o seguinte trecho de log `aemerror` é usado:

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp40782847611-87] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

Usando as IDs de pod, o ponto de dados após a data e a hora, os logs podem ser agrupados pelo pod ou instância do AEM dentro do serviço, facilitando o rastreamento e o entendimento da execução do código.

__Pod cm-p12345-e56789-aem-author-abcdefg-1111__

```
01.01.2020 12:00:00.000 [cm-p12345-e56789-aem-author-abcdefg-1111] *DEBUG* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Preparing to collect resources
01.01.2020 12:00:02.003 [cm-p12345-e56789-aem-author-abcdefg-1111] *ERROR* [qtp2078364989-269] com.example.components.impl.ExampleModelImpl Unable to collect any resources
```

__Pod cm-p12345-e56789-aem-author-abcdefg-2222__

```
01.01.2020 12:00:01.002 [cm-p12345-e56789-aem-author-abcdefg-2222] *WARN*  [qtp2078364989-269] com.example.services.impl.ExampleServiceImpl Unable to resolve resource [ /content/example ] to a resource. Aborting.
```

## Níveis de log recomendados{#log-levels}

As orientações gerais sobre Adobe por ambiente do AEM as a Cloud Service são:

+ Desenvolvimento Local (SDK do AEM): `DEBUG`
+ Desenvolvimento: `DEBUG`
+ Estágio: `WARN`
+ Produção: `ERROR`

Ao definir o nível de log mais apropriado para cada tipo de ambiente com o AEM as a Cloud Service, os níveis de log são mantidos no código

+ As configurações de log do Java são mantidas nas configurações do OSGi
+ Níveis de log do Apache Web Server e Dispatcher no projeto do Dispatcher

...e, portanto, exigir uma implantação para alterar.

### Variáveis específicas do ambiente para definir níveis de log Java

Uma alternativa para definir níveis de log Java estáticos conhecidos para cada ambiente é usar AEM Cloud Service como [variáveis específicas do ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) para parametrizar níveis de log, permitindo que os valores sejam alterados dinamicamente por meio da [CLI de Adobe I/O com o plug-in do Cloud Manager](#aio-cli).

Isso requer a atualização das configurações de OSGi de registro para usar os espaços reservados para variáveis específicas do ambiente. [Valores padrão](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) para níveis de log devem ser definidos de acordo com [recomendações de Adobe](#log-levels). Por exemplo:

`/apps/example/config/org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json`

```
{
    ...
    "org.apache.sling.commons.log.names": ["com.example"],
    "org.apache.sling.commons.log.level": "$[env:LOG_LEVEL;default=DEBUG]"
    ...
}
```

Esta abordagem tem desvantagens que devem ser tidas em conta:

+ [Um número limitado de variáveis de ambiente é permitido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables). A criação de uma variável para gerenciar o nível de log usará uma.
+ As variáveis de ambiente podem ser gerenciadas de forma programática por meio do [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), do [Adobe I/O CLI](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid) e das [APIs HTTP do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ As alterações nas variáveis de ambiente devem ser redefinidas manualmente por uma ferramenta compatível. Esquecer de redefinir um ambiente de alto tráfego, como Produção, para um nível de registro menos detalhado pode inundar os registros e afetar o desempenho do AEM.

_As variáveis específicas do ambiente não funcionam para as configurações de log do Dispatcher ou do Apache Web Server, pois elas não são definidas por meio da configuração OSGi._
