---
title: Capítulo 3 - Tópicos avançados de cache
description: O Capítulo 3 do tutorial Demystified Cache do AEM Dispatcher aborda como superar as limitações discutidas no Capítulo 2.
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '6162'
ht-degree: 0%

---


# Capítulo 3 - Tópicos avançados de cache

*&quot;Há apenas duas coisas difíceis na Ciência da Computação: invalidação do cache e nomeação de itens.&quot;*

- PHIL KARLTON

## Visão geral

Esta é a Parte 3 de uma série de três partes para armazenamento em cache no AEM. Onde as duas primeiras partes se concentraram no armazenamento em cache http simples no Dispatcher e quais limitações existem. Essa parte discute algumas ideias sobre como superar essas limitações.

## Armazenamento em cache em geral

[O Capítulo 1](chapter-1.md) e o  [Capítulo 2](chapter-2.md) desta série incidiram principalmente sobre o Dispatcher. Explicámos as noções básicas, as limitações e onde é necessário fazer certas compensações.

A complexidade e as complexidades do armazenamento em cache não são problemas exclusivos do Dispatcher. O armazenamento em cache é difícil em geral.

Ter o Dispatcher como sua única ferramenta em sua caixa de ferramentas seria na verdade uma limitação real.

Neste capítulo, queremos ampliar ainda mais a nossa visão sobre armazenamento em cache e desenvolver algumas ideias sobre como você pode superar algumas das falhas do Dispatcher. Não há uma bala de prata - você terá que fazer compensações no seu projeto. Lembre-se, com a precisão de armazenamento em cache e invalidação sempre vem a complexidade, e com a complexidade há a possibilidade de erros.

Terão de fazer compensações nestas áreas.

* Desempenho e latência
* Consumo de recursos / Carga da CPU / Uso do disco
* Precisão / Moeda / Estabilidade / Segurança
* Simplicidade / Complexidade / Custo / Manutenção / Prontidão de erro

Estas dimensões estão interligadas num sistema bastante complexo. Não há um caso simples. Tornar um sistema mais simples pode torná-lo mais rápido ou mais lento. Ela pode reduzir os custos de desenvolvimento, mas aumentar os custos no helpdesk, por exemplo, se os clientes visualizarem conteúdo obsoleto ou reclamarem de um site lento. Todos estes fatores têm de ser considerados e equilibrados entre si. Mas, por agora, já deveríamos ter uma boa ideia, que não existe uma bala de prata ou uma única &quot;prática recomendada&quot; - apenas um monte de práticas ruins e algumas boas.

## Armazenamento em Cache Encadeado

### Visão geral

#### Fluxo de dados

Fornecer uma página de um servidor para o navegador de um cliente cruza uma grande variedade de sistemas e subsistemas. Se você olhar com atenção, há uma série de dados sobre o lúpulo que devem ser extraídos da fonte para o dreno, cada um dos quais é um potencial candidato para armazenamento em cache.

![Fluxo de dados de um aplicativo CMS típico](assets/chapter-3/data-flow-typical-cms-app.png)

*Fluxo de dados de um aplicativo CMS típico*

<br> 

Vamos iniciar nossa jornada com um pedaço de dados que fica em um disco rígido e que precisa ser exibido em um navegador.

#### Hardware e sistema operacional

Primeiro, a própria unidade de disco rígido (HDD) tem algum cache integrado no hardware. Segundo, o sistema operacional que monta o disco rígido usa memória livre para armazenar em cache blocos acessados com frequência para acelerar o acesso.

#### Repositório de conteúdo

O próximo nível é o CRX ou Oak - o banco de dados de documentos usado pelo AEM. O CRX e o Oak dividem os dados em segmentos que podem ser armazenados em cache na memória, assim como para evitar um acesso mais lento à unidade de disco rígido.

#### Dados de terceiros

A maioria das instalações maiores da Web tem dados de terceiros também; dados provenientes de um sistema de informações sobre produtos, de um sistema de gerenciamento de relação com o cliente, de um banco de dados herdado ou de qualquer outro serviço da Web arbitrário. Esses dados não precisam ser transferidos da fonte sempre que forem necessários, especialmente quando não forem conhecidos por mudarem com pouca frequência. Portanto, ele pode ser armazenado em cache, se não for sincronizado no banco de dados do CRX.

#### Camada de negócios - Aplicativo/Modelo

Geralmente, seus scripts de modelo não renderizam o conteúdo bruto proveniente do CRX por meio da API JCR. Provavelmente, há uma camada comercial entre essa que mescla, calcula e/ou transforma dados em um objeto de domínio comercial. Adivinhe o quê - se essas operações forem caras, você deve considerar armazená-las em cache.

#### Fragmentos de marcação

O modelo agora é a base para a renderização da marcação para um componente. Por que não armazenar em cache o modelo renderizado também?

#### Dispatcher, CDN e outros proxies

Off vai a HTML-Page renderizada para o Dispatcher. Já discutimos que o principal objetivo do Dispatcher é armazenar em cache páginas HTML e outros recursos da Web (apesar de seu nome). Antes de os recursos chegarem ao navegador, ele pode passar um proxy reverso - que pode armazenar em cache e um CDN - que também é usado para armazenamento em cache. O cliente pode se sentar em um escritório, que concede acesso à Web somente por meio de um proxy - e esse proxy pode decidir armazenar em cache também para salvar tráfego.

