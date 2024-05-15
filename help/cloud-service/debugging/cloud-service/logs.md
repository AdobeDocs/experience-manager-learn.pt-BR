---
title: Logs
description: Os registros atuam como linha de frente para depurar aplicativos de AEM no AEM as a Cloud Service, mas dependem do registro adequado no aplicativo AEM implantado.
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

Os registros atuam como linha de frente para depurar aplicativos de AEM no AEM as a Cloud Service, mas dependem do registro adequado no aplicativo AEM implantado.

Todas as atividades de log do serviço AEM de um determinado ambiente (Autor, Publicação/Dispatcher de publicação) são consolidadas em um único arquivo de log, mesmo se diferentes pods nesse serviço gerarem as instruções de log.

As IDs de pod são fornecidas em cada declaração de log, permitindo a filtragem ou o agrupamento de declarações de log. As IDs de pod estão no formato de:

+ `cm-p<PROGRAM ID>-e<ENVIRONMENT ID>-aem-<author|publish>-<POD NAME>`
+ Exemplo: `cm-p12345-e56789-aem-author-abcdefabde-98765`

## Arquivos de log personalizados

O AEM as a Cloud Service não é compatível com arquivos de log personalizados, no entanto, ele é compatível com registro personalizado.

Para que os registros Java fiquem disponíveis no AEM as a Cloud Service (via [Cloud Manager](#cloud-manager) ou [CLI do Adobe I/O](#aio)), as declarações de log personalizadas devem ser gravadas no `error.log`. Logs gravados em logs nomeados personalizados, como `example.log`, não poderão ser acessados pelo AEM as a Cloud Service.

Os logs podem ser gravados no `error.log` usando uma propriedade de configuração OSGi do Sling LogManager no `org.apache.sling.commons.log.LogManager.factory.config~example.cfg.json` arquivos.

```
{
   ...
   "org.apache.sling.commons.log.file": "logs/error.log"
   ...
}
```

## Logs do serviço de Autor e Publicação no AEM

Os serviços de Autor e Publicação do AEM fornecem logs do servidor de tempo de execução do AEM:

+ `aemerror` é o log de erros Java (encontrado em `/crx-quickstart/logs/error.log` no SDK do AEM (início rápido local). A seguir estão os [níveis de log recomendados](#log-levels) para logs personalizados por tipo de ambiente:
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemaccess` lista solicitações HTTP para o serviço AEM com detalhes
+ `aemrequest` lista solicitações HTTP feitas ao serviço AEM e suas respostas HTTP correspondentes

## Logs do Dispatcher de publicação do AEM

Somente o Dispatcher de publicação do AEM fornece logs do Apache Web Server e do Dispatcher, pois esses aspectos só existem no nível de publicação do AEM, e não no nível de criação do AEM.

+ `httpdaccess` lista solicitações HTTP feitas ao Apache Web Server/Dispatcher do serviço AEM.
+ `httperror`  lista mensagens de log do Apache Web Server e ajuda com a depuração de módulos Apache compatíveis, como `mod_rewrite`.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`
+ `aemdispatcher` lista mensagens de log dos módulos do Dispatcher, incluindo filtragem e veiculação de mensagens de cache.
   + Desenvolvimento: `DEBUG`
   + Estágio: `WARN`
   + Produção: `ERROR`

## Cloud Manager{#cloud-manager}

O Adobe Cloud Manager permite o download de logs, por dia, por meio da ação Baixar logs de um ambiente.

![Cloud Manager - Baixar logs](./assets/logs/download-logs.png)

Esses registros podem ser baixados e inspecionados por meio de qualquer ferramenta de análise de registros.

## CLI do Adobe I/O com plug-in do Cloud Manager{#aio}

O Adobe Cloud Manager é compatível com o acesso a logs as a Cloud Service do AEM por meio do [CLI do Adobe I/O](https://github.com/adobe/aio-cli) com o [Plug-in do Cloud Manager para a CLI do Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager).

Primeiro, [configurar o Adobe I/O com o plug-in do Cloud Manager](../../local-development-environment/development-tools.md#aio-cli).

Garantir que a ID do programa e a ID do ambiente relevantes tenham sido identificadas e usar [list-available-log-options](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerlist-available-log-options-environmentid) para listar as opções de log usadas para [tail](#aio-cli-tail-logs) ou [baixar](#aio-cli-download-logs) logs.

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

A CLI do Adobe I/O fornece a capacidade de rastrear logs em tempo real do AEM as a Cloud Service usando o [tail-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagertail-log-environmentid-service-name) comando. O rejeito é útil para observar a atividade do registro em tempo real à medida que as ações são executadas no ambiente as a Cloud Service do AEM.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:tail-logs <ENVIRONMENT ID> <SERVICE> <NAME>
```

Outras ferramentas de linha de comando, como `grep` podem ser utilizados em conjunto com `tail-logs` para ajudar a isolar declarações de interesse de log, por exemplo:

```
$ aio cloudmanager:tail-logs 12345 author | grep com.example.MySlingModel
```

... só exibe as instruções de registro geradas a partir de `com.example.MySlingModel` ou conter essa string nelas.

### Download de logs{#aio-cli-download-logs}

A CLI do Adobe I/O fornece a capacidade de fazer download de logs do AEM as a Cloud Service usando o [download-logs](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerdownload-logs-environmentid-service-name-days)). Isso fornece o mesmo resultado final que o download dos logs na interface da Web do Cloud Manager, com a diferença sendo o `download-logs` O comando consolida logs em dias, com base no número de dias de solicitação dos logs.

```
$ aio config:set cloudmanager_programid <PROGRAM ID>
$ aio cloudmanager:download-logs <ENVIRONMENT> <SERVICE> <NAME> <DAYS>
```

## Noções básicas sobre logs

Os registros no AEM as a Cloud Service têm vários pods gravando instruções de registro neles. Como várias instâncias do AEM gravam no mesmo arquivo de log, é importante entender como analisar e reduzir o ruído durante a depuração. Para explicar, o seguinte `aemerror` o trecho de log é usado:

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

As orientações gerais de Adobe sobre níveis de log por ambiente AEM as a Cloud Service são:

+ Desenvolvimento local (SDK do AEM): `DEBUG`
+ Desenvolvimento: `DEBUG`
+ Estágio: `WARN`
+ Produção: `ERROR`

Definir o nível de log mais apropriado para cada tipo de ambiente é com AEM as a Cloud Service, os níveis de log são mantidos no código

+ As configurações de log do Java são mantidas nas configurações do OSGi
+ Níveis de log do Apache Web Server e Dispatcher no projeto do Dispatcher

...e, portanto, exigir uma implantação para alterar.

### Variáveis específicas do ambiente para definir níveis de log Java

Uma alternativa para definir níveis de log Java estáticos conhecidos para cada ambiente é usar AEM como Cloud Service [variáveis específicas do ambiente](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values) para parametrizar os níveis de log, permitindo que os valores sejam alterados dinamicamente por meio da [CLI do Adobe I/O com plug-in do Cloud Manager](#aio-cli).

Isso requer a atualização das configurações de OSGi de registro para usar os espaços reservados para variáveis específicas do ambiente. [Valores padrão](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#default-values) para níveis de log devem ser definidos conforme [recomendações do Adobe](#log-levels). Por exemplo:

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

+ [Um número limitado de variáveis de ambiente é permitido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#number-of-variables)e criar uma variável para gerenciar o nível de log usará um.
+ As variáveis de ambiente podem ser gerenciadas de forma programática via [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html), [CLI do Adobe I/O](https://github.com/adobe/aio-cli-plugin-cloudmanager#aio-cloudmanagerset-environment-variables-environmentid), e [APIs HTTP do Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#cloud-manager-api-format-for-setting-properties).
+ As alterações nas variáveis de ambiente devem ser redefinidas manualmente por uma ferramenta compatível. Esquecer de redefinir um ambiente de alto tráfego, como Produção, para um nível de registro menos detalhado pode inundar os registros e afetar o desempenho do AEM.

_As variáveis específicas do ambiente não funcionam para as configurações de log do Apache Web Server ou Dispatcher, pois não são definidas por meio da configuração OSGi._
