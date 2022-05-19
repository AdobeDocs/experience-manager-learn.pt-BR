---
title: Console do desenvolvedor
description: AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço de AEM em execução que são úteis na depuração.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
source-git-commit: 751aed9b8659d6a600429efb2bf60825b6d39144
workflow-type: tm+mt
source-wordcount: '1396'
ht-degree: 0%

---

# Depuração AEM as a Cloud Service com o Console do desenvolvedor

AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço de AEM em execução que são úteis na depuração.

Cada ambiente AEM as a Cloud Service tem seu próprio Console do desenvolvedor.

## Acesso ao Console do desenvolvedor

Para acessar e usar o Console do desenvolvedor, as seguintes permissões devem ser fornecidas ao Adobe ID do desenvolvedor por meio do [Admin Console](https://adminconsole.adobe.com).

1. Certifique-se de que a organização do Adobe que afetou o Cloud Manager e AEM produtos as a Cloud Service esteja ativa no seletor de organização do Adobe.
1. O desenvolvedor deve ser membro do produto do Cloud Manager __Desenvolvedor - Cloud Service__ Perfil do produto
   + Se essa associação não existir, o desenvolvedor não poderá fazer logon no Console do desenvolvedor.
1. O desenvolvedor deve ser membro do __Usuários AEM__ ou __Administradores de AEM__ Perfil do produto no autor e/ou publicação do AEM.
   + Se essa associação não existir, a variável [status](#status) os despejos atingirão o tempo limite com um erro 401 Unauthorized .

### Solução de problemas do acesso ao Console do desenvolvedor

#### 401 Erro não autorizado quando do estatuto de dumping

![Console do desenvolvedor - 401 Não autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se for reportado um erro 401 Unauthorized com qualquer status de dumping , significa que o usuário ainda não existe com as permissões necessárias AEM as a Cloud Service ou que os tokens de logon são inválidos ou expiraram.

Para resolver o problema 401 Não autorizado:

1. Certifique-se de que o usuário é membro do Perfil de produto do Adobe IMS apropriado (Administradores AEM ou Usuários AEM) para a instância de produto as a Cloud Service AEM associada do Console do desenvolvedor.
   + Lembre-se de que o Console do desenvolvedor acessa duas Instâncias de produto do Adobe IMS; As instâncias de produto Autor e Publicação as a Cloud Service do AEM, para garantir que os Perfis de produto corretos sejam usados, dependendo de qual camada de serviço requer acesso por meio do Console do desenvolvedor.
1. Faça logon no AEM as a Cloud Service (Autor ou Publicação) e verifique se o usuário e os grupos foram sincronizados corretamente no AEM.
   + O Console do desenvolvedor requer que o registro do usuário seja criado na camada de serviço de AEM correspondente para ser autenticado nessa camada de serviço.
1. Limpe os cookies dos navegadores, bem como o estado do aplicativo (armazenamento local) e faça logon novamente no Console do desenvolvedor, garantindo que o token de acesso que o Console do desenvolvedor está usando esteja correto e expirado.

## Pod

AEM serviços as a Cloud Service de Autor e Publicação são compostos de várias instâncias, respectivamente, para lidar com a variabilidade do tráfego e atualizações contínuas sem tempo de inatividade. Essas instâncias são chamadas de Pods. A seleção de pod no Console do desenvolvedor define o escopo dos dados que serão expostos por meio de outros controles.

![Console do desenvolvedor - Pod](./assets/developer-console/pod.png)

+ Um pod é uma instância discreta que faz parte de um Serviço de AEM (Autor ou Publicação)
+ Os pods são transitórios, o que significa AEM criações as a Cloud Service e os destrói conforme necessário
+ Somente os pods que fazem parte do ambiente AEM as a Cloud Service associado são listados no alternador de pods do Console do desenvolvedor.
+ Na parte inferior do alternador de pods, as opções de conveniência permitem selecionar Pods por tipo de serviço:
   + Todos os autores
   + Todos os editores
   + Todas as instâncias

## Status

O status fornece opções para a saída de um estado de tempo de execução AEM específico no texto ou na saída JSON. O Console do desenvolvedor fornece informações semelhantes ao console da Web OSGi do início rápido local do SDK AEM, com a diferença marcada de que o Console do desenvolvedor é somente leitura.

![Console do desenvolvedor - Status](./assets/developer-console/status.png)

### Pacotes

Pacotes lista todos os pacotes OSGi no AEM. Essa funcionalidade é semelhante a [Pacotes OSGi do início rápido local do SDK AEM](http://localhost:4502/system/console/bundles) at `/system/console/bundles`.

Pacotes ajudam na depuração por:

+ Listando todos os pacotes OSGi implantados no AEM como um serviço
+ Listando o estado de cada pacote OSGi; incluindo se estiverem ativos ou não
+ Fornecer detalhes sobre dependências não resolvidas que façam com que os pacotes OSGi se tornem ativos

### Componentes

Os componentes listam todos os componentes OSGi no AEM. Essa funcionalidade é semelhante a [Componentes OSGi do início rápido local do AEM SDK](http://localhost:4502/system/console/components) at `/system/console/components`.

Os componentes ajudam na depuração por:

+ Listando todos os componentes OSGi implantados em AEM as a Cloud Service
+ Fornecer o estado de cada componente OSGi; incluindo se estiverem ativos ou insatisfeitos
+ Fornecer detalhes sobre referências de serviço insatisfeitas pode fazer com que os componentes OSGi fiquem ativos
+ Listando propriedades OSGi e seus valores vinculados ao componente OSGi.
   + Isso exibirá os valores reais injetados por [Variáveis de configuração do ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### Configurações

As configurações listam todas as configurações do componente OSGi (propriedades e valores do OSGi). Essa funcionalidade é semelhante a [AEM Gerenciador de configuração OSGi do início rápido local do SDK](http://localhost:4502/system/console/configMgr) at `/system/console/configMgr`.

Ajuda das configurações na depuração por:

+ Listando propriedades OSGi e seus valores por componente OSGi
   + Isso NÃO exibirá os valores reais inseridos via [Variáveis de configuração do ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). Consulte [Componentes](#components) acima, para os valores inseridos.
+ Localização e identificação de propriedades mal configuradas

### Índices Oak

Os Índices Oak fornecem um despejo dos nós definidos abaixo `/oak:index`. Lembre-se de que isso não mostra índices mesclados, o que ocorre quando um índice de AEM é modificado.

Ajuda dos Índices Oak na depuração por:

+ Listando todas as definições do índice Oak fornecendo insights de como as consultas de pesquisa são executadas em AEM. Lembre-se de que os índices modificados em AEM não são refletidos aqui. Essa exibição só é útil para índices fornecidos exclusivamente por AEM ou fornecidos exclusivamente pelo código personalizado.

### Serviços OSGi

Componentes lista todos os serviços OSGi. Essa funcionalidade é semelhante a [Serviços OSGi do início rápido local do AEM SDK](http://localhost:4502/system/console/services) at `/system/console/services`.

Ajuda dos Serviços OSGi na depuração por:

+ Listar todos os serviços OSGi no AEM, juntamente com seu pacote OSGi, e todos os pacotes OSGi que o consomem

### Sling Jobs

O Sling Jobs lista todas as filas do Sling Jobs. Essa funcionalidade é semelhante a [AEM trabalhos de inicialização rápida local do SDK](http://localhost:4502/system/console/slingevent) at `/system/console/slingevent`.

Ajuda do Sling Jobs na depuração ao:

+ Listagem de filas de trabalhos do Sling e suas configurações
+ Fornecer insights sobre o número de trabalhos do Sling ativos, enfileirados e processados, o que é útil para depurar problemas com o Fluxo de trabalho, o Fluxo de trabalho transitório e outros trabalhos executados pelos Trabalhos do Sling no AEM.

## Pacotes Java

Os pacotes Java permitem verificar se um pacote Java, e a versão, estão disponíveis para uso AEM as a Cloud Service. Essa funcionalidade é igual a [Localizador de dependência do início rápido local do SDK AEM](http://localhost:4502/system/console/depfinder) at `/system/console/depfinder`.

![Console do desenvolvedor - Pacotes Java](./assets/developer-console/java-packages.png)

Os pacotes Java são usados para solucionar problemas de pacotes que não estão sendo iniciados por causa de importações não resolvidas ou classes não resolvidas em scripts (HTL, JSP etc). Se os pacotes Java não relatarem nenhum pacote exportarem um pacote Java (ou a versão não corresponder àquela importada por um pacote OSGi):

+ Certifique-se de que a versão da dependência do AEM API do projeto corresponde à versão da versão da AEM do ambiente (e, se possível, atualize tudo para a mais recente).
+ Se as dependências de Maven extras forem usadas no projeto Maven
   + Determine se uma API alternativa fornecida pela dependência da API do SDK AEM pode ser usada.
   + Se a dependência extra for necessária, verifique se ela é fornecida como um pacote OSGi (em vez de um Jar simples) e se está incorporada ao pacote de código do seu projeto, (`ui.apps`), semelhante à forma como o pacote OSGi principal é incorporado na variável `ui.apps` pacote.

## Servlets

Os servlets são usados para fornecer informações sobre como o AEM resolve um URL para um servlet ou script Java (HTL, JSP) que, em última análise, lida com a solicitação. Essa funcionalidade é igual a [AEM Resolvedor do Sling Servlet do início rápido local do SDK](http://localhost:4502/system/console/servletresolver) at `/system/console/servletresolver`.

![Console do desenvolvedor - Servlets](./assets/developer-console/servlets.png)

Os servlets ajudam na depuração a determinar:

+ Como um URL é decomposto em suas partes endereçáveis (recurso, seletor, extensão).
+ Para qual servlet ou script um URL resolve, ajudando a identificar URLs mal formados ou servlets/scripts mal registrados.

## Queries

As consultas ajudam a fornecer informações sobre o que e como as consultas de pesquisa são executadas no AEM. Essa funcionalidade é igual a  [AEM Ferramentas de inicialização rápida local do SDK > Desempenho da consulta ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) console.

As consultas só funcionam quando um pod específico é selecionado, pois abre o console da Web de Desempenho da Consulta desse pod, exigindo que o desenvolvedor tenha acesso para fazer logon no serviço de AEM.

![Console do Desenvolvedor - Consultas - Explicar Consulta](./assets/developer-console/queries__explain-query.png)

As consultas ajudam na depuração ao:

+ Explicando como as consultas são interpretadas, analisadas e executadas pelo Oak. Isso é muito importante ao rastrear por que um query está lento e entender como ele pode ser acelerado.
+ Listando as consultas mais populares em execução no AEM, com a capacidade de Explicá-las.
+ Listando as consultas mais lentas em execução no AEM, com a capacidade de Explicá-las.