#### Cache do navegador

Por último, mas não menos importante: o navegador também armazena em cache. Este é um ativo facilmente ignorado. Mas é o cache mais próximo e mais rápido que você tem na cadeia de armazenamento em cache. Infelizmente - não é compartilhado entre usuários -, mas ainda entre solicitações diferentes de um usuário.

### Onde armazenar em cache e por que

É uma longa cadeia de caches potenciais. E todos nós temos enfrentado problemas onde vimos conteúdo desatualizado. Mas considerando quantos estágios existem, é um milagre que na maioria das vezes ele esteja funcionando.

Mas onde nessa cadeia faz sentido armazenar em cache? No começo? No fim? Em todo lugar? Depende... e depende de um grande número de fatores. Mesmo dois recursos no mesmo site podem desejar uma resposta diferente para essa pergunta.

Para vos dar uma ideia aproximada dos fatores que podem ser considerados.

**Tempo de vida**  - se os objetos tiverem um tempo de vida curto inerente (os dados de tráfego podem ter dados de tempo de vida mais curtos do que o tempo), talvez não valha a pena armazenar em cache.

**Custo de produção -** Quanto caro (em termos de ciclos de CPU e E/S) é a reprodução e entrega de um objeto. Se for cache barato, talvez não seja necessário.

**Tamanho**  - Objetos grandes exigem mais recursos para serem armazenados em cache. Isso poderia ser um fator limitante e deve ser contrabalançado com os benefícios.

**Frequência de acesso**  - se os objetos forem acessados raramente, o armazenamento em cache pode não ser efetivo. Eles ficariam obsoletos ou seriam invalidados antes de serem acessados na segunda vez do cache. Tais itens apenas bloqueariam os recursos de memória.

**Acesso compartilhado**  - Os dados usados por mais de uma entidade devem ser armazenados em cache mais acima da cadeia. Na verdade, a cadeia de armazenamento em cache não é uma cadeia, mas uma árvore. Um dado no repositório pode ser usado por mais de um modelo. Esses modelos, por sua vez, podem ser usados por mais de um script de renderização para gerar fragmentos HTML. Esses fragmentos são incluídos em várias páginas, que são distribuídas para vários usuários com seus caches privados no navegador. Então &quot;compartilhar&quot; não significa compartilhar apenas entre pessoas, mas entre softwares. Se você quiser encontrar um cache &quot;compartilhado&quot; em potencial, apenas rastreie a árvore de volta para a raiz e encontre um ancestral comum - é onde você deve armazenar em cache.

**Distribuição geoespacial**  - Se seus usuários estiverem distribuídos pelo mundo, o uso de uma rede distribuída de caches pode ajudar a reduzir a latência.

**Largura de banda de rede e latência**  - Falando de latência, quem são seus clientes e que tipo de rede eles estão usando? Talvez seus clientes sejam clientes de dispositivos móveis em um país subdesenvolvido usando a conexão 3G de smartphones da geração mais antiga? Considere a criação de objetos menores e armazene-os em cache nos caches do navegador.

Esta lista de longe não é abrangente, mas pensamos que já tenha a ideia.

### Regras básicas para armazenamento em cache em cadeia

Mais uma vez - o armazenamento em cache é difícil. Vamos compartilhar algumas regras básicas, que extraímos de projetos anteriores que podem ajudá-lo a evitar problemas em seu projeto.

#### Evite o armazenamento em cache duplo

Cada uma das camadas introduzidas no último capítulo fornece algum valor na cadeia de armazenamento em cache. Economizando ciclos de computação ou aproximando os dados do consumidor. Não é errado armazenar em cache um pedaço de dados em vários estágios da cadeia - mas você deve sempre considerar quais são os benefícios e os custos do próximo estágio. O armazenamento em cache de uma página inteira no sistema de Publicação geralmente não oferece nenhum benefício, pois isso já é feito no Dispatcher.

#### Misturar estratégias de invalidação

Existem três estratégias básicas de invalidação:

* **TTL, Time to Live:** um objeto expira após um período fixo (por exemplo, &quot;2 horas a partir de agora&quot;)
* **Data de expiração:** o objeto expira no momento definido no futuro (por exemplo, &quot;17:00 PM em 10 de junho de 2019&quot;)
* **Baseado em evento:** o objeto é invalidado explicitamente por um evento que aconteceu na plataforma (por exemplo, quando uma página é alterada e ativada)

Agora você pode usar estratégias diferentes em diferentes camadas de cache, mas há algumas &quot;tóxicas&quot;.

#### Invalidação baseada em evento

![Invalidação baseada em evento puro](assets/chapter-3/event-based-invalidation.png)

*Invalidação baseada em evento puro: Invalidar do cache interno para a camada externa*

<br> 

A invalidação pura baseada em eventos é a mais fácil de compreender, a mais fácil de se tornar teoricamente correta e a mais precisa.

Simplificando, os caches são invalidados um por um após a alteração do objeto.

Basta manter uma regra em mente:

