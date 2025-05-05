---
title: Console do desenvolvedor
description: O AEM as a Cloud Service fornece um Developer Console para cada ambiente que expõe vários detalhes do serviço do AEM em execução que são úteis na depuração.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
duration: 295
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 0%

---

# Depuração do AEM as a Cloud Service com a Developer Console

O AEM as a Cloud Service fornece um Developer Console para cada ambiente que expõe vários detalhes do serviço do AEM em execução que são úteis na depuração.

Cada ambiente do AEM as a Cloud Service tem seu próprio Developer Console.

## Navegar até o Developer Console

O Developer Console é acessado por ambiente AEM as a Cloud Service via Cloud Manager.

![Navegar até o Developer Console](./assets/developer-console/navigate.png)

1. Navegue até __[Cloud Manager](https://my.cloudmanager.adobe.com/)__
2. Abra o __Programa__ que contém o ambiente do AEM as a Cloud Service para abrir o Developer Console.
3. Localize o __Ambiente__ e selecione o `...`.
4. Selecione __Developer Console__ na lista suspensa.


## Acesso ao Developer Console

Para acessar e usar o Developer Console, as seguintes permissões devem ser concedidas ao Adobe ID do desenvolvedor via [Admin Console da Adobe](https://adminconsole.adobe.com).

1. Certifique-se de que, no alternador da Organização do Adobe, é possível visualizar a Organização do Adobe relacionada aos ambientes que você deseja inspecionar no Developer Console.
1. Para fazer logon na Developer Console, o desenvolvedor deve ser membro de qualquer uma das seguintes funções:
   + [Perfil do Produto __do Cloud Manager - Cloud Service__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html?lang=pt-BR#assign-developer): neste caso, o desenvolvedor verá a lista completa de ambientes disponíveis na URL do Developer Console selecionada; se um ambiente de desenvolvimento ou RDE tiver sido selecionado no Cloud Manager, outro ambiente de desenvolvimento ou RDEs nesse mesmo Programa poderão ser exibidos.
   + [__Perfil de Produto de Administradores do AEM__ em __Autor do AEM__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html?lang=pt-BR#aem-product-profiles): nesse caso, a lista de ambientes descrita no marcador anterior será limitada aos perfis de produto relacionados aos quais essa função for atribuída.
1. O desenvolvedor deve ser membro do Perfil de Produto [__Usuários do AEM__ ou __Administradores do AEM__ no Autor e/ou Publicação do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html?lang=pt-BR#aem-product-profiles).
   + Se esta associação não existir, os despejos de [status](#status) atingirão o tempo limite com um erro 401 Não autorizado.

### Solução de problemas de acesso ao Developer Console

#### Quando eu fizer login, não vejo um ambiente listado que estou procurando

Certifique-se do seguinte:

+ Você selecionou o URL correto do Developer Console clicando nos três pontos do ambiente selecionado por meio do Cloud Manager e selecione Developer Console.
+ Você tem o [Perfil de Produto __do Cloud Manager - Cloud Service__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html?lang=pt-BR#assign-developer) para ver a lista completa de ambientes ou você faz parte do Perfil de Produto [__Administradores do AEM__ no __Autor do AEM__](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html?lang=pt-BR#aem-product-profiles) para o ambiente não encontrado.

#### 401 Erro não autorizado no status dedumping

![Developer Console - 401 Não autorizado](./assets/developer-console/troubleshooting__401-unauthorized.png)

Se for relatado um erro 401 Não autorizado despejando qualquer status, significa que o usuário ainda não existe com as permissões necessárias no AEM as a Cloud Service ou os tokens de logon usados são inválidos ou expiraram.

Para resolver o problema 401 Não autorizado:

1. Certifique-se de que o usuário é membro do Perfil de produto do Adobe IMS apropriado (Administradores do AEM ou Usuários do AEM) para a instância de produto associada do Developer Console AEM as a Cloud Service.
   + Lembre-se de que o Developer Console acessa 2 Instâncias de produto do Adobe IMS; as instâncias de produto do AEM as a Cloud Service Author e Publish, portanto, verifique se os Perfis de produto corretos são usados, dependendo de qual camada de serviço requer acesso por meio do Developer Console.
1. Faça logon na AEM as a Cloud Service (Autor ou Publicação) e verifique se o usuário e os grupos foram sincronizados corretamente na AEM.
   + O Developer Console requer que seu registro de usuário seja criado na camada de serviço correspondente do AEM para que ele seja autenticado nessa camada de serviço.
1. Limpe os cookies dos navegadores, o estado do aplicativo (armazenamento local) e faça logon novamente no Developer Console, garantindo que o token de acesso que a Developer Console está usando esteja correto e sem expiração.

## Pod

Os serviços de Autor e Publicação do AEM as a Cloud Service são compostos de várias instâncias, respectivamente, para lidar com a variabilidade do tráfego e atualizações contínuas sem tempo de inatividade. Essas instâncias são chamadas de Pods. A seleção de pods no Developer Console define o escopo dos dados que serão expostos por meio dos outros controles.

![Developer Console - Pod](./assets/developer-console/pod.png)

+ Um pod é uma instância discreta que faz parte de um serviço do AEM (Autor ou Publicação)
+ Os pods são transitórios, o que significa que o AEM as a Cloud Service os cria e destrói conforme necessário
+ Somente os pods que fazem parte do ambiente associado do AEM as a Cloud Service são listados no alternador de pods do Developer Console desse ambiente.
+ Na parte inferior do alternador de pods, as opções de conveniência permitem selecionar pods por tipo de serviço:
   + Todos os autores
   + Todos os editores
   + Todas as instâncias

## Status

Status fornece opções para a saída de um estado de tempo de execução específico do AEM em texto ou saída JSON. O Developer Console fornece informações semelhantes ao console OSGi da Web de início rápido local do AEM SDK, com a diferença marcada de que o Developer Console é somente leitura.

![Developer Console - Status](./assets/developer-console/status.png)

### Pacotes

Pacotes lista todos os pacotes OSGi no AEM. Esta funcionalidade é semelhante aos [Pacotes OSGi de início rápido locais do AEM SDK](http://localhost:4502/system/console/bundles) em `/system/console/bundles`.

Pacotes de ajuda na depuração ao:

+ Listando todos os pacotes OSGi implantados no AEM as a Service
+ Listando o estado de cada pacote OSGi; incluindo se eles estão ativos ou não
+ Fornecer detalhes em dependências não resolvidas que fazem com que pacotes OSGi se tornem ativos

### Componentes

Componentes lista todos os componentes OSGi no AEM. Esta funcionalidade é semelhante aos [Componentes OSGi de início rápido locais do AEM SDK](http://localhost:4502/system/console/components) em `/system/console/components`.

Ajuda dos componentes na depuração ao:

+ Listagem de todos os componentes OSGi implantados no AEM as a Cloud Service
+ Fornecer o estado de cada componente OSGi; incluindo se ele estiver ativo ou insatisfeito
+ Fornecer detalhes em referências de serviço insatisfeitas pode fazer com que os componentes OSGi se tornem ativos
+ Listagem de propriedades OSGi e seus valores vinculados ao componente OSGi.
   + Isso exibirá os valores reais inseridos por meio de [variáveis de configuração de ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=pt-BR#environment-specific-configuration-values).

### Configurações

Configurações lista todas as configurações do componente OSGi (propriedades e valores OSGi). Esta funcionalidade é semelhante ao [Gerenciador de configurações OSGi de início rápido local do AEM SDK](http://localhost:4502/system/console/configMgr) em `/system/console/configMgr`.

Ajuda das configurações na depuração de:

+ Listagem de propriedades OSGi e seus valores por componente OSGi
   + Isso NÃO exibirá os valores reais inseridos por meio de [variáveis de configuração de ambiente OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=pt-BR#environment-specific-configuration-values). Consulte [Componentes](#components) acima para obter os valores inseridos.
+ Localizar e identificar propriedades configuradas incorretamente

### Índices Oak

Os Índices Oak fornecem um despejo dos nós definidos abaixo de `/oak:index`. Lembre-se de que isso não mostra índices mesclados, que ocorrem quando um índice AEM é modificado.

A ajuda dos Índices Oak na depuração por:

+ Listando todas as definições de índice do Oak, fornecendo insights sobre como as consultas de pesquisa são executadas no AEM. Lembre-se, que os índices modificados do AEM não são refletidos aqui. Essa exibição é útil apenas para índices fornecidos exclusivamente pela AEM ou fornecidos exclusivamente pelo código personalizado.

### Serviços OSGi

Componentes lista todos os serviços OSGi. Esta funcionalidade é semelhante aos [Serviços OSGi de início rápido locais do AEM SDK](http://localhost:4502/system/console/services) em `/system/console/services`.

Ajuda dos Serviços OSGi na depuração de:

+ Listagem de todos os serviços OSGi no AEM, juntamente com o pacote OSGi de fornecimento e todos os pacotes OSGi que o consomem

### Sling Jobs

Sling Jobs lista todas as filas de Sling Jobs. Esta funcionalidade é semelhante aos [Trabalhos de início rápido locais do AEM SDK](http://localhost:4502/system/console/slingevent) em `/system/console/slingevent`.

Ajuda dos trabalhos do Sling na depuração ao:

+ Listagem de filas de trabalhos do Sling e suas configurações
+ Fornecer insights sobre o número de trabalhos Sling ativos, enfileirados e processados, o que é útil para depurar problemas com o fluxo de trabalho, o fluxo de trabalho transitório e outro trabalho executado pelos trabalhos Sling no AEM.

## Pacotes Java

Pacotes Java permitem verificar se um pacote Java e a versão estão disponíveis para uso no AEM as a Cloud Service. Esta funcionalidade é igual ao [Localizador de dependências de início rápido local do AEM SDK](http://localhost:4502/system/console/depfinder) em `/system/console/depfinder`.

![Developer Console - Pacotes Java](./assets/developer-console/java-packages.png)

Os pacotes Java são usados para solucionar problemas. Os pacotes não são iniciados devido a importações não resolvidas ou classes não resolvidas em scripts (HTL, JSP etc). Se os relatórios de Pacotes Java não exportam pacotes Java (ou a versão não corresponde à importada por um pacote OSGi):

+ Verifique se a versão da dependência da API do Maven do AEM do seu projeto corresponde à versão do AEM Release do ambiente (e, se possível, atualize tudo para a versão mais recente).
+ Se dependências Maven extras forem usadas no projeto Maven
   + Determine se uma API alternativa fornecida pela dependência da API SDK do AEM pode ser usada em seu lugar.
   + Se a dependência extra for necessária, verifique se ela é fornecida como um pacote OSGi (em vez de um Jar simples) e se está incorporada ao pacote de código do seu projeto, (`ui.apps`), de modo semelhante a como o Pacote OSGi principal está incorporado ao pacote `ui.apps`.

## Servlets

Servlets são usados para fornecer informações sobre como o AEM resolve um URL para um servlet ou script Java (HTL, JSP) que manipula a solicitação. AEM Esta funcionalidade é a mesma que o [Resolvedor de Servlet Sling do SDK local de início rápido](http://localhost:4502/system/console/servletresolver) em `/system/console/servletresolver`.

![Developer Console - Servlets](./assets/developer-console/servlets.png)

Os servlets ajudam na depuração da determinação:

+ Como um URL é decomposto em suas partes endereçáveis (recurso, seletor, extensão).
+ A que servlet ou script um URL resolve, ajudando a identificar URLs mal formados ou servlets/scripts mal registrados.

## Consultas

As consultas ajudam a fornecer insights sobre o que e como as consultas de pesquisa são executadas no AEM. Essa funcionalidade é a mesma que o console [Ferramentas > Desempenho da consulta](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) do AEM SDK.

O Queries só funciona quando um pod específico é selecionado, pois abre o console da Web Desempenho de consulta desse pod, exigindo que o desenvolvedor tenha acesso para fazer logon no serviço AEM.

![Developer Console - Consultas - Explicar consulta](./assets/developer-console/queries__explain-query.png)

As consultas ajudam na depuração ao:

+ Explicando como as consultas são interpretadas, analisadas e executadas pelo Oak. Isso é muito importante ao rastrear por que um query é lento e entender como ele pode ser acelerado.
+ Listagem das consultas mais populares executadas no AEM, com a capacidade de Explicá-las.
+ Listar as consultas mais lentas em execução no AEM, com a capacidade de Explicá-las.
