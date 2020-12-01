---
title: Console do desenvolvedor
description: AEM como Cloud Service fornece um Console de desenvolvedor para cada ambiente que expõe vários detalhes do serviço AEM em execução que são úteis na depuração.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
translation-type: tm+mt
source-git-commit: 1af3661e5c18206d58d339d51d5189834e843023
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---


# Depuração de AEM como Cloud Service com o Developer Console

AEM como Cloud Service fornece um Console de desenvolvedor para cada ambiente que expõe vários detalhes do serviço AEM em execução que são úteis na depuração.

Cada AEM como ambiente tem seu próprio Developer Console.

## Acesso ao Developer Console

Para acessar e usar o Console do desenvolvedor, as seguintes permissões devem ser dadas ao Adobe ID do desenvolvedor por meio do Admin Console[Adobe](https://adminconsole.adobe.com).

1. Verifique se a Adobe Org que afetou o Cloud Manager e o AEM como Cloud Service está ativa no switch Adobe Org.
1. O desenvolvedor deve ser membro do __Developer - Cloud Service__ Perfil de produto do Cloud Manager.
   + Se essa associação não existir, o desenvolvedor não poderá fazer logon no Developer Console.
1. O desenvolvedor deve ser membro do Perfil de produto __Administradores AEM__ do AEM Author and Publish service.
   + Se essa associação não existir, o [status](#status) despejo expirará com um erro 401 Unauthorized.

### Solução de problemas de acesso ao Console do desenvolvedor

#### 401 Erro não autorizado quando o estatuto de dumping

![Developer Console - 401 Unauthorized](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se for reportado um erro 401 Unauthorized sendo despejado, isso significa que o usuário ainda não possui as permissões necessárias no AEM como Cloud Service ou que os tokens de login usados são inválidos ou expiraram.

Para resolver o problema 401 Não autorizado:

1. Certifique-se de que o usuário seja membro do Perfil de produto Adobe IMS apropriado (Administradores AEM ou Usuários AEM) para o AEM associado do Developer Console como uma instância do Produto Cloud Service.
   + Lembre-se de que o Console do desenvolvedor acessa Instâncias de produto Adobe IMS 2; as AEM como instâncias de produto de autor e publicação de Cloud Service, para garantir que os Perfis de produto corretos sejam usados, dependendo de qual camada de serviço requer acesso por meio do Console do desenvolvedor.
1. Faça logon no AEM como um Cloud Service (Autor ou Publicação) e verifique se o usuário e os grupos foram sincronizados corretamente no AEM.
   + O Developer Console requer que seu registro de usuário seja criado na camada de serviço AEM correspondente para que seja autenticado nessa camada de serviço.
1. Limpe os cookies dos navegadores, bem como o estado do aplicativo (armazenamento local) e faça novamente o logon no Developer Console, garantindo que o token de acesso Developer Console esteja usando a versão correta e inexpirada.

## Pod

AEM como um Autor de Cloud Service e serviços de publicação são compostos de várias instâncias, respectivamente, para lidar com a variabilidade do tráfego e atualizações contínuas sem tempo de inatividade. Essas instâncias são chamadas de pods. A seleção de pod no Console do desenvolvedor define o escopo dos dados que serão expostos por meio de outros controles.

![Console do desenvolvedor - Pod](./assets/developer-console/pod.png)

+ Um pod é uma instância discreta que faz parte de um Serviço AEM (Autor ou Publicação)
+ Os pods são transitórios, o que significa AEM como um Cloud Service cria e os destrói conforme a necessidade
+ Somente os pods que fazem parte do AEM associado como um ambiente Cloud Service, são listados no alternador de pods do Console do desenvolvedor.
+ Na parte inferior do alternador de pods, as opções de conveniência permitem selecionar Pods por tipo de serviço:
   + Todos os autores
   + Todos os editores
   + Todas as instâncias

## Status

O status fornece opções para a saída AEM estado de tempo de execução específico no texto ou na saída JSON. O Console do desenvolvedor fornece informações semelhantes ao console Web OSGi do Início rápido local do SDK do AEM, com a diferença acentuada de que o Console do desenvolvedor é somente leitura.

![Developer Console - Status](./assets/developer-console/status.png)

### Pacotes

Os pacotes listas todos os pacotes OSGi no AEM. Essa funcionalidade é semelhante a [AEM Pacotes OSGi do Início Rápido local do SDK](http://localhost:4502/system/console/bundles) em `/system/console/bundles`.

Ajuda do pacote na depuração por:

+ Listando todos os pacotes OSGi implantados em AEM como um serviço
+ Listando o estado de cada pacote OSGi; incluindo se estiverem ativos ou não
+ Fornecer detalhes sobre dependências não resolvidas que fazem com que os pacotes OSGi se tornem ativos

### Componentes

Componentes lista todos os componentes OSGi no AEM. Essa funcionalidade é semelhante a [AEM componentes OSGi do Início rápido local do SDK](http://localhost:4502/system/console/components) em `/system/console/components`.

Os componentes ajudam na depuração por:

+ Listando todos os componentes OSGi implantados em AEM como um Cloud Service
+ Fornecer o estado de cada componente OSGi; incluindo se estiverem ativos ou insatisfeitos
+ Fornecer detalhes sobre referências de serviço insatisfeitas pode fazer com que os componentes OSGi se tornem ativos
+ Listando propriedades OSGi e seus valores vinculados ao componente OSGi

### Configurações

As configurações listas todas as configurações do componente OSGi (propriedades e valores OSGi). Essa funcionalidade é semelhante a [AEM Gerenciador de Configuração OSGi do SDK](http://localhost:4502/system/console/configMgr) em `/system/console/configMgr`.

As configurações ajudam na depuração ao:

+ Listando propriedades OSGi e seus valores pelo componente OSGi
+ Localização e identificação de propriedades configuradas incorretamente

### Índices de Oak

Os Índices de Oak fornecem um despejo dos nós definidos abaixo de `/oak:index`. Lembre-se de que isso não mostra índices unidos, o que ocorre quando um índice AEM é modificado.

Ajuda dos Índices Oak na depuração ao:

+ Listando todas as definições de índice Oak que fornecem informações sobre como os query de pesquisa são executados em AEM. Lembre-se de que os índices modificados para AEM não são refletidos aqui. Essa visualização só é útil para índices fornecidos exclusivamente por AEM ou apenas pelo código personalizado.

### Serviços OSGi

Componentes listas todos os serviços OSGi. Essa funcionalidade é semelhante a [AEM serviços OSGi do Início Rápido local do SDK](http://localhost:4502/system/console/services) em `/system/console/services`.

Ajuda dos serviços OSGi na depuração ao:

+ Listar todos os serviços OSGi no AEM, junto com o pacote OSGi fornecido e todos os pacotes OSGi que o consumem

### Tarefas de arremesso

Tarefas de Venda listas todas as filas de Tarefas de Venda. Essa funcionalidade é semelhante a [AEM tarefas locais de início rápido](http://localhost:4502/system/console/slingevent) do SDK em `/system/console/slingevent`.

Ajuda do Sling Jobs na depuração ao:

+ Lista de filas de Sling Job e suas configurações
+ Fornecer insights sobre o número de trabalhos ativos, em fila e processados do Sling, o que é útil para depurar problemas com o Fluxo de trabalho, o Fluxo de trabalho temporário e outros trabalhos executados pelo Sling Jobs no AEM.

## Pacotes Java

Os Pacotes Java permitem verificar se um pacote Java e uma versão estão disponíveis para uso em AEM como Cloud Service. Essa funcionalidade é igual a [AEM Localizador de Dependência de Início Rápido local do SDK](http://localhost:4502/system/console/depfinder) em `/system/console/depfinder`.

![Console do desenvolvedor - Pacotes Java](./assets/developer-console/java-packages.png)

Os Pacotes Java são usados para impedir que os Pacotes sejam iniciados devido a importações não resolvidas ou classes não resolvidas em scripts (HTL, JSP etc). Se os pacotes Java não relatarem nenhum pacote exportam um pacote Java (ou a versão não corresponde àquela importada por um pacote OSGi):

+ Certifique-se de que a versão da dependência da API AEM do seu projeto corresponda à versão da versão AEM do ambiente (e, se possível, atualize tudo para a mais recente).
+ Se dependências Maven extras forem usadas no projeto Maven
   + Determine se uma API alternativa fornecida pela dependência da API do SDK AEM pode ser usada.
   + Se a dependência extra for necessária, verifique se ela é fornecida como um pacote OSGi (em vez de um Jar simples) e se está incorporada ao pacote de código do seu projeto, (`ui.apps`), de modo semelhante à forma como o pacote OSGi principal está incorporado ao pacote `ui.apps`.

## Servlets

Servlets são usados para fornecer informações sobre como AEM resolve um URL para um servlet ou script Java (HTL, JSP) que, em última análise, lida com a solicitação. Essa funcionalidade é igual a [AEM SDK do Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) em `/system/console/servletresolver`.

![Developer Console - Servlets](./assets/developer-console/servlets.png)

Servlets ajudam na depuração a determinar:

+ Como um URL é decomposto em suas partes endereçáveis (recurso, seletor, extensão).
+ Para qual servlet ou script um URL resolve, ajudando a identificar URLs mal formados ou servlets/scripts mal registrados.

## Query

Os query ajudam a fornecer informações sobre o que e como os query de pesquisa são executados em AEM. Essa funcionalidade é a mesma do console [AEM Ferramentas > Desempenho do Query ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) do Início rápido local do SDK.

Os query só funcionam quando um pod específico é selecionado, pois abre o console Web Desempenho do Query desse pod, exigindo que o desenvolvedor tenha acesso para fazer logon no serviço AEM.

![Console do desenvolvedor - Query - Explicar o Query](./assets/developer-console/queries__explain-query.png)

Query ajudam na depuração ao:

+ Explicando como os query são interpretados, analisados e executados por Oak. Isso é muito importante ao rastrear por que um query é lento, e entender como ele pode ser acelerado.
+ Listando os query mais populares em execução no AEM, com a habilidade de explicá-los.
+ Listando os query mais lentos em execução no AEM, com a capacidade de Explicá-los.
