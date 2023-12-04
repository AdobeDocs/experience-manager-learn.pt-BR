---
title: Console do desenvolvedor
description: O AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço AEM em execução que são úteis na depuração.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 388
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1408'
ht-degree: 0%

---

# Depuração do AEM as a Cloud Service com o Console do desenvolvedor

O AEM as a Cloud Service fornece um Console do desenvolvedor para cada ambiente que expõe vários detalhes do serviço AEM em execução que são úteis na depuração.

Cada ambiente as a Cloud Service do AEM tem seu próprio Console do desenvolvedor.

## Navegue até o Console do desenvolvedor

O Console do desenvolvedor é acessado por ambiente as a Cloud Service AEM por meio do Cloud Manager.

![Navegue até o Console do desenvolvedor](./assets/developer-console/navigate.png)

1. Navegue até __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. Abra o __Programa__ que contém o ambiente do AEM as a Cloud Service para abrir o Console do desenvolvedor.
3. Localize o __Ambiente__ e selecione a variável `...`.
4. Selecionar __Console do desenvolvedor__ na lista suspensa.


## Acesso ao Developer Console

Para acessar e usar o Console do desenvolvedor, as seguintes permissões devem ser concedidas ao Adobe ID do desenvolvedor via [Adobe Admin Console](https://adminconsole.adobe.com).

1. Verifique se a Adobe Org que afetou os produtos do Cloud Manager e AEM as a Cloud Service está ativa no alternador Adobe Org.
1. O desenvolvedor deve ser membro do [do Cloud Manager Product __Desenvolvedor - Cloud Service__ Perfil do produto](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer).
   + Se essa associação não existir, o desenvolvedor não poderá fazer logon no Console do desenvolvedor.
1. O desenvolvedor deve ser membro do [__Usuários do AEM__ ou __Administradores de AEM__ Perfil de produto no autor e/ou publicação do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + Se essa associação não existir, o [status](#status) os despejos atingirão o tempo limite com um erro 401 Não autorizado.

### Solução de problemas de acesso ao Developer Console

#### 401 Erro não autorizado no status dedumping

![Console do desenvolvedor - 401 Não autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se for relatado o despejo de qualquer status de um erro 401 Não autorizado, significa que o usuário ainda não existe com as permissões necessárias no AEM as a Cloud Service ou os tokens de logon usados são inválidos ou expiraram.

Para resolver o problema 401 Não autorizado:

1. Certifique-se de que o usuário é membro do Perfil de produto do Adobe IMS apropriado (Administradores de AEM ou Usuários de AEM AEM as a Cloud Service) para a instância de produto do associada ao Console do desenvolvedor.
   + Lembre-se de que o Console do desenvolvedor acessa duas instâncias de produto do Adobe IMS; as instâncias de produto do Autor e Publicação as a Cloud Service do AEM; portanto, verifique se os Perfis de produto corretos são usados, dependendo da camada de serviço que requer acesso por meio do Console do desenvolvedor.
1. Faça logon no AEM as a Cloud Service (Autor ou Publicação) e verifique se o usuário e os grupos foram sincronizados corretamente no AEM.
   + O Console do desenvolvedor requer que seu registro de usuário seja criado na camada de serviço do AEM correspondente para que ele seja autenticado nessa camada de serviço.
1. Limpe os cookies do navegador, o estado do aplicativo (armazenamento local) e faça logon novamente no Console do desenvolvedor, garantindo que o token de acesso que o Console do desenvolvedor está usando esteja correto e sem expiração.

## Pod

Os serviços de Autor e Publicação as a Cloud Service do AEM são compostos de várias instâncias, respectivamente, para lidar com a variabilidade do tráfego e as atualizações contínuas sem tempo de inatividade. Essas instâncias são chamadas de Pods. A seleção de pods no Console do desenvolvedor define o escopo dos dados que serão expostos por meio dos outros controles.

![Console do desenvolvedor - Pod](./assets/developer-console/pod.png)

+ Um pod é uma instância discreta que faz parte de um serviço AEM (Autor ou Publicação)
+ As cápsulas são transitórias, o que significa que o AEM as a Cloud Service as cria e as destrói conforme necessário
+ Somente os pods que fazem parte do ambiente associado do AEM as a Cloud Service são listados no alternador de pods do Console do desenvolvedor.
+ Na parte inferior do alternador de pods, as opções de conveniência permitem selecionar pods por tipo de serviço:
   + Todos os autores
   + Todos os editores
   + Todas as instâncias

## Status

Status fornece opções para a saída de um estado de tempo de execução AEM específico em uma saída de texto ou JSON. O Console do desenvolvedor fornece informações semelhantes ao console da Web OSGi do SDK local do AEM, com a diferença acentuada de que o Console do desenvolvedor é somente leitura.

![Console do desenvolvedor - Status](./assets/developer-console/status.png)

### Pacotes

Pacotes lista todos os pacotes OSGi no AEM. Essa funcionalidade é semelhante à [Pacotes OSGi de início rápido locais do SDK do AEM](http://localhost:4502/system/console/bundles) em `/system/console/bundles`.

Pacotes de ajuda na depuração ao:

+ Listando todos os pacotes OSGi implantados no AEM as a Service
+ Listando o estado de cada pacote OSGi; incluindo se eles estão ativos ou não
+ Fornecer detalhes em dependências não resolvidas que fazem com que pacotes OSGi se tornem ativos

### Componentes

Componentes lista todos os componentes OSGi no AEM. Essa funcionalidade é semelhante à [Componentes OSGi do SDK local do AEM](http://localhost:4502/system/console/components) em `/system/console/components`.

Ajuda dos componentes na depuração ao:

+ Listagem de todos os componentes OSGi implantados no AEM as a Cloud Service
+ Fornecer o estado de cada componente OSGi; incluindo se ele estiver ativo ou insatisfeito
+ Fornecer detalhes em referências de serviço insatisfeitas pode fazer com que os componentes OSGi se tornem ativos
+ Listagem de propriedades OSGi e seus valores vinculados ao componente OSGi.
   + Isso exibirá os valores reais inseridos via [Variáveis de configuração do ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### Configurações

Configurações lista todas as configurações do componente OSGi (propriedades e valores OSGi). Essa funcionalidade é semelhante à [Gerenciador de configurações OSGi de início rápido local do SDK do AEM](http://localhost:4502/system/console/configMgr) em `/system/console/configMgr`.

Ajuda das configurações na depuração de:

+ Listagem de propriedades OSGi e seus valores por componente OSGi
   + Isso NÃO exibirá valores reais inseridos via [Variáveis de configuração do ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). Consulte [Componentes](#components) acima, para os valores inseridos.
+ Localizar e identificar propriedades configuradas incorretamente

### Índices Oak

Os índices Oak fornecem um despejo dos nós definidos abaixo de `/oak:index`. Lembre-se de que isso não mostra índices mesclados, que ocorrem quando um índice AEM é modificado.

Ajuda dos índices Oak na depuração por:

+ Listando todas as definições de índice Oak, fornecendo insights sobre como as consultas de pesquisa são executadas no AEM. Lembre-se, que os índices AEM modificados não são refletidos aqui. Essa exibição é útil apenas para índices fornecidos exclusivamente pelo AEM ou fornecidos exclusivamente pelo código personalizado.

### Serviços OSGi

Componentes lista todos os serviços OSGi. Essa funcionalidade é semelhante à [Serviços OSGi do SDK local do AEM](http://localhost:4502/system/console/services) em `/system/console/services`.

Ajuda dos Serviços OSGi na depuração de:

+ Listagem de todos os serviços OSGi no AEM, juntamente com o pacote OSGi de fornecimento e todos os pacotes OSGi que o consomem

### Sling Jobs

Sling Jobs lista todas as filas de Sling Jobs. Essa funcionalidade é semelhante à [Tarefas do SDK local do AEM](http://localhost:4502/system/console/slingevent) em `/system/console/slingevent`.

Ajuda dos trabalhos do Sling na depuração ao:

+ Listagem de filas de trabalhos do Sling e suas configurações
+ Fornecer insights sobre o número de trabalhos Sling ativos, enfileirados e processados, o que é útil para depurar problemas com o fluxo de trabalho, fluxo de trabalho transitório e outro trabalho executado por trabalhos Sling no AEM.

## Pacotes Java

Pacotes Java permitem verificar se um pacote Java e a versão estão disponíveis para uso no AEM as a Cloud Service. Essa funcionalidade é a mesma que [Localizador de dependências de início rápido do SDK local do AEM](http://localhost:4502/system/console/depfinder) em `/system/console/depfinder`.

![Console do desenvolvedor - Pacotes Java](./assets/developer-console/java-packages.png)

Os pacotes Java são usados para solucionar problemas. Os pacotes não são iniciados devido a importações não resolvidas ou classes não resolvidas em scripts (HTL, JSP etc). Se os relatórios de Pacotes Java não exportam pacotes Java (ou a versão não corresponde à importada por um pacote OSGi):

+ Verifique se a versão da dependência do maven da API do AEM do seu projeto corresponde à versão do AEM do ambiente (e, se possível, atualize tudo para a versão mais recente).
+ Se dependências Maven extras forem usadas no projeto Maven
   + Determine se uma API alternativa fornecida pela dependência da API do SDK do AEM pode ser usada.
   + Se a dependência extra for necessária, verifique se ela é fornecida como um pacote OSGi (em vez de um Jar simples) e incorporada ao pacote de código do projeto, (`ui.apps`), semelhante a como o pacote OSGi principal é incorporado no `ui.apps` pacote.

## Servlets

Servlets são usados para fornecer informações sobre como o AEM resolve um URL para um servlet ou script Java (HTL, JSP) que, em última análise, lida com a solicitação. Essa funcionalidade é a mesma que [Resolvedor de Servlet Sling de início rápido local do SDK do AEM](http://localhost:4502/system/console/servletresolver) em `/system/console/servletresolver`.

![Console do desenvolvedor - Servlets](./assets/developer-console/servlets.png)

Os servlets ajudam na depuração da determinação:

+ Como um URL é decomposto em suas partes endereçáveis (recurso, seletor, extensão).
+ A que servlet ou script um URL resolve, ajudando a identificar URLs mal formados ou servlets/scripts mal registrados.

## Consultas

As consultas ajudam a fornecer insights sobre o que e como as consultas de pesquisa são executadas no AEM. Essa funcionalidade é a mesma que  [Ferramentas > Desempenho da consulta do SDK local do AEM](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) console.

O Queries só funciona quando um pod específico é selecionado, pois abre o console da Web Desempenho de consulta desse pod, exigindo que o desenvolvedor tenha acesso para fazer logon no serviço AEM.

![Console do desenvolvedor - Consultas - Explicar consulta](./assets/developer-console/queries__explain-query.png)

As consultas ajudam na depuração ao:

+ Explicando como as consultas são interpretadas, analisadas e executadas pelo Oak. Isso é muito importante ao rastrear por que um query é lento e entender como ele pode ser acelerado.
+ Listando as consultas mais populares executadas no AEM, com a capacidade de Explicá-las.
+ Listar as consultas mais lentas em execução no AEM, com a capacidade de Explicá-las.
