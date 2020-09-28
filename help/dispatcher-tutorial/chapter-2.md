---
title: Capítulo 2 - Definição de modelos de fragmento do conteúdo do Evento
seo-title: Introdução ao AEM Content Services - Capítulo 2 - Definição de modelos de fragmento de conteúdo do Evento
description: O Capítulo 2 do tutorial sem cabeçalho AEM abrange a ativação e a definição de Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para a criação de Eventos.
seo-description: O Capítulo 2 do tutorial sem cabeçalho AEM abrange a ativação e a definição de Modelos de fragmento de conteúdo usados para definir uma estrutura de dados normalizada e uma interface de criação para a criação de Eventos.
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '1917'
ht-degree: 0%

---


# Capítulo 2 - Infraestruturas

## Configuração de uma infraestrutura de cache

Introduzimos a topologia básica de um sistema de publicação e de um expedidor no Capítulo 1 desta série. Um conjunto de servidores Publish e Dispatcher pode ser configurado em muitas variações - dependendo da carga esperada, da topologia do(s) data center(s) e das propriedades de failover desejadas.

Desenharemos as topologias mais comuns e descreveremos as vantagens e onde elas ficam aquém. A lista - é claro - nunca pode ser abrangente. O único limite é a sua imaginação.

### A configuração &quot;Legacy&quot;

Nos primeiros dias, o número de visitantes potenciais era pequeno, o hardware era caro e os servidores da Web não eram considerados tão críticos para os negócios como são hoje. Uma configuração comum era ter um Dispatcher funcionando como balanceador de carga e cache em frente a dois ou mais sistemas de publicação. O servidor Apache no núcleo do Dispatcher era muito estável e - na maioria das configurações - capaz de atender uma quantidade razoável de solicitação.

![Configuração do Dispatcher &quot;herdado&quot; - não muito comum pelos padrões atuais](assets/chapter-2/legacy-dispatcher-setup.png)

*Configuração do Dispatcher &quot;herdado&quot; - não muito comum pelos padrões atuais*

<br> 

Este é o local de onde o expedidor recebeu seu nome: Era basicamente enviando pedidos. Essa configuração não é mais comum, pois não pode mais atender às demandas mais altas de desempenho e estabilidade exigidas hoje.

### Configuração com várias pernas

Hoje uma topologia um pouco diferente é mais comum. Uma topologia de várias pernas teria um Dispatcher por servidor de publicação. Um balanceador de carga dedicado (hardware) fica na frente da infraestrutura AEM enviando as solicitações para essas duas (ou mais) pernas:

![Configuração moderna do Dispatcher &quot;Standard&quot; - Fácil de manusear e manter](assets/chapter-2/modern-standard-dispatcher-setup.png)

*Configuração moderna do Dispatcher &quot;Standard&quot; - Fácil de manusear e manter*

<br> 

Aqui estão as razões para este tipo de configuração.

1. Em média, os sites servem muito mais tráfego do que no passado. Assim, é necessário aumentar a &quot;infraestrutura do Apache&quot;.

2. A configuração &quot;Herdado&quot; não fornecia redundância no nível do Dispatcher. Se o Apache Server falhou, o site inteiro estava inacessível.

3. Os servidores Apache são baratos. Eles são baseados em código aberto e, dado que você tem um data center virtual, eles podem ser provisionados muito rápido.

4. Esta configuração fornece uma maneira fácil de um cenário de atualização &quot;acumulado&quot; ou &quot;estagnado&quot;. Basta encerrar o Dispatcher 1 ao instalar um novo pacote de software no Publicar 1. Quando a instalação for concluída e você tiver o Publish 1 suficientemente testado contra fumaça da rede interna, limpe o cache no Dispatcher 1 e start-o novamente enquanto derruba o Dispatcher 2 para manutenção do Publish 2.

5. A invalidação do cache torna-se muito fácil e determinista nesta configuração. Como apenas um sistema de publicação está conectado a um Dispatcher, há apenas um Dispatcher para invalidar. A ordem e o momento da invalidação são triviais.

### A configuração &quot;Scale Out&quot;

Os servidores Apache são baratos e fáceis de provisionar, por que não impulsionar o dimensionamento para fora desse nível um pouco mais. Por que não ter dois ou mais Dispatchers na frente de cada servidor de publicação?

![Configuração &quot;Expandir&quot; - tem algumas áreas de aplicativo, mas também limitações e limitações](assets/chapter-2/scale-out-setup.png)

*Configuração &quot;Expandir&quot; - tem algumas áreas de aplicativo, mas também limitações e limitações*

<br> 

