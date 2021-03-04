---
title: Capítulo 2 - Definição dos modelos de fragmento do conteúdo do evento
seo-title: Introdução aos serviços de conteúdo do AEM - Capítulo 2 - Definição dos modelos de fragmento do conteúdo do evento
description: O Capítulo 2 do tutorial AEM Headless abrange a ativação e definição dos Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para criar Eventos.
seo-description: O Capítulo 2 do tutorial AEM Headless abrange a ativação e definição dos Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para criar Eventos.
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '1917'
ht-degree: 0%

---


# Capítulo 2 - Infraestruturas

## Configurando uma infraestrutura de armazenamento em cache

Introduzimos a topologia básica de um sistema de Publicação e um dispatcher no Capítulo 1 desta série. Um conjunto de servidores do Publish e Dispatcher pode ser configurado em muitas variações - dependendo da carga esperada, da topologia de seu(s) data center(s) e das propriedades de failover desejadas.

Desenharemos as topologias mais comuns e descreveremos as vantagens e onde elas ficam aquém. A lista - é claro - nunca pode ser abrangente. O único limite é a sua imaginação.

### A configuração &quot;Legacy&quot;

Nos primeiros dias, o número de visitantes potenciais era pequeno, o hardware era caro e os servidores da Web não eram considerados tão críticos para os negócios quanto são hoje. Uma configuração comum era ter um Dispatcher funcionando como um balanceador de carga e cache na frente de dois ou mais sistemas de Publicação. O servidor Apache no núcleo do Dispatcher era muito estável e - na maioria das configurações - capaz de atender uma quantidade decente de solicitação.

![Configuração do Dispatcher &quot;herdada&quot; - Não é muito comum pelos padrões atuais](assets/chapter-2/legacy-dispatcher-setup.png)

*Configuração do Dispatcher &quot;herdada&quot; - Não é muito comum pelos padrões atuais*

<br> 

É aqui que o dispatcher recebeu seu nome de: Ele basicamente estava enviando solicitações. Essa configuração não é mais muito comum, pois não pode atender às demandas maiores de desempenho e estabilidade exigidas hoje.

### Configuração de várias legendas

Atualmente, uma topologia um pouco diferente é mais comum. Uma topologia de várias legendas teria um Dispatcher por servidor de publicação. Um balanceador de carga dedicado (hardware) fica na frente da infraestrutura do AEM, enviando as solicitações para essas duas (ou mais) pernas:

