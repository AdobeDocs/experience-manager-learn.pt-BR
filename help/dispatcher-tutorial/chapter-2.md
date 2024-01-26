---
title: Capítulo 2 - Infraestrutura do Dispatcher
description: Entender a topologia de publicação e dispatcher. Saiba mais sobre as topologias e configurações mais comuns.
feature: Dispatcher
topic: Architecture
role: Architect
level: Beginner
doc-type: Tutorial
exl-id: a25b6f74-3686-40a9-a148-4dcafeda032f
duration: 504
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1882'
ht-degree: 0%

---

# Capítulo 2 - Infraestrutura

## Configurando uma Infraestrutura de Armazenamento em Cache

Introduzimos a topologia básica de um sistema de publicação e um dispatcher no Capítulo 1 desta série. Um conjunto de servidores Publish e Dispatcher pode ser configurado em muitas variações, dependendo da carga esperada, da topologia de seu(s) data center(s) e das propriedades de failover desejadas.

Vamos esboçar as topologias mais comuns e descrever as vantagens e onde elas ficam aquém. A lista - é claro - nunca pode ser abrangente. O único limite é a sua imaginação.

### A configuração &quot;herdada&quot;

No início, o número de visitantes em potencial era pequeno, o hardware era caro e os servidores da Web não eram considerados tão críticos para os negócios quanto são hoje. Uma configuração comum era ter um Dispatcher atuando como balanceador de carga e cache na frente de dois ou mais sistemas de publicação. O servidor Apache no núcleo do Dispatcher era muito estável e, na maioria das configurações, capaz o suficiente para atender a uma quantidade decente de solicitações.

![Configuração &quot;herdada&quot; do Dispatcher - Não muito comum pelos padrões atuais](assets/chapter-2/legacy-dispatcher-setup.png)

*Configuração &quot;herdada&quot; do Dispatcher - Não muito comum pelos padrões atuais*

<br> 

Foi daqui que o dispatcher recebeu seu nome: Ele basicamente enviava solicitações. Essa configuração não é mais muito comum, pois não pode atender às demandas mais altas de desempenho e estabilidade necessárias atualmente.

### Configuração de várias pernas

Hoje em dia uma topologia ligeiramente diferente é mais comum. Uma topologia de várias pernas teria um Dispatcher por servidor de publicação. Um balanceador de carga dedicado (hardware) fica na frente da infraestrutura do AEM despachando as solicitações para esses dois (ou mais) segmentos:

