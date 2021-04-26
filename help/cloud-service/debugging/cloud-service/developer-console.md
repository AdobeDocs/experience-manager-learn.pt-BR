---
title: Console do desenvolvedor
description: O AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço de AEM em execução que são úteis na depuração.
feature: Ferramentas do desenvolvedor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Desenvolvimento
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: 048a37a9813e7b61ff069c4606b8d23cc6b6844f
workflow-type: tm+mt
source-wordcount: '1351'
ht-degree: 0%

---


# Depuração do AEM como um Cloud Service com o Console do desenvolvedor

O AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço de AEM em execução que são úteis na depuração.

Cada AEM como um ambiente Cloud Service tem seu próprio Console do desenvolvedor.

## Acesso ao Console do desenvolvedor

Para acessar e usar o Console do desenvolvedor, as seguintes permissões devem ser fornecidas ao Adobe ID do desenvolvedor por meio do [Adobe via Admin Console](https://adminconsole.adobe.com).

1. Certifique-se de que a Organização do Adobe que afetou o Cloud Manager e o AEM como produtos do Cloud Service esteja ativa no seletor de Organização do Adobe.
1. O desenvolvedor deve ser membro do __Developer - Cloud Service__ Perfil de produto do Cloud Manager.
   + Se essa associação não existir, o desenvolvedor não poderá fazer logon no Console do desenvolvedor.
1. O desenvolvedor deve ser membro do __AEM Usuários__ ou __AEM Administradores__ Perfil de Produto no Autor e/ou Publicação do AEM.
   + Se essa associação não existir, os despejos de [status](#status) expirarão com um erro 401 Não autorizado.

### Solução de problemas do acesso ao Console do desenvolvedor

#### 401 Erro não autorizado quando do estatuto de dumping

![Console do desenvolvedor - 401 Não autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se for reportado um erro 401 Unauthorized com qualquer status de dumping , significa que o usuário ainda não existe com as permissões necessárias no AEM como Cloud Service ou que os tokens de login usados são inválidos ou expiraram.

Para resolver o problema 401 Não autorizado:

1. Certifique-se de que o usuário é membro do Perfil de produto do Adobe IMS apropriado (Administradores AEM ou Usuários AEM) para o AEM associado do Console do desenvolvedor como uma instância do Cloud Service Product.
   + Lembre-se de que o Console do desenvolvedor acessa 2 Instâncias do produto Adobe IMS; As instâncias de produto AEM as a Cloud Service Author and Publish , portanto, certifique-se de que os Perfis de produto corretos sejam usados, dependendo de qual camada de serviço requer acesso por meio do Console do desenvolvedor.
1. Faça logon no AEM como um Cloud Service (Autor ou Publicação) e verifique se o usuário e os grupos foram sincronizados corretamente no AEM.
   + O Console do desenvolvedor requer que o registro do usuário seja criado na camada de serviço de AEM correspondente para ser autenticado nessa camada de serviço.
1. Limpe os cookies dos navegadores, bem como o estado do aplicativo (armazenamento local) e faça logon novamente no Console do desenvolvedor, garantindo que o token de acesso que o Console do desenvolvedor está usando esteja correto e expirado.

## Pod

Os AEM como um Cloud Service Author e Publish são compostos de várias instâncias, respectivamente, para lidar com a variabilidade do tráfego e atualizações contínuas sem tempo de inatividade. Essas instâncias são chamadas de Pods. A seleção de pod no Console do desenvolvedor define o escopo dos dados que serão expostos por meio de outros controles.

![Console do desenvolvedor - Pod](./assets/developer-console/pod.png)

+ Um pod é uma instância discreta que faz parte de um Serviço de AEM (Autor ou Publicação)
+ Os pods são transitórios, o que significa AEM quando um Cloud Service cria e os destrói conforme necessário
+ Somente os pods que fazem parte do AEM associado como um ambiente Cloud Service são listados no alternador de pods do Console do desenvolvedor.
+ Na parte inferior do alternador de pods, as opções de conveniência permitem selecionar Pods por tipo de serviço:
   + Todos os autores
   + Todos os editores
   + Todas as instâncias

## Status

O status fornece opções para a saída de um estado de tempo de execução AEM específico no texto ou na saída JSON. O Console do desenvolvedor fornece informações semelhantes ao console da Web OSGi do início rápido local do SDK AEM, com a diferença marcada de que o Console do desenvolvedor é somente leitura.

![Console do desenvolvedor - Status](./assets/developer-console/status.png)

### Pacotes

Pacotes lista todos os pacotes OSGi no AEM. Essa funcionalidade é semelhante a [AEM Pacotes OSGi do início rápido local do SDK](http://localhost:4502/system/console/bundles) em `/system/console/bundles`.

Pacotes ajudam na depuração por:

+ Listando todos os pacotes OSGi implantados no AEM como um serviço
+ Listando o estado de cada pacote OSGi; incluindo se estiverem ativos ou não
+ Fornecer detalhes sobre dependências não resolvidas que façam com que os pacotes OSGi se tornem ativos

### Componentes

Os componentes listam todos os componentes OSGi no AEM. Essa funcionalidade é semelhante aos [AEM Componentes OSGi do início rápido local do SDK](http://localhost:4502/system/console/components) em `/system/console/components`.

Os componentes ajudam na depuração por:

+ Listando todos os componentes OSGi implantados em AEM como um Cloud Service
+ Fornecer o estado de cada componente OSGi; incluindo se estiverem ativos ou insatisfeitos
+ Fornecer detalhes sobre referências de serviço insatisfeitas pode fazer com que os componentes OSGi fiquem ativos
+ Listando propriedades OSGi e seus valores vinculados ao componente OSGi

### Configurações

As configurações listam todas as configurações do componente OSGi (propriedades e valores do OSGi). Essa funcionalidade é semelhante ao [AEM Gerenciador de Configuração do OSGi do SDK ](http://localhost:4502/system/console/configMgr) em `/system/console/configMgr`.

Ajuda das configurações na depuração por:

+ Listando propriedades OSGi e seus valores por componente OSGi
+ Localização e identificação de propriedades mal configuradas

### Índices Oak

Os Índices Oak fornecem um despejo dos nós definidos abaixo de `/oak:index`. Lembre-se de que isso não mostra índices mesclados, o que ocorre quando um índice de AEM é modificado.

Ajuda dos Índices Oak na depuração por:

+ Listando todas as definições do índice Oak fornecendo insights de como as consultas de pesquisa são executadas em AEM. Lembre-se de que os índices modificados em AEM não são refletidos aqui. Essa exibição só é útil para índices fornecidos exclusivamente por AEM ou fornecidos exclusivamente pelo código personalizado.

### Serviços OSGi

Componentes lista todos os serviços OSGi. Essa funcionalidade é semelhante a [AEM Serviços OSGi do SDK do início rápido local](http://localhost:4502/system/console/services) em `/system/console/services`.

Ajuda dos Serviços OSGi na depuração por:

+ Listar todos os serviços OSGi no AEM, juntamente com seu pacote OSGi, e todos os pacotes OSGi que o consomem

### Tarefas de arremesso

O Sling Jobs lista todas as filas do Sling Jobs. Essa funcionalidade é semelhante a [AEM trabalhos de inicialização rápida local do SDK](http://localhost:4502/system/console/slingevent) em `/system/console/slingevent`.

Ajuda do Sling Jobs na depuração ao:

+ Listagem de filas de trabalhos do Sling e suas configurações
+ Fornecer insights sobre o número de trabalhos do Sling ativos, enfileirados e processados, o que é útil para depurar problemas com o Fluxo de trabalho, o Fluxo de trabalho transitório e outros trabalhos executados pelos Trabalhos do Sling no AEM.

## Pacotes Java

Os pacotes Java permitem verificar se um pacote Java, e a versão, estão disponíveis para uso no AEM como um Cloud Service. Essa funcionalidade é igual a [AEM Localizador de dependência do início rápido local do SDK](http://localhost:4502/system/console/depfinder) em `/system/console/depfinder`.

![Console do desenvolvedor - Pacotes Java](./assets/developer-console/java-packages.png)

Os pacotes Java são usados para solucionar problemas de pacotes que não estão sendo iniciados por causa de importações não resolvidas ou classes não resolvidas em scripts (HTL, JSP etc). Se os pacotes Java não relatarem nenhum pacote exportarem um pacote Java (ou a versão não corresponder àquela importada por um pacote OSGi):

+ Certifique-se de que a versão da dependência do AEM API do projeto corresponde à versão da versão da AEM do ambiente (e, se possível, atualize tudo para a mais recente).
+ Se as dependências de Maven extras forem usadas no projeto Maven
   + Determine se uma API alternativa fornecida pela dependência da API do SDK AEM pode ser usada.
   + Se a dependência extra for necessária, assegure-se de que ela seja fornecida como um pacote OSGi (em vez de um Jar simples) e esteja incorporada ao pacote de código do seu projeto, (`ui.apps`), de forma semelhante à forma como o pacote OSGi principal é incorporado no pacote `ui.apps`.

## Servlets

Os servlets são usados para fornecer informações sobre como o AEM resolve um URL para um servlet ou script Java (HTL, JSP) que, em última análise, lida com a solicitação. Essa funcionalidade é igual a [AEM SDK do Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) local do quickstart em `/system/console/servletresolver`.

![Console do desenvolvedor - Servlets](./assets/developer-console/servlets.png)

Os servlets ajudam na depuração a determinar:

+ Como um URL é decomposto em suas partes endereçáveis (recurso, seletor, extensão).
+ Para qual servlet ou script um URL resolve, ajudando a identificar URLs mal formados ou servlets/scripts mal registrados.

## Queries

As consultas ajudam a fornecer informações sobre o que e como as consultas de pesquisa são executadas no AEM. Essa funcionalidade é igual ao console [AEM do início rápido local do SDK Tools > Query Performance ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html).

As consultas só funcionam quando um pod específico é selecionado, pois abre o console da Web de Desempenho da Consulta desse pod, exigindo que o desenvolvedor tenha acesso para fazer logon no serviço de AEM.

![Console do Desenvolvedor - Consultas - Explicar Consulta](./assets/developer-console/queries__explain-query.png)

As consultas ajudam na depuração ao:

+ Explicando como as consultas são interpretadas, analisadas e executadas pelo Oak. Isso é muito importante ao rastrear por que um query está lento e entender como ele pode ser acelerado.
+ Listando as consultas mais populares em execução no AEM, com a capacidade de Explicá-las.
+ Listando as consultas mais lentas em execução no AEM, com a capacidade de Explicá-las.