Sempre invalidar do interior para o cache externo. Se você invalidou um cache externo primeiro, ele pode re-armazenar em cache conteúdo obsoleto de um cache interno. Não faça pressuposições em que momento um cache é atualizado novamente - verifique se está atualizado. Melhor, acionando a invalidação do cache externo _depois de_ invalidar o interno.

Essa é a teoria. Mas, na prática, há uma série de armadilhas. Os eventos devem ser distribuídos - possivelmente em uma rede. Na prática, isso torna o sistema de invalidação mais difícil de implementar.

#### Auto - Recuperação

Com a invalidação baseada em eventos, você deve ter um plano de contingência. E se um evento de invalidação for perdido? Uma estratégia simples poderia ser invalidar ou limpar após um determinado período. Então - você pode ter perdido esse evento e agora servir conteúdo obsoleto. Mas seus objetos também têm um TTL implícito de várias horas (dias) somente. Então, eventualmente o sistema se cura automaticamente.

#### Invalidação pura baseada em TTL

![Invalidação não sincronizada baseada em TTL](assets/chapter-3/ttl-based-invalidation.png)

*Invalidação não sincronizada baseada em TTL*

<br> 

Este também é um regime bastante comum. Você empilha várias camadas de caches, cada uma com direito de servir um objeto por um determinado período.

É fácil de implementar. Infelizmente, é difícil prever a vida útil efetiva de um pedaço de dados.

![Chace exterior que prolonga o plano de vida de um objeto interno](assets/chapter-3/outer-cache.png)

*O cache externo prolongamento da duração de um objeto interno*

<br> 

Considere a ilustração acima. Cada camada de armazenamento em cache apresenta um TTL de 2 minutos. Agora - o TTL geral também deve ser de 2 minutos, certo? Nem por isso. Se a camada externa buscar o objeto antes de ele ficar obsoleto, a camada externa na verdade prolonga o tempo de vida efetivo do objeto. O tempo de vida efetivo pode estar entre 2 e 4 minutos nesse caso. Considere que você concordou com seu departamento de negócios, um dia é tolerável - e você tem quatro camadas de caches. O TTL real em cada camada não deve ser superior a seis horas... aumentando a taxa de erros do cache...

Não estamos a dizer que se trata de um mau esquema. Você só deveria saber seus limites. E é uma estratégia fácil e boa para começar. Somente se o tráfego do site aumentar, você pode considerar uma estratégia mais precisa.

*Sincronização do tempo de invalidação definindo uma data específica*

#### Data de expiração com base na invalidação

Você obtém um tempo de vida efetivo mais previsível, se estiver definindo uma data específica no objeto interno e propagando isso para os caches externos.