![Configuração moderna do Dispatcher &quot;padrão&quot; — fácil de manusear e manter](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configuração moderna do Dispatcher &quot;padrão&quot; — fácil de manusear e manter*

<br> 

Estas são as razões para esse tipo de configuração,

1. Os sites, em média, atendem a muito mais tráfego do que no passado. Portanto, é necessário aumentar a &quot;infraestrutura do Apache&quot;.

2. A configuração &quot;Herdada&quot; não fornecia redundância no nível do Dispatcher. Se o Apache Server ficar inativo, todo o site estava inacessível.

3. Os servidores Apache são baratos. Elas são baseadas em código aberto e, como você tem um data center virtual, elas podem ser provisionadas com muita rapidez.

4. Essa configuração oferece uma maneira fácil de atualizar um cenário &quot;contínuo&quot; ou &quot;escalonado&quot;. Você simplesmente desliga o Dispatcher 1 ao instalar um novo pacote de software no Publish 1. Quando a instalação estiver concluída e você tiver o Publish 1 da rede interna suficientemente testado contra fumaça, limpe o cache no Dispatcher 1 e reinicie-o enquanto desativa o Dispatcher 2 para manutenção do Publish 2.

5. A invalidação de cache se torna muito fácil e determinística nessa configuração. Como apenas um sistema de publicação está conectado a um Dispatcher, há apenas um Dispatcher para invalidar. A ordem e o momento da invalidação são triviais.

### A configuração de &quot;Expansão&quot;

Os servidores Apache são baratos e fáceis de provisionar, por que não forçar a expansão desse nível um pouco mais. Por que não ter dois ou mais Dispatchers na frente de cada Servidor de publicação?

![Configuração de &quot;expansão&quot; - tem algumas áreas de aplicativo, mas também limitações e avisos](assets/chapter-2/scale-out-setup.png)

*Configuração de &quot;expansão&quot; - tem algumas áreas de aplicativo, mas também limitações e avisos*

<br> 

Você pode fazer isso! E há muitos cenários de aplicação válidos para essa configuração. Mas também há algumas limitações e complexidades que você deve considerar.

#### Invalidação

Cada sistema de publicação está conectado a uma variedade de Dispatchers e cada um deles deve ser invalidado quando o conteúdo for alterado.

#### Manutenção

Escusado será dizer que a configuração inicial dos sistemas Dispatcher e Publish é um pouco mais complexa. Mas lembre-se também de que o esforço de uma versão &quot;contínua&quot; também é um pouco maior. Os sistemas AEM podem e devem ser atualizados durante a execução. Mas é sábio não fazer isso enquanto eles estão ativamente atendendo solicitações. Normalmente, você deseja atualizar apenas uma parte dos Sistemas de publicação, enquanto os outros ainda atendem ativamente ao tráfego e, depois de testar, alternam para a outra parte. Se você tiver sorte e puder acessar o balanceador de carga em seu processo de implantação, poderá desativar o roteamento para os servidores em manutenção aqui. Se você estiver em um balanceador de carga compartilhado sem acesso direto, é melhor desligar os dispatchers do Publish que deseja atualizar. Quanto mais existirem, mais você terá que desligar. Se houver um grande número e você estiver planejando atualizações frequentes, recomenda-se alguma automação. Se você não tiver ferramentas de automação, dimensionar é uma má ideia, de qualquer forma.

Em um projeto anterior, usamos um truque diferente para remover um sistema de publicação do balanceamento de carga sem ter acesso direto ao próprio balanceador de carga.

O balanceador de carga geralmente &quot;faz o ping&quot;, uma página específica para ver se o servidor está ativo e em execução. Uma escolha trivial geralmente é fazer ping na página inicial. Mas se você quiser usar o ping para sinalizar o balanceador de carga para não balancear o tráfego, escolha outra coisa. Você cria um modelo ou servlet dedicado que pode ser configurado para responder com `"up"` ou `"down"` (no corpo ou como código de resposta http). A resposta dessa página, claro, não deve ser armazenada em cache no Dispatcher; portanto, ela sempre é buscada recentemente no sistema de publicação. Agora, se você configurar o balanceador de carga para verificar esse template ou servlet, poderá facilmente deixar o Publish &quot;fingir&quot; estar inativo. Ela não faria parte do balanceamento de carga e pode ser atualizada.

#### Distribuição mundial

&quot;Distribuição mundial&quot; é uma configuração de &quot;Expansão&quot; em que você tem vários Dispatchers na frente de cada sistema de Publicação - agora distribuídos em todo o mundo para estarem mais próximos do cliente e fornecerem um melhor desempenho. É claro que nesse cenário você não tem um balanceador de carga central, mas um esquema de balanceamento de carga baseado em DNS e geo-IP.

>[!NOTE]
>
>Na verdade, você está criando um tipo de rede de distribuição de conteúdo (CDN) com essa abordagem; portanto, deve considerar comprar uma solução de CDN pronta para uso em vez de criar uma por conta própria. Criar e manter uma CDN personalizada não é uma tarefa trivial.

#### Escala horizontal

Mesmo em um data center local, uma topologia de &quot;expansão&quot; com vários Dispatchers na frente de cada sistema de publicação tem algumas vantagens. Se você observar gargalos de desempenho nos servidores Apache devido ao alto tráfego (e uma boa taxa de acerto de cache) e não puder mais aumentar o hardware (adicionando CPUs, RAM e discos mais rápidos), poderá aumentar o desempenho adicionando Dispatchers. Isso é chamado de &quot;dimensionamento horizontal&quot;. No entanto, há limites, especialmente quando o tráfego é invalidado com frequência. Vamos descrever o efeito na próxima seção.

#### Limites da topologia de expansão

A adição de servidores proxy normalmente deve aumentar o desempenho. Entretanto, há cenários em que a adição de servidores pode realmente diminuir o desempenho. Como? Considere ter um portal de notícias, em que você introduz novos artigos e páginas a cada minuto. Um Dispatcher invalida por &quot;invalidação automática&quot;: sempre que uma página é publicada, todas as páginas no cache do mesmo site são invalidadas. Esse é um recurso útil. Abordamos isso em [Capítulo 1](chapter-1.md) desta série - mas também significa que, quando você tem alterações frequentes em seu site, você está invalidando o cache com bastante frequência. Se você tiver apenas um Dispatcher por instância de publicação, o primeiro visitante que solicitar uma página acionará um novo armazenamento em cache dessa página. O segundo visitante já obtém a versão em cache.

Se você tiver dois Dispatchers, o segundo visitante terá 50% de chance de a página não ser armazenada em cache e ele experimentará uma latência maior quando essa página for renderizada novamente. Ter ainda mais Dispatchers por publicação torna as coisas ainda piores. O que acontece é que o servidor de publicação recebe mais carga porque precisa renderizar novamente a página para cada Dispatcher separadamente.

![Desempenho reduzido em um cenário de expansão com liberações frequentes de cache.](assets/chapter-2/decreased-performance.png)

*Desempenho reduzido em um cenário de expansão com liberações frequentes de cache.*

<br> 

#### Reduzindo problemas de expansão

Você pode considerar usar um armazenamento compartilhado central para todos os Dispatchers ou sincronizar os sistemas de arquivos dos servidores Apache para atenuar os problemas. Podemos fornecer apenas uma experiência em primeira mão limitada, mas estar preparados para que isso aumente a complexidade do sistema e possa introduzir uma classe totalmente nova de erros.

Tivemos alguns experimentos com NFS, mas o NFS apresenta enormes problemas de desempenho devido ao bloqueio de conteúdo. Isso na verdade diminuiu o desempenho geral.

**Conclusão** - O compartilhamento de um sistema de arquivos comum entre vários dispatchers NÃO é uma abordagem recomendada.

Se você estiver enfrentando problemas de desempenho, aumente a escala de Publicação e Dispatchers igualmente para evitar o pico de carga nas instâncias do Publisher. Não há uma regra de ouro sobre a proporção Publicar/Dispatcher: ela depende muito da distribuição das solicitações e da frequência das publicações e invalidações de cache.

Se você também estiver preocupado com a latência que um visitante enfrenta, considere usar uma rede de entrega de conteúdo, a recuperação de cache, o aquecimento preventivo do cache e a definição de um tempo de carência, conforme descrito em [Capítulo 1](chapter-1.md) desta série ou consulte algumas ideias avançadas de [Parte 3](chapter-3.md).

### A configuração &quot;Cross Connected&quot;

Outra configuração que vimos de vez em quando é a configuração &quot;entre conexões&quot;: as instâncias de Publicação não têm Dispatchers dedicados, mas todos os Dispatchers estão conectados a todos os sistemas de Publicação.

![Topologia entre conexões: maior redundância e complexidade](assets/chapter-2/cross-connected-setup.png)

<br> 

*Topologia entre conexões: maior redundância e complexidade.*

À primeira vista, isso fornece mais redundância para um orçamento relativamente pequeno. Quando um dos servidores Apache está inativo, você ainda pode ter dois sistemas de publicação fazendo o trabalho de renderização. Além disso, se um dos sistemas de publicação falhar, ainda haverá dois Dispatchers que atenderão à carga em cache.

Isso, no entanto, vem com um preço.

Primeiro, tirar uma perna para manutenção é bastante complicado. Na verdade, foi para isso que esse esquema foi projetado; para ser mais resiliente e permanecer em funcionamento por todos os meios possíveis. Temos visto planos de manutenção complicados sobre como lidar com isso. Reconfigure o Dispatcher 2 primeiro, removendo a conexão cruzada. Reinicialização do Dispatcher 2. Desligando o Dispatcher 1, atualizando Publicar 1, ... e assim por diante. Você deve considerar cuidadosamente se isso aumenta para mais de duas pernas. Vocês chegarão à conclusão de que isso realmente aumenta a complexidade, os custos e é uma fonte formidável de erro humano. Seria melhor automatizar isso. Portanto, é melhor verificar se você realmente tem os recursos humanos para incluir essa tarefa de automação no cronograma do seu projeto. Embora você possa economizar alguns custos de hardware com isso, você pode gastar o dobro em equipe de TI.

Em segundo lugar, você pode ter algum aplicativo de usuário em execução no AEM que requer um logon. Use sessões aderentes para garantir que um usuário sempre seja distribuído da mesma instância do AEM, para que você possa manter o estado da sessão nessa instância. Com essa configuração com conexão cruzada, é necessário garantir que as sessões adesivas estejam funcionando corretamente no balanceador de carga e nos Dispatchers. Não é impossível, mas você precisa estar ciente disso e adicionar algumas horas extras de configuração e teste, o que - novamente - pode nivelar a economia planejada com a economia de hardware.

### Conclusão

Não recomendamos que você use esse esquema de conexão cruzada como uma opção padrão. No entanto, se decidir usá-lo, avalie cuidadosamente os riscos e os custos ocultos e planeje incluir a automação da configuração como parte do projeto.

## Próxima etapa

* [3 - Tópicos avançados de armazenamento em cache](chapter-3.md)