Você pode fazer isso! E há muitos cenários válidos para essa configuração. Mas há também algumas limitações e complexidades que você deve considerar.

#### Invalidação

Cada sistema de publicação está conectado a uma grande variedade de distribuidores, cada um deve ser invalidado quando o conteúdo for alterado.

#### Manutenção

Escusado será dizer que a configuração inicial dos sistemas Dispatcher e Publish é um pouco mais complexa. Mas lembre-se também de que o esforço de uma versão &quot;rolante&quot; também é um pouco maior. Os sistemas AEM podem e devem ser atualizados durante a execução. Mas é sensato não fazer isso enquanto estão a servir ativamente pedidos. Geralmente, você deseja atualizar apenas uma parte dos sistemas de publicação - enquanto os outros ainda servem tráfego ativamente e, depois dos testes, alternam para a outra parte. Se tiver sorte e puder acessar o balanceador de carga em seu processo de implantação, você poderá desativar o roteamento nos servidores em manutenção aqui. Se você estiver em um balanceador de carga compartilhado sem acesso direto, é melhor desligar os despachantes do Publicar que deseja atualizar. Quanto mais houver, mais você terá que fechar. Se houver um grande número de atualizações e você estiver planejando atualizações frequentes, alguma automação será aconselhada. Se você não tem ferramentas de automação, a redução é uma má ideia de qualquer forma.

Em um projeto passado, usamos um truque diferente para remover um sistema de publicação do balanceamento de carga sem ter acesso direto ao próprio balanceador de carga.

O balanceador de carga geralmente &quot;ping&quot;, uma página específica para ver se o servidor está funcionando. Uma escolha trivial geralmente é tocar na página inicial. Mas se você quiser usar o ping para sinalizar o balanceador de carga para não equilibrar o tráfego, você escolheria outra coisa. Você cria um modelo ou servlet dedicado que pode ser configurado para responder com `"up"` ou `"down"` (no corpo ou como código de resposta http). A resposta dessa página, claro, não deve ser armazenada em cache no dispatcher - portanto, ela é sempre obtida recentemente do sistema de publicação. Agora, se você configurar o balanceador de carga para verificar este modelo ou servlet, você pode facilmente deixar o Publicar &quot;fingir&quot; que está inativo. Ele não faria parte do balanceamento de carga e poderia ser atualizado.

#### Distribuição no mundo inteiro

A &quot;Distribuição no mundo inteiro&quot; é uma configuração de &quot;Expandir&quot;, na qual você tem vários Dispatchers em frente a cada sistema de publicação - agora distribuídos no mundo inteiro para se aproximar do cliente e oferecer um melhor desempenho. Claro, nesse cenário você não tem um balanceador de carga central, mas um esquema de balanceamento de carga baseado em DNS e geo-IP.

>[!NOTE]
>
>Na verdade, você está construindo uma espécie de Rede de Distribuição de Conteúdo (CDN) com essa abordagem - então você deve considerar comprar uma solução de CDN pronta para ser usada em vez de construir uma sozinho. Construir e manter um CDN personalizado não é uma tarefa trivial.

#### Escala horizontal

Mesmo em um datacenter local, uma topologia &quot;Expandir&quot;, que apresenta algumas vantagens para vários Dispatchers na frente de cada sistema de publicação. Se você perceber gargalos no desempenho dos servidores Apache devido ao alto tráfego (e a uma boa taxa de ocorrência de cache) e não conseguir mais aumentar o hardware (adicionando CPUs, RAM e discos mais rápidos), você pode aumentar o desempenho adicionando os Dispatchers. Isso é chamado de &quot;escala horizontal&quot;. No entanto, isso tem limites - especialmente quando você está invalidando o tráfego com frequência. Descreveremos o efeito na próxima seção.

#### Limites da topologia de &quot;Scale Out&quot;

A adição de servidores proxy normalmente aumenta o desempenho. Entretanto, há cenários em que a adição de servidores pode diminuir o desempenho. Como? Considere que você tem um portal de notícias, onde você apresenta novos artigos e páginas a cada minuto. Um Dispatcher invalida por &quot;invalidação automática&quot;: Sempre que uma página é publicada, todas as páginas no cache do mesmo site são invalidadas. Este é um recurso útil - nós abordamos isso no [Capítulo 1](chapter-1.md) desta série - mas também significa que, quando você tem mudanças frequentes em seu site, você está invalidando o cache com muita frequência. Se você tiver apenas uma instância do Dispatcher por Publicação, o primeiro visitante que solicitar uma página acionará um novo cache dessa página. O segundo visitante já obtém a versão em cache.

