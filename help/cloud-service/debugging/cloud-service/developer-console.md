---
title: Console do desenvolvedor
description: O AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço AEM em execução, úteis na depuração.
feature: Developer Tools
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1349'
ht-degree: 0%

---


# Depuração do AEM as a Cloud Service com o Console do desenvolvedor

O AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço AEM em execução, úteis na depuração.

Cada ambiente do AEM as a Cloud Service tem seu próprio Console do desenvolvedor.

## Acesso ao Console do desenvolvedor

Para acessar e usar o Console do desenvolvedor, as seguintes permissões devem ser fornecidas à Adobe ID do desenvolvedor por meio do [Admin Console da Adobe](https://adminconsole.adobe.com).

1. Verifique se a Adobe Org que afetou os produtos do Cloud Manager e do AEM as a Cloud Service está ativa no Adobe Org switcher.
1. O desenvolvedor deve ser membro do __Developer - Cloud Service__ Perfil de produto do Cloud Manager Product.
   + Se essa associação não existir, o desenvolvedor não poderá fazer logon no Console do desenvolvedor.
1. O desenvolvedor deve ser membro do Perfil de produto __AEM Administrators__ do AEM Author and Publish Service.
   + Se essa associação não existir, os despejos de [status](#status) expirarão com um erro 401 Não autorizado.

### Solução de problemas do acesso ao Console do desenvolvedor

#### 401 Erro não autorizado quando do estatuto de dumping

![Console do desenvolvedor - 401 Não autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se for reportado qualquer status de dumping , um erro 401 Unauthorized significa que o usuário ainda não existe com as permissões necessárias no AEM as a Cloud Service ou os tokens de logon usados são inválidos ou expiraram.

Para resolver o problema 401 Não autorizado:

1. Certifique-se de que o usuário é membro do Perfil de produto do Adobe IMS apropriado (Administradores do AEM ou Usuários do AEM) para a instância de produto associada do AEM as a Cloud Service do Console do desenvolvedor.
   + Lembre-se de que o Console do desenvolvedor acessa duas Instâncias de produto do Adobe IMS; As instâncias de produto Autor e Publicação do AEM as a Cloud Service , portanto, certifique-se de que os Perfis de produto corretos sejam usados, dependendo de qual camada de serviço requer acesso por meio do Console do desenvolvedor.
1. Faça logon no AEM as a Cloud Service (Autor ou Publicação) e verifique se seu usuário e grupos foram sincronizados corretamente no AEM.
   + O Console do desenvolvedor requer que o registro do usuário seja criado no nível de serviço do AEM correspondente para ser autenticado nesse nível de serviço.
1. Limpe os cookies dos navegadores, bem como o estado do aplicativo (armazenamento local) e faça logon novamente no Console do desenvolvedor, garantindo que o token de acesso que o Console do desenvolvedor está usando esteja correto e expirado.

## Pod

Os serviços de Autor e Publicação do AEM as a Cloud Service são compostos de várias instâncias, respectivamente, para lidar com a variabilidade do tráfego e atualizações contínuas sem tempo de inatividade. Essas instâncias são chamadas de Pods. A seleção de pod no Console do desenvolvedor define o escopo dos dados que serão expostos por meio de outros controles.

![Console do desenvolvedor - Pod](./assets/developer-console/pod.png)

+ Um pod é uma instância discreta que faz parte de um AEM Service (Autor ou Publicação)
+ Os pods são transitórios, o que significa que o AEM as a Cloud Service os cria e os destrói conforme necessário
+ Somente os pods que fazem parte do ambiente associado do AEM as a Cloud Service são listados no alternador de pods do Console do desenvolvedor.
+ Na parte inferior do alternador de pods, as opções de conveniência permitem selecionar Pods por tipo de serviço:
   + Todos os autores
   + Todos os editores
   + Todas as instâncias

## Status

O status fornece opções para a saída de um estado específico de tempo de execução do AEM em texto ou saída JSON. O Console do desenvolvedor fornece informações semelhantes ao console da Web OSGi do AEM SDK, com a diferença marcada de que o Console do desenvolvedor é somente leitura.

![Console do desenvolvedor - Status](./assets/developer-console/status.png)

### Pacotes

Pacotes lista todos os pacotes OSGi no AEM. Essa funcionalidade é semelhante aos [Pacotes OSGi do AEM SDK de inicialização rápida local](http://localhost:4502/system/console/bundles) em `/system/console/bundles`.

Pacotes ajudam na depuração por:

+ Listando todos os pacotes OSGi implantados no AEM as a Service
+ Listando o estado de cada pacote OSGi; incluindo se estiverem ativos ou não
+ Fornecer detalhes sobre dependências não resolvidas que façam com que os pacotes OSGi se tornem ativos

### Componentes

Os componentes listam todos os componentes OSGi no AEM. Essa funcionalidade é semelhante aos [Componentes OSGi do AEM SDK do início rápido local](http://localhost:4502/system/console/components) em `/system/console/components`.

Os componentes ajudam na depuração por:

+ Listar todos os componentes OSGi implantados no AEM as a Cloud Service
+ Fornecer o estado de cada componente OSGi; incluindo se estiverem ativos ou insatisfeitos
+ Fornecer detalhes sobre referências de serviço insatisfeitas pode fazer com que os componentes OSGi fiquem ativos
+ Listando propriedades OSGi e seus valores vinculados ao componente OSGi

### Configurações

As configurações listam todas as configurações do componente OSGi (propriedades e valores do OSGi). Essa funcionalidade é semelhante ao [Gerenciador de Configuração do OSGi do AEM SDK ](http://localhost:4502/system/console/configMgr) em `/system/console/configMgr`.

Ajuda das configurações na depuração por:

+ Listando propriedades OSGi e seus valores por componente OSGi
+ Localização e identificação de propriedades mal configuradas

### Índices Oak

Os Índices Oak fornecem um despejo dos nós definidos abaixo de `/oak:index`. Lembre-se de que isso não mostra índices mesclados, o que ocorre quando um índice AEM é modificado.

Ajuda dos Índices Oak na depuração por:

+ Listando todas as definições do índice Oak fornecendo insights de como as consultas de pesquisa são executadas no AEM. Lembre-se de que os índices modificados no AEM não são refletidos aqui. Essa exibição é útil apenas para índices fornecidos exclusivamente pelo AEM ou fornecidos apenas pelo código personalizado.

### Serviços OSGi

Componentes lista todos os serviços OSGi. Essa funcionalidade é semelhante aos [Serviços OSGi do AEM SDK do início rápido local](http://localhost:4502/system/console/services) em `/system/console/services`.

Ajuda dos Serviços OSGi na depuração por:

+ Listar todos os serviços OSGi no AEM, juntamente com seu pacote OSGi, e todos os pacotes OSGi que o consomem

### Tarefas de arremesso

O Sling Jobs lista todas as filas do Sling Jobs. Essa funcionalidade é semelhante aos [Trabalhos de início rápido locais do AEM SDK](http://localhost:4502/system/console/slingevent) em `/system/console/slingevent`.

Ajuda do Sling Jobs na depuração ao:

+ Listagem de filas de trabalhos do Sling e suas configurações
+ Fornecer insights sobre o número de trabalhos do Sling ativos, enfileirados e processados, o que é útil para depurar problemas com o Fluxo de trabalho, o Fluxo de trabalho transitório e outros trabalhos executados pelos Trabalhos do Sling no AEM.

## Pacotes Java

Os pacotes Java permitem verificar se um pacote Java, e uma versão, estão disponíveis para uso no AEM as a Cloud Service. Essa funcionalidade é igual ao [Localizador de dependência do início rápido local do AEM SDK](http://localhost:4502/system/console/depfinder) em `/system/console/depfinder`.

![Console do desenvolvedor - Pacotes Java](./assets/developer-console/java-packages.png)

Os pacotes Java são usados para solucionar problemas de pacotes que não estão sendo iniciados por causa de importações não resolvidas ou classes não resolvidas em scripts (HTL, JSP etc). Se os pacotes Java não relatarem nenhum pacote exportarem um pacote Java (ou a versão não corresponder àquela importada por um pacote OSGi):

+ Certifique-se de que a versão da dependência maven da API do AEM do seu projeto corresponda à versão da versão do AEM do ambiente (e, se possível, atualize tudo para a mais recente).
+ Se as dependências de Maven extras forem usadas no projeto Maven
   + Determine se uma API alternativa fornecida pela dependência da API do SDK do AEM pode ser usada.
   + Se a dependência extra for necessária, assegure-se de que ela seja fornecida como um pacote OSGi (em vez de um Jar simples) e esteja incorporada ao pacote de código do seu projeto, (`ui.apps`), de forma semelhante à forma como o pacote OSGi principal é incorporado no pacote `ui.apps`.

## Servlets

Os servlets são usados para fornecer informações sobre como o AEM resolve um URL para um servlet ou script Java (HTL, JSP) que, em última análise, lida com a solicitação. Essa funcionalidade é igual ao Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) local do SDK do AEM [AEM SDK em `/system/console/servletresolver`.

![Console do desenvolvedor - Servlets](./assets/developer-console/servlets.png)

Os servlets ajudam na depuração a determinar:

+ Como um URL é decomposto em suas partes endereçáveis (recurso, seletor, extensão).
+ Para qual servlet ou script um URL resolve, ajudando a identificar URLs mal formados ou servlets/scripts mal registrados.

## Queries

As consultas ajudam a fornecer insights sobre o que e como as consultas de pesquisa são executadas no AEM. Essa funcionalidade é igual ao console [Ferramentas > Desempenho da consulta ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) do quickstart local do AEM SDK.

As consultas só funcionam quando um pod específico é selecionado, pois abre o console da Web de Desempenho da Consulta desse pod, exigindo que o desenvolvedor tenha acesso para fazer logon no serviço AEM.

![Console do Desenvolvedor - Consultas - Explicar Consulta](./assets/developer-console/queries__explain-query.png)

As consultas ajudam na depuração ao:

+ Explicando como as consultas são interpretadas, analisadas e executadas pelo Oak. Isso é muito importante ao rastrear por que um query está lento e entender como ele pode ser acelerado.
+ Listando as consultas mais populares em execução no AEM, com a capacidade de Explicá-las.
+ Listando as consultas mais lentas em execução no AEM, com a capacidade de Explicá-las.