![Configuração moderna do Dispatcher &quot;Padrão&quot; - Fácil de manusear e manter](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configuração moderna do Dispatcher &quot;Padrão&quot; - Fácil de manusear e manter*

<br> 

Estas são as razões para este tipo de configuração.

1. Em média, os sites veiculam muito mais tráfego do que no passado. Assim, há a necessidade de aumentar a &quot;infraestrutura do Apache&quot;.

2. A configuração &quot;Legado&quot; não fornecia redundância no nível do Dispatcher. Se o servidor Apache falhou, o site inteiro não pôde ser acessado.

3. Os servidores Apache são baratos. Eles são baseados em código aberto e, dado que você tem um data center virtual, eles podem ser provisionados muito rápido.

4. Essa configuração fornece uma maneira fácil de um cenário de atualização &quot;acumulado&quot; ou &quot;escalonado&quot;. Você simplesmente encerra o Dispatcher 1 ao instalar um novo pacote de software no Publish 1. Quando a instalação for concluída e você tiver o Publish 1 testado com fumaça suficiente da rede interna, limpe o cache no Dispatcher 1 e inicie-o novamente enquanto derruba o Dispatcher 2 para manutenção do Publish 2.

5. A invalidação de cache torna-se muito fácil e determinística nessa configuração. Como apenas um sistema de Publicação está conectado a um Dispatcher, há apenas um Dispatcher para invalidar. A ordem e o tempo da invalidação são triviais.

### A configuração &quot;Expandir para fora&quot;

Os servidores Apache são baratos e fáceis de provisionar, por que não impulsionar o dimensionamento para fora desse nível um pouco mais. Por que não ter dois ou mais Dispatchers na frente de cada servidor de Publicação?

![Configuração &quot;Expandir&quot; - Tem algumas áreas de aplicativo, mas também limitações e avisos](assets/chapter-2/scale-out-setup.png)

*Configuração &quot;Expandir&quot; - Tem algumas áreas de aplicativo, mas também limitações e avisos*

<br> 

Você pode fazer isso! E há muitos cenários de aplicativo válidos para essa configuração. Mas também há algumas limitações e complexidades que você deve considerar.

#### Invalidação

Cada sistema de Publicação está conectado a uma grande variedade de Dispatchers, cada um deve ser invalidado quando o conteúdo foi alterado.

#### Manutenção

Escusado será dizer que a configuração inicial dos sistemas Dispatcher e Publish é um pouco mais complexa. Mas lembre-se também de que o esforço de uma versão &quot;em andamento&quot; também é um pouco maior. Os sistemas AEM podem e devem ser atualizados durante a execução. Mas é sensato não o fazer enquanto estão a servir ativamente pedidos. Geralmente, você deseja atualizar apenas uma parte dos sistemas de publicação, enquanto os outros ainda veiculam tráfego ativamente e depois, após os testes, alternar para a outra parte. Se tiver sorte e puder acessar o balanceador de carga em seu processo de implantação, você poderá desativar o roteamento para os servidores em manutenção aqui. Se você estiver em um balanceador de carga compartilhado sem acesso direto, é melhor desligar os dispatchers da Publicação que deseja atualizar. Quanto mais houver, mais você terá que fechar. Se houver um grande número e você estiver planejando atualizações frequentes, será recomendável usar alguma automação. Se você não tem ferramentas de automação, o dimensionamento é uma má ideia de qualquer maneira.

Em um projeto anterior, usamos um truque diferente para remover um sistema de Publicação do balanceamento de carga sem ter acesso direto ao próprio balanceador de carga.

O balanceador de carga geralmente &quot;pings&quot;, uma página específica para ver se o servidor está funcionando. Uma escolha trivial geralmente é fazer ping na página inicial. Mas se você quiser usar o ping para sinalizar o balanceador de carga para não equilibrar o tráfego, você escolheria outra coisa. Você cria um modelo ou servlet dedicado que pode ser configurado para responder com `"up"` ou `"down"` (no corpo ou como código de resposta http). A resposta dessa página, é claro, não deve ser armazenada em cache no dispatcher. Portanto, ela é sempre buscada recentemente no sistema de Publicação. Agora, se você configurar o balanceador de carga para verificar esse modelo ou servlet, poderá facilmente deixar a opção Publicar &quot;fingir&quot; estar inativa. Ele não faria parte do balanceamento de carga e pode ser atualizado.

#### Distribuição mundial

A &quot;Distribuição no mundo inteiro&quot; é uma configuração de &quot;Dimensionamento&quot; em que você tem vários Dispatchers na frente de cada sistema de Publicação - agora distribuídos em todo o mundo para estar mais perto do cliente e fornecer um melhor desempenho. É claro que nesse cenário você não tem um balanceador de carga central, mas um esquema de balanceamento de carga baseado em DNS e geo-IP.

>[!NOTE]
>
>Na verdade, você está criando um tipo de Rede de distribuição de conteúdo (CDN) com essa abordagem - portanto, você deve considerar comprar uma solução CDN pronta para uso em vez de criar uma você mesmo. Criar e manter um CDN personalizado não é tarefa trivial.

#### Escala horizontal

Mesmo em um datacenter local, uma topologia &quot;Dimensionar saída&quot; com vários Dispatchers na frente de cada sistema de publicação tem algumas vantagens. Se você vir gargalos de desempenho nos servidores Apache devido ao alto tráfego (e a uma boa taxa de acesso ao cache) e não conseguir mais dimensionar o hardware (adicionando CPUs, RAM e discos mais rápidos), você pode aumentar o desempenho adicionando Dispatchers. Isso é chamado de &quot;escala horizontal&quot;. No entanto, isso tem limites, especialmente quando você está invalidando o tráfego com frequência. Descreveremos o efeito na próxima seção.

#### Limites da Topologia de Expansão

A adição de servidores proxy normalmente aumenta o desempenho. Entretanto, há cenários em que a adição de servidores pode diminuir o desempenho. Como? Considere que você tem um portal de notícias, onde apresenta novos artigos e páginas a cada minuto. Um Dispatcher é invalidado por &quot;invalidação automática&quot;: Sempre que uma página é publicada, todas as páginas no cache do mesmo site são invalidadas. Esse é um recurso útil - cobrimos isso no [Capítulo 1](chapter-1.md) desta série - mas também significa que, quando você tem alterações frequentes no seu site, está invalidando o cache com frequência. Se você tiver apenas um Dispatcher por instância de publicação, o primeiro visitante solicitando uma página, acionará um novo cache dessa página. O segundo visitante já obtém a versão em cache.

Se você tiver dois Dispatchers, o segundo visitante terá 50% de chance de a página não ser armazenada em cache e ele experimentará uma latência maior quando essa página for renderizada novamente. Ter ainda mais Dispatchers por Publicação torna as coisas ainda piores. O que acontece é que o servidor de Publicação recebe mais carga porque ele precisa renderizar novamente a página para cada Dispatcher separadamente.

![Diminuição do desempenho em um cenário de dimensionamento com liberações frequentes de cache.](assets/chapter-2/decreased-performance.png)

*Diminuição do desempenho em um cenário de dimensionamento com liberações frequentes de cache.*

<br> 

#### Atenuando problemas de dimensionamento excessivo

Você pode considerar usar um armazenamento compartilhado central para todos os Dispatchers ou sincronizar os sistemas de arquivos dos servidores Apache para atenuar os problemas. Só podemos proporcionar uma experiência limitada em primeira mão, mas estar preparados para que isso aumente a complexidade do sistema e possa introduzir toda uma nova classe de erros.

Tivemos alguns experimentos com NFS - mas o NFS apresenta grandes problemas de desempenho devido ao bloqueio de conteúdo. Na verdade, isso diminuiu o desempenho geral.

**Conclusão**  - O compartilhamento de um sistema de arquivos comum entre vários dispatchers NÃO é uma abordagem recomendada.

Se você estiver enfrentando problemas de desempenho, amplie o Publish e o Dispatchers igualmente para evitar o pico de carga nas instâncias do Publisher. Não há uma regra de ouro sobre a taxa de Publicação/Dispatcher - ela depende muito da distribuição das solicitações e da frequência de publicações e invalidações de cache.

Se você também estiver preocupado com a latência que um visitante enfrenta, considere usar uma rede de entrega de conteúdo, a recuperação do cache, o aquecimento preemptivo do cache, definir um tempo de carência conforme descrito no [Capítulo 1](chapter-1.md) desta série ou consultar algumas ideias avançadas de [Parte 3](chapter-3.md).

### A configuração &quot;Cross Connected&quot;

Outra configuração que vimos de vez em quando é a configuração &quot;conectada cruzada&quot;: As instâncias de Publicação não têm Dispatchers dedicados, mas todos os Dispatchers estão conectados a todos os sistemas de Publicação.

![Topologia interconectada: Redundância aumentada e mais complexidade](assets/chapter-2/cross-connected-setup.png)

<br> 

*Topologia interconectada: Redundância aumentada e mais complexidade.*

À primeira vista, isso proporciona mais redundância para um orçamento relativamente pequeno. Quando um dos servidores do Apache estiver inativo, você ainda poderá ter dois sistemas Publish fazendo o trabalho de renderização. Além disso, se um dos sistemas de Publicação falhar, você ainda terá dois Dispatchers atendendo à carga em cache.

No entanto, isto tem um preço.

Primeiro, tirar uma perna para manutenção é muito complicado. Na verdade, foi para isso que este programa foi concebido; ser mais resiliente e manter-se a funcionar por todos os meios possíveis. Vimos planos de manutenção complicados sobre a forma de lidar com isso. Reconfigure o Dispatcher 2 primeiro, removendo a conexão cruzada. Reiniciando o Dispatcher 2. Desligando o Dispatcher 1, atualizando o Publish 1, ... e assim por diante. Você deve considerar cuidadosamente se isso for escalável até mais de duas pernas. Chegaremos à conclusão de que, na verdade, aumenta a complexidade, os custos e é uma fonte formidável de erro humano. Seria melhor automatizar isso. Então, verifique melhor se você realmente tem os recursos humanos para incluir essa tarefa de automação no cronograma do seu projeto. Embora você possa economizar alguns custos de hardware com isso, você pode gastar o dobro na equipe de TI.

Em segundo lugar, você pode ter algum aplicativo de usuário em execução no AEM que exija um logon. Você usa sessões fixas, para garantir que um usuário sempre seja operado da mesma instância do AEM, para que seja possível manter o estado da sessão nessa instância. Com essa configuração interconectada, você precisa garantir que as sessões aderentes estejam funcionando corretamente no balanceador de carga e nos Dispatchers. Não é impossível - mas você precisa estar ciente disso e adicionar algumas horas extras de configuração e teste, o que - novamente - pode nivelar a economia que você planejou ao salvar hardware.

### Conclusão

Não recomendamos que você use esse esquema de conexão cruzada como opção padrão. No entanto, se você decidir usá-lo, deverá avaliar cuidadosamente os riscos e os custos ocultos e planejar incluir a automação de configuração como parte do seu projeto.

## Próxima etapa

* [3 - Tópicos avançados de cache](chapter-3.md)