Se você tiver dois Dispatchers, o segundo visitante terá 50% de chance de a página não ser armazenada em cache e ele experimentará uma latência maior quando a página for renderizada novamente. Ter ainda mais Dispatchers por Publicação torna as coisas ainda piores. O que acontece é que o servidor de publicação recebe mais carga porque ele precisa renderizar novamente a página para cada Dispatcher separadamente.

![Diminuição do desempenho em um cenário de expansão com frequentes liberações de cache.](assets/chapter-2/decreased-performance.png)

*Diminuição do desempenho em um cenário de expansão com frequentes liberações de cache.*

<br> 

#### Redução de problemas de sobredimensionamento

Você pode considerar usar um armazenamento compartilhado central para todos os Dispatchers ou sincronizar os sistemas de arquivos dos servidores Apache para atenuar os problemas. Só podemos proporcionar uma experiência limitada em primeira mão, mas estamos preparados para que isso contribua para a complexidade do sistema e possa introduzir toda uma nova classe de erros.

Tivemos alguns experimentos com NFS - mas o NFS apresenta grandes problemas de desempenho devido ao bloqueio de conteúdo. Isso na verdade diminuiu o desempenho geral.

**Conclusão** - O compartilhamento de um sistema de arquivos comum entre vários despachantes NÃO é uma abordagem recomendada.

Se você estiver enfrentando problemas de desempenho, amplie o Publicar e o Dispatchers igualmente para evitar o pico de carga nas instâncias do Publisher. Não há regra de ouro sobre a taxa de Publicação/Dispatcher - ela depende muito da distribuição das solicitações e da frequência de publicações e invalidações de cache.

Se você também estiver preocupado com a latência que um visitante apresenta, considere usar uma rede de delivery de conteúdo, rebuscar o cache, pré-aquecimento do cache, definir um tempo de carência conforme descrito no [Capítulo 1](chapter-1.md) desta série ou consultar algumas ideias avançadas da [Parte 3](chapter-3.md).

### A Configuração &quot;Cross Connected&quot;

Outra configuração que temos visto de vez em quando é a configuração &quot;cross-connected&quot;: As instâncias de Publicação não têm Dispatchers dedicados, mas todos os Dispatchers estão conectados a todos os sistemas de Publicação.

![Topologia interconectada: Maior redundância e mais complexidade](assets/chapter-2/cross-connected-setup.png)

<br> 

*Topologia interconectada: Maior redundância e mais complexidade.*

À primeira vista, isso proporciona mais redundância para um orçamento relativamente pequeno. Quando um dos servidores do Apache está inativo, você ainda pode ter dois sistemas de publicação que estão fazendo a renderização. Além disso, se um dos sistemas de publicação falhar, você ainda terá dois Dispatchers servindo a carga em cache.

No entanto, isso tem um preço.

Primeiro, tirar uma perna para manutenção é muito complicado. Na verdade, este programa foi projetado para isso. Ser mais resiliente e manter-se a funcionar por todos os meios possíveis. Temos visto planos de manutenção complicados sobre como lidar com isso. Reconfigure o Dispatcher 2 primeiro, removendo a conexão cruzada. Reiniciando o Dispatcher 2. Desligando o Dispatcher 1, atualizando o Publicar 1, ... e assim por diante. Você deve considerar cuidadosamente se isso aumentar até mais de duas pernas. Vocês chegarão à conclusão de que isso na verdade aumenta a complexidade, os custos e é uma fonte formidável de erro humano. Seria melhor automatizar isso. Então, verifique melhor, se você realmente tem os recursos humanos para incluir essa tarefa de automação na programação do seu projeto. Embora você possa economizar alguns custos de hardware com isso, você pode gastar duplo na equipe de TI.

Segundo, você pode ter algum aplicativo de usuário em execução no AEM que requer um logon. Você usa sessões aderentes para garantir que um usuário sempre seja servido da mesma instância AEM para que você possa manter o estado da sessão nessa instância. Com essa configuração conectada cruzada, é necessário verificar se as sessões adesivas estão funcionando corretamente no balanceador de carga e nos Dispatchers. Não é impossível - mas você precisa estar ciente disso e adicionar algumas configurações e horas de teste adicionais, que - novamente - podem nivelar as economias que você planejou ao salvar hardware.

### Conclusão

Não recomendamos que você use esse esquema de conexão cruzada como opção padrão. No entanto, se você decidir usá-la, precisará avaliar os riscos e os custos ocultos e planejar incluir a automação de configuração como parte do seu projeto.

## Próxima etapa

* [3 - Tópicos avançados de cache](chapter-3.md)