![Sincronização de datas de expiração](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronização de datas de expiração*

<br> 

No entanto, nem todos os caches podem propagar as datas. E pode se tornar desagradável, quando o cache externo agrega dois objetos internos com datas de expiração diferentes.

#### Misturar invalidação baseada em eventos e baseada em TTL

![Misturar estratégias baseadas em eventos e baseadas em TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Misturar estratégias baseadas em eventos e baseadas em TTL*

<br> 

Além disso, um esquema comum no AEM é usar a invalidação baseada em eventos nos caches internos (por exemplo, caches em memória onde os eventos podem ser processados em tempo quase real) e os caches baseados em TTL no exterior, onde talvez você não tenha acesso a invalidação explícita.

No mundo do AEM, você teria um cache na memória para objetos de negócios e fragmentos de HTML nos sistemas de Publicação, que é invalidado, quando os recursos subjacentes são alterados e você propaga esse evento de alteração para o dispatcher, que também funciona com base em eventos. Na frente disso, você teria, por exemplo, um CDN baseado em TTL.

Ter uma camada de (curto) armazenamento em cache com base em TTL na frente de um Dispatcher poderia efetivamente atenuar um pico que normalmente ocorria após uma invalidação automática.

#### Misturar TTL - e invalidação baseada em evento

![Misturar TTL - e invalidação baseada em evento](assets/chapter-3/toxic.png)

*Tóxico: Misturar TTL - e invalidação baseada em evento*

<br> 

Esta combinação é tóxica. Nunca coloque o cache baseado em eventos e depois de um cache com base em TTL ou Expiry. Lembram-se do efeito de spill-over que tivemos na estratégia &quot;TTL puro&quot;? O mesmo efeito pode ser observado aqui. Somente que o evento de invalidação do cache externo já tenha ocorrido pode não acontecer novamente - nunca, Isso pode expandir o tempo de vida do objeto em cache para infinito.

![Combinado com base em TTL e em eventos: Spill-over para infinito](assets/chapter-3/infinity.png)

*Combinado com base em TTL e em eventos: Spill-over para infinito*

<br> 

## Cache parcial e armazenamento em cache na memória

Você pode entrar no estágio do processo de renderização para adicionar camadas de armazenamento em cache. Desde obter objetos de transferência remota de dados ou criar objetos comerciais locais até armazenar em cache a marcação renderizada de um único componente. Deixaremos implementações concretas em um tutorial posterior. Mas talvez você queira que já tenha implementado algumas dessas camadas de armazenamento em cache. Portanto, o mínimo que podemos fazer aqui é introduzir os princípios básicos - e ganhar.

### Palavras de aviso

#### Respeitar Controle de Acesso

As técnicas descritas aqui são bastante eficientes e um _must-has_ deve estar em cada caixa de ferramentas de desenvolvedor do AEM. Mas não fique muito animado, use-os sabiamente. Armazenar um objeto em um cache e compartilhá-lo com outros usuários em solicitações de acompanhamento significa, na verdade, contornar o controle de acesso. Isso geralmente não é um problema em sites voltados para o público, mas pode ser, quando um usuário precisa fazer logon antes de obter acesso.

Considere armazenar a marcação HTML de um menu principal de sites em um cache na memória para compartilhá-la entre várias páginas. Na verdade, esse é um exemplo perfeito para armazenar HTML parcialmente renderizado, já que a criação de uma navegação geralmente é cara, pois requer a passagem de muitas páginas.

Você não está compartilhando essa mesma estrutura de menu entre todas as páginas, mas também com todos os usuários, o que a torna ainda mais eficiente. Mas espere ... mas talvez haja alguns itens no menu que são reservados somente para um determinado grupo de usuários. Nesse caso, o armazenamento em cache pode se tornar um pouco mais complexo.

#### Armazenar em cache apenas objetos comerciais personalizados

Se algum - esse é o conselho mais importante, podemos dar a você:

>[!WARNING]
>
>Somente armazene em cache objetos que são seus, que são imutáveis, que você mesmo criou, que são superficiais e não têm referência de saída.

O que isso significa?

1. Você não sabe sobre o ciclo de vida planejado dos objetos de outras pessoas. Considere obter um resumo de uma referência a um objeto de solicitação e decida armazená-lo em cache. Agora, a solicitação terminou e o contêiner de servlet deseja reciclar esse objeto para a próxima solicitação recebida. Nesse caso, outra pessoa está mudando o conteúdo sobre o qual você pensou que tinha controle exclusivo. Não descarte isso - vimos algo assim acontecendo em um projeto. O cliente estava vendo outros dados de clientes em vez de seus próprios.

2. Desde que um objeto seja referenciado por uma cadeia de outras referências, ele não poderá ser removido do heap. Se você retiver um objeto supostamente pequeno em seu cache que faz referência a uma representação de 4 MB de uma imagem, você terá uma boa chance de ter problemas com vazamento de memória. Os caches deveriam se basear em referências fracas. Mas - as referências fracas não funcionam como se pode esperar. Essa é a melhor maneira absoluta de produzir um vazamento de memória e terminar com um erro de falta de memória. E - você não sabe qual é o tamanho da memória retida dos objetos estranhos, certo?

3. Especialmente no Sling, você pode adaptar (quase) cada objeto um ao outro. Considere que você coloca um recurso no cache. A próxima solicitação (com direitos de acesso diferentes) busca esse recurso e o adapta em um resourceResolver ou uma sessão para acessar outros recursos aos quais ele não teria acesso.

4. Mesmo que você crie um &quot;wrapper&quot; fino ao redor de um recurso do AEM, não deve armazená-lo em cache - mesmo que seja seu próprio e imutável. O objeto encapsulado seria uma referência (que nós proibimos antes) e se olharmos bem, isso basicamente cria os mesmos problemas descritos no último item.

5. Se quiser armazenar em cache, crie seus próprios objetos copiando dados primitivos em seus próprios objetos shallo. Talvez você queira vincular entre seus próprios objetos por referências. Por exemplo, talvez você queira armazenar em cache uma árvore de objetos. Isso é bom, mas somente armazena em cache objetos que você acabou de criar na mesma solicitação, e nenhum objeto que tenha sido solicitado de outro lugar (mesmo se for o espaço de nome do objeto &quot;seu&quot;). _Copiar_ objetos é a chave. E certifique-se de limpar toda a estrutura de objetos vinculados de uma só vez e evitar referências de entrada e saída à sua estrutura.

6. Sim - e mantenha seus objetos imutáveis. Propriedades privadas, somente e sem setters.

São muitas regras, mas vale a pena segui-las. Mesmo que você seja experiente e super inteligente e tenha tudo sob controle. O jovem colega do seu projeto acabou de se formar na universidade. Ele não sabe de todas essas armadilhas. Se não houver armadilhas, não há nada para evitar. Mantenha-o simples e compreensível.

### Ferramentas e bibliotecas

Esta série trata da compreensão de conceitos e capacitação para a criação de uma arquitetura que melhor se ajuste ao seu caso de uso.

Não estamos a promover nenhum instrumento em particular. Mas dê dicas de como avaliá-las. Por exemplo, o AEM tem um cache integrado simples com um TTL fixo desde a versão 6.0. Você deve usá-lo? Provavelmente não na publicação, onde um cache baseado em eventos segue na cadeia (dica: O Dispatcher). Mas pode ser por uma escolha decente para um Autor. Também há um cache HTTP feito pelo Adobe ACS commons que pode valer a pena considerar.

Ou você cria o seu próprio, com base em uma estrutura de armazenamento em cache madura, como [Ehcache](https://www.ehcache.org). Isso pode ser usado para armazenar em cache objetos Java e marcação renderizada (`String` objetos).

Em alguns casos simples, você também pode usar mapas de hash simultâneos - você verá limites rapidamente aqui - na ferramenta ou em suas habilidades. A simultaneidade é tão difícil de dominar quanto nomear e armazenar em cache.

#### Referências

* [Cache HTTP do ACS Commons  ](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Estrutura de armazenamento em cache do Ehcache](https://www.ehcache.org)

### Termos básicos

Não vamos entrar na teoria do armazenamento em cache muito fundo aqui, mas nos sentimos obrigados a fornecer algumas palavras-chave, para que você tenha um bom começo.

#### Despejo de cache

Falamos muito de invalidação e limpeza. _O_ despejo de cache está relacionado a estes termos: Depois de uma entrada, ela é despejada, ela não está mais disponível. Mas o despejo acontece não quando uma entrada está desatualizada, mas quando o cache está cheio. Itens mais recentes ou &quot;mais importantes&quot; extraem do cache itens mais antigos ou menos importantes. Quais entradas você terá que sacrificar é uma decisão caso a caso. Você pode despejar os mais antigos ou aqueles que foram usados muito raramente ou acessados por muito tempo.

#### Armazenamento em cache preventivo

O armazenamento em cache preventivo significa recriar a entrada com conteúdo novo no momento em que é invalidada ou considerada desatualizada. É claro que você faria isso somente com alguns recursos, que você tem certeza de que são acessados com frequência e imediatamente. Caso contrário, você desperdiçaria recursos ao criar entradas de cache que talvez nunca fossem solicitadas. Ao criar entradas de cache preventivamente, você poderia reduzir a latência da primeira solicitação para um recurso após a invalidação do cache.

#### Aquecimento de cache

O aquecimento do cache está intimamente relacionado ao armazenamento em cache preventivo. Embora você não usasse esse termo para um sistema dinâmico. E é menos tempo limitado do que o primeiro. Você não armazena em cache novamente imediatamente após a invalidação, mas preenche gradualmente o cache quando o tempo permitir.

Por exemplo, você retira um trecho Publicar / Dispatcher do balanceador de carga para atualizá-lo. Antes de reintegrá-lo, você rastreia automaticamente as páginas acessadas com mais frequência para colocá-las no cache novamente. Quando o cache é &quot;quente&quot; - preenchido adequadamente, você reintegra a perna no balanceador de carga.

Ou talvez você reintegre a perna de uma só vez, mas você diminui o tráfego para aquela perna para que ela tenha a chance de aquecer os caches por uso regular.

Ou talvez você também queira armazenar em cache algumas páginas acessadas com menos frequência em momentos em que o sistema está ocioso para diminuir a latência quando elas são realmente acessadas por solicitações reais.

#### Identidade do objeto de cache, carga, dependência de invalidação e TTL

Em geral, um objeto em cache ou uma &quot;entrada&quot; tem cinco propriedades principais,

#### Chave

Essa é a identidade pela qual você identifica e objetos. Para recuperar a carga útil ou para removê-la do cache. O dispatcher, por exemplo, usa o URL de uma página como chave. Observe que o dispatcher não usa os caminhos de páginas. Isso não é suficiente para diferenciar renderizações diferentes. Outros caches podem usar chaves diferentes. Veremos alguns exemplos mais tarde.

#### Valor / Carga

Esse é o tesouro do objeto, os dados que você deseja recuperar. No caso do dispatcher, é o conteúdo dos arquivos. Mas também pode ser uma árvore de objetos Java.

#### TTL

Já cobrimos o TTL. A hora após a qual uma entrada é considerada obsoleta e não deve mais ser entregue.

#### Dependência

Isso está relacionado à invalidação baseada em eventos. Em quais dados originais depende esse objeto? Na Parte I, já dissemos, que um rastreamento de dependência real e preciso é muito complexo. Mas com nosso conhecimento do sistema você pode aproximar as dependências com um modelo mais simples. Invalidamos objetos suficientes para limpar conteúdo obsoleto... e talvez inadvertidamente mais do que seria necessário. Mas ainda assim tentamos manter abaixo de &quot;limpar tudo&quot;.

Quais objetos dependem do que os outros são genuínos em cada aplicativo. Daremos alguns exemplos de como implementar posteriormente uma estratégia de dependência.

### Armazenamento em cache de fragmentos HTML

![Reutilização de um fragmento renderizado em páginas diferentes](assets/chapter-3/re-using-rendered-fragment.png)

*Reutilização de um fragmento renderizado em páginas diferentes*

<br> 

O HTML Fragment Caching é uma ferramenta poderosa. A ideia é armazenar em cache a marcação HTML gerada por um componente em um cache na memória. Você pode perguntar, por que devo fazer isso? Estou armazenando em cache toda a marcação da página no dispatcher de qualquer maneira - incluindo a marcação desse componente. Estamos de acordo. Você faz, mas uma vez por página. Você não está compartilhando essa marcação entre as páginas.

Imagine que você está renderizando uma navegação na parte superior de cada página. A marcação é semelhante em cada página. Mas você está renderizando repetidamente para cada página, que não está no Dispatcher. E lembre-se: Após a invalidação automática, todas as páginas precisam ser renderizadas novamente. Basicamente, você está executando o mesmo código com os mesmos resultados centenas de vezes.

De nossa experiência, renderizar uma navegação superior aninhada é uma tarefa muito cara. Geralmente, você percorre uma boa parte da árvore de documentos para gerar os itens de navegação. Mesmo que você só precise do título de navegação e do URL - as páginas precisam ser carregadas na memória. E aqui eles estão entupindo recursos preciosos. De novo e de novo.

Mas o componente é compartilhado entre muitas páginas. E o compartilhamento de algo é um indicador de uso de um cache. Portanto - o que você gostaria de fazer é verificar se o componente de navegação já foi renderizado e armazenado em cache e, em vez de renderizar novamente, emitir apenas o valor dos caches.

Há duas maravilhosas sutilezas desse esquema que facilmente perdemos:

1. Você está armazenando em cache uma Java String. Uma String não tem referências de saída e é imutável. Portanto, considerando os avisos acima - isto é super seguro.

2. A invalidação também é super fácil. Sempre que qualquer coisa alterar seu site, você deseja invalidar essa entrada de cache. A reconstrução é relativamente barata, pois precisa ser executada apenas uma vez e depois é reutilizada por todas as centenas de páginas.

Isso é um grande alívio para seus servidores de Publicação.

### Implementação de caches de fragmentos

#### Tags personalizadas

Antigamente, em que você usava o JSP como mecanismo de modelo, era bastante comum usar uma tag JSP personalizada envolvendo o código de renderização dos componentes.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

A tag personalizada que capturaria seu corpo e o gravaria no cache ou impediria a execução de seu corpo e produziria a carga útil da entrada do cache.

A &quot;Chave&quot; é o caminho de componentes que ele teria na página inicial. Não usamos o caminho do componente na página atual, pois isso criaria uma entrada de cache por página - o que contradiz nossa intenção de compartilhar esse componente. Também não estamos usando apenas o caminho relativo dos componentes (`jcr:conten/mainnavigation`), pois isso nos impediria de usar componentes de navegação diferentes em sites diferentes.

&quot;Cache&quot; é um indicador onde armazenar a entrada. Geralmente, você tem mais de um cache em que armazena itens. Cada um deles pode se comportar um pouco diferente. Portanto, é bom diferenciar o que está armazenado - mesmo que no final seja apenas cordas.

&quot;Dependência&quot; é isso que a entrada do cache depende. O cache &quot;main-navigation&quot; pode ter uma regra: se houver alguma alteração abaixo do nó &quot;depenency&quot;, a entrada correspondente deve ser eliminada. Dessa forma, sua implementação de cache precisaria se registrar como um ouvinte de eventos no repositório para estar ciente das alterações e, em seguida, aplicar as regras específicas do cache para descobrir o que precisa ser invalidado.

O acima foi apenas um exemplo. Você também pode optar por ter uma árvore de caches. Onde o primeiro nível é usado para separar sites (ou locatários) e o segundo nível, então se ramifica em tipos de conteúdo (por exemplo, &quot;navegação principal&quot;) - que podem sobrescrever a adição do caminho das páginas iniciais como no exemplo acima.

A propósito, também é possível usar essa abordagem com componentes mais modernos baseados em HTL. Em seguida, você teria um wrapper JSP ao redor do seu script HTL.

#### Filtros de componentes

Mas, em uma abordagem HTL pura, é preferível criar o cache de fragmentos com um filtro de componente do Sling. Ainda não vimos isso na natureza, mas essa é a abordagem que usaríamos nessa questão.

#### Sling Dynamic Include

O cache de fragmentos é usado se você tiver algo constante (a navegação) no contexto de um ambiente em alteração (páginas diferentes).

Mas você também pode ter o oposto, um contexto relativamente constante (uma página que raramente muda) e alguns fragmentos que mudam constantemente nessa página (por exemplo, um ticker ao vivo).

Nesse caso, você pode dar a [Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html) uma chance. Em essência, esse é um filtro de componente, que envolve o componente dinâmico e, em vez de renderizar o componente na página, cria uma referência. Essa referência pode ser uma chamada Ajax, para que o componente seja incluído pelo navegador e, portanto, a página ao redor possa ser estaticamente armazenada em cache. Ou - alternativamente - o Sling Dynamic Include pode gerar uma diretiva SSI (Server Side Include). Essa diretiva seria executada no servidor Apache. Você pode até mesmo usar as diretivas ESI - Edge Side Include se usar o Varnish ou um CDN compatível com scripts ESI.

![Diagrama de sequência de uma solicitação usando o Sling Dynamic Include](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagrama de sequência de uma solicitação usando o Sling Dynamic Include*

<br> 

A documentação do SDI informa que você deve desativar o armazenamento em cache para URLs que terminam em &quot;*.nocache.html&quot;, o que faz sentido, pois você está lidando com componentes dinâmicos.

Você poderá ver outra opção como usar o SDI: Se você _não_ desativar o cache do dispatcher para as inclusões, o Dispatcher atuará como um cache de fragmento semelhante ao que descrevemos no último capítulo: As páginas e os fragmentos de componente de maneira uniforme e independente são armazenados em cache no dispatcher e unidos pelo script SSI no servidor Apache quando a página é solicitada. Ao fazer isso, você poderia implementar componentes compartilhados como a navegação principal (considerando que você sempre usa o mesmo URL de componente).

Isso deveria funcionar - em teoria. Mas...

Aconselhamos a não fazer isso: Você perderia a capacidade de ignorar o cache dos componentes dinâmicos reais. O SDI é configurado globalmente e as alterações que você faria para o seu &quot;cache de fragmento-mans insatisfatório&quot; também se aplicariam aos componentes dinâmicos.

Recomendamos que você estude cuidadosamente a documentação do SDI. Existem algumas outras limitações, mas a SDI é uma ferramenta valiosa em alguns casos.

#### Referências

* [docs.oracle.com - Como gravar tags JSP personalizadas](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Criação e uso de filtros de componentes](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Configuração de Sling Dynamic Inclusions no AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Armazenamento de modelo em cache

![Armazenamento em cache baseado em modelo: Um objeto comercial com duas renderizações diferentes](assets/chapter-3/model-based-caching.png)

*Armazenamento em cache baseado em modelo: Um objeto comercial com duas renderizações diferentes*

<br> 

Voltemos ao caso com a navegação. Estávamos assumindo que cada página exigiria a mesma marcação da navegação.

Mas talvez não seja esse o caso. Talvez você queira renderizar uma marcação diferente para o item na navegação que representa a _página atual_.

```
Travel Destinations

<ul class="maninnav">
  <li class="currentPage">Travel Destinations
    <ul>
      <li>Finland
      <li>Canada
      <li>Norway
    </ul>
  <li>News
  <li>About us
<ul>
```

```
News

<ul class="maninnav">
  <li>Travel Destinations
  <li class="currentPage">News
    <ul>
      <li>Winter is coming>
      <li>Calm down in the wild
    </ul>
  <li>About us
<is
```

Essas são duas renderizações completamente diferentes. Ainda assim, o _objeto comercial_ - a árvore de navegação completa - é o mesmo.  O _objeto comercial_ aqui seria um gráfico de objetos representando os nós na árvore. Esse gráfico pode ser facilmente armazenado em um cache na memória. No entanto, lembre-se de que este gráfico não deve conter nenhum objeto ou referência a nenhum objeto que você mesmo não criou - especialmente agora os nós JCR.

#### Armazenamento em cache no navegador

Já tocamos na importância do armazenamento em cache no navegador, e há muitos bons tutoriais por aí. No final - para o navegador - o Dispatcher é apenas um servidor da Web que segue o protocolo HTTP.

No entanto - apesar da teoria - reunimos alguns conhecimentos que não encontrámos em outro lugar e que queremos partilhar.

Essencialmente, o armazenamento em cache do navegador pode ser aproveitado de duas maneiras diferentes,

1. O navegador tem um recurso em cache do qual sabe a data exata de expiração. Nesse caso, não solicita o recurso novamente.

2. O navegador tem um recurso, mas não tem certeza se ainda é válido. Nesse caso, ele perguntaria ao servidor da Web (o Dispatcher, no nosso caso). Dê-me o recurso se ele foi modificado desde a última vez que o entregou. Se não tiver sido alterado, o servidor responde com &quot;304 - não alterado&quot; e somente os metadados foram transmitidos.

#### Depuração

Se você estiver otimizando as configurações do Dispatcher para o armazenamento em cache do navegador, é extremamente útil usar um servidor proxy de desktop entre seu navegador e o servidor da Web. Nós preferimos &quot;Charles Web Debugging Proxy&quot; de Karl von Randow.

Usando o Charles, você pode ler as solicitações e respostas, que são transmitidas de e para o servidor. E - você pode aprender muito sobre o protocolo HTTP. Navegadores modernos também oferecem alguns recursos de depuração, mas os recursos de um proxy de desktop são inéditos. Você pode manipular os dados transferidos, limitar a transmissão, reproduzir solicitações únicas e muito mais. E a interface do usuário é claramente organizada e bastante abrangente.

O teste mais básico é usar o site como um usuário normal - com o proxy no meio - e verificar no proxy se o número de solicitações estáticas (para /etc/...) está diminuindo com o tempo - pois elas devem estar no cache e não devem mais ser solicitadas.

Encontramos, um proxy pode fornecer uma visão geral mais clara, já que a solicitação em cache não aparece no log, enquanto alguns depuradores incorporados do navegador ainda mostram essas solicitações com &quot;0 ms&quot; ou &quot;do disco&quot;. O que é correto e preciso, mas pode deixar a sua visão em nuvem um pouco.

Você pode então detalhar e verificar os cabeçalhos dos arquivos transferidos para ver, por exemplo, se os cabeçalhos http &quot;Expira&quot; estão corretos. Você pode repetir solicitações com cabeçalhos if-modified-since definidos para ver se o servidor responde corretamente com um código de resposta 304 ou 200. Você pode observar o tempo das chamadas assíncronas e também pode testar suas suposições de segurança em um certo grau. Lembre-se de termos dito para você não aceitar todos os seletores que não são esperados explicitamente? Aqui você pode jogar com o URL e os parâmetros e ver se o aplicativo se comporta bem.

Há apenas uma coisa que pedimos que você não faça, quando estiver depurando seu cache:

Não recarregue páginas no navegador!

Um &quot;recarregamento de navegador&quot;, um _simple-reload_, bem como um _forced-reload_ (&quot;_shift-reload_&quot;) não é o mesmo que uma solicitação de página normal. Uma simples solicitação de recarregamento define um cabeçalho

```
Cache-Control: max-age=0
```

E um Shift-Reload (mantendo a tecla &quot;Shift&quot; pressionada ao clicar no botão de recarregamento) normalmente define um cabeçalho de solicitação

```
Cache-Control: no-cache
```

Ambos os cabeçalhos têm efeitos semelhantes, mas um pouco diferentes, mas o mais importante é que diferem completamente de uma solicitação normal quando você abre um URL do slot do URL ou usa links no site. A navegação normal não define cabeçalhos de Controle de Cache, mas provavelmente um cabeçalho if-modified-since.

Portanto, se você deseja depurar o comportamento normal de navegação, faça exatamente o seguinte: _Navegue normalmente_. Usar o botão de recarregamento do seu navegador é a melhor maneira de não ver erros de configuração de cache em sua configuração.

Use seu proxy Charles para ver o que estamos falando. Sim - e enquanto estiver aberto - você pode reproduzir as solicitações logo ali. Não é necessário recarregar a partir do navegador.

## Teste de desempenho

Ao usar um proxy, você tem uma noção do comportamento de tempo das suas páginas. É claro que isso não é de longe um teste de desempenho.  Um teste de desempenho exigiria que vários clientes solicitassem suas páginas simultaneamente.

Um erro comum, que vimos com muita frequência, é que o teste de desempenho inclui apenas um número muito pequeno de páginas e essas páginas são entregues apenas do cache do Dispatcher.

Se você estiver promovendo seu aplicativo para o sistema ativo, a carga será completamente diferente do que você testou.

No sistema ativo, o padrão de acesso não é aquele pequeno número de páginas igualmente distribuídas que você tem em testes (página inicial e poucas páginas de conteúdo). O número de páginas é muito maior e as solicitações são distribuídas de forma muito desigual. E, é claro, as páginas ao vivo não podem ser veiculadas a 100% do cache: Há solicitações de invalidação provenientes do sistema de Publicação que invalidam automaticamente uma grande parte dos seus recursos preciosos.

Ah sim - e quando estiver reconstruindo o cache do Dispatcher, você descobrirá que o sistema de Publicação também se comporta de forma bastante diferente, dependendo se você solicita apenas algumas páginas - ou um número maior. Mesmo que todas as páginas sejam complexas de forma semelhante, o número delas representa uma função. Lembras-te do que dissemos sobre armazenamento em cache encadeado? Se você sempre solicitar o mesmo número pequeno de páginas, as chances são boas, de que os blocos de acordo com os dados brutos estejam no cache dos discos rígidos ou de que os blocos sejam armazenados em cache pelo sistema operacional. Além disso, há uma boa chance de que o Repositório tenha armazenado em cache o segmento de acordo na memória principal. Assim, a renderização é significativamente mais rápida do que quando você tinha outras páginas removendo umas das outras agora e depois de vários caches.

O armazenamento em cache é difícil, assim como o teste de um sistema que depende do armazenamento em cache. Então, o que você pode fazer para ter um cenário mais preciso da vida real?

Pensamos que seria necessário realizar mais de um teste e você precisaria fornecer mais de um índice de desempenho como medida da qualidade de sua solução.

Se você já tiver um site existente, meça o número de solicitações e como elas são distribuídas. Tente modelar um teste que use uma distribuição de solicitações semelhante. Adicionar alguma aleatoriedade não poderia doer. Você não precisa simular um navegador que carregaria recursos estáticos como JS e CSS - eles não importam. Eles são armazenados em cache no navegador ou no Dispatcher eventualmente e não correspondem à carga significativamente. Mas imagens referenciadas importam. Encontre a distribuição em arquivos de log antigos e modele um padrão de solicitação semelhante.

Agora faça um teste com seu Dispatcher não armazenando em cache. Esse é o seu pior cenário. Descubra em que carga máxima seu sistema está se tornando instável sob essas piores condições. Você também pode piorar a situação tirando algumas pernas do Dispatcher/Publish se desejar.

Em seguida, realize o mesmo teste com todas as configurações de cache necessárias para &quot;ativar&quot;. Aumente lentamente suas solicitações paralelas para aquecer o cache e ver quanto seu sistema pode levar sob essas melhores condições de caso.

Um cenário médio de caso seria executar o teste com o Dispatcher ativado, mas também com algumas invalidações acontecendo. Você pode simular isso tocando nos arquivos de status por um cronjob ou enviando as solicitações de invalidação em intervalos irregulares para o Dispatcher. Não se esqueça de limpar também alguns dos recursos invalidados não automaticamente de vez em quando.

Você pode variar o último cenário, aumentando as solicitações de invalidação e aumentando a carga.

Isso é um pouco mais complexo do que apenas um teste de carga linear - mas dá muito mais confiança em sua solução.

Você pode se afastar do esforço. Mas, por favor, faça um teste em caso de pior caso no sistema de publicação com um número maior de páginas (igualmente distribuídas) para ver os limites do sistema. Certifique-se de interpretar o número do melhor cenário e provisionar seus sistemas com espaço suficiente.