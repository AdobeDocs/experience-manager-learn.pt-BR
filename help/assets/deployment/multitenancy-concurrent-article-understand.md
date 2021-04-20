---
title: Noções básicas sobre multilocação e desenvolvimento simultâneo
description: Saiba mais sobre os benefícios, desafios e técnicas para gerenciar uma implementação de vários locatários com o Adobe Experience Manager Assets.
feature: Connected Assets
version: 6.5
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2024'
ht-degree: 0%

---


# Entendendo a multilocação e o desenvolvimento simultâneo {#understanding-multitenancy-and-concurrent-development}

## Introdução {#introduction}

Quando várias equipes estão implantando seu código nos mesmos ambientes do AEM, há práticas que devem seguir para garantir que as equipes possam trabalhar de forma tão independente quanto possível, sem pisar nos dedos das outras equipes. Embora nunca possam ser totalmente eliminadas, essas técnicas minimizarão as dependências entre equipes. Para que um modelo de desenvolvimento simultâneo seja bem-sucedido, uma boa comunicação entre as equipes de desenvolvimento é fundamental.

Além disso, quando várias equipes de desenvolvimento trabalham no mesmo ambiente AEM, é provável que haja algum grau de multilocação em jogo. Muito foi escrito sobre as considerações práticas de tentar suportar vários locatários em um ambiente do AEM, especialmente em torno dos desafios enfrentados ao gerenciar governança, operações e desenvolvimento. Este documento explora alguns dos desafios técnicos da implementação do AEM em um ambiente de vários locatários, mas muitas dessas recomendações se aplicarão a qualquer organização com várias equipes de desenvolvimento.

É importante observar que, embora o AEM possa suportar vários sites e até várias marcas sendo implantadas em um único ambiente, ele não oferece uma verdadeira multilocação. Algumas configurações de ambiente e recursos de sistemas sempre serão compartilhados em todos os sites implantados em um ambiente. Este documento fornece orientação para minimizar os impactos desses recursos compartilhados e oferece sugestões para simplificar a comunicação e a colaboração nessas áreas.

## Benefícios e desafios {#benefits-and-challenges}

Há muitos desafios para implementar um ambiente de vários locatários.

Dentre elas:

* Complexidade técnica adicional
* Aumento das despesas gerais de desenvolvimento
* Dependências entre organizações no recurso compartilhado
* Maior complexidade operacional

Apesar dos desafios enfrentados, a execução de um aplicativo de vários locatários tem benefícios, como:

* Redução dos custos de hardware
* Redução do tempo de comercialização para sites futuros
* Custos de implementação mais baixos para locatários futuros
* Arquitetura padrão e práticas de desenvolvimento em toda a empresa
* Uma base de código comum

Se a empresa exigir um multilocação real, com conhecimento zero de outros locatários e sem código, conteúdo ou autores comuns compartilhados, as instâncias de autor separadas serão a única opção viável. O aumento global do esforço de desenvolvimento deve ser comparado com as economias em infraestruturas e custos de licença, a fim de determinar se esta abordagem é a mais adequada.

## Técnicas de desenvolvimento {#development-techniques}

### Gerenciamento de dependências {#managing-dependencies}

Ao gerenciar as dependências do projeto Maven, é importante que todas as equipes usem a mesma versão de um determinado pacote OSGi no servidor. Para ilustrar o que pode dar errado quando os projetos Maven são mal gerenciados, apresentamos um exemplo:

O projeto A depende da versão 1.0 da biblioteca, foo; A versão 1.0 do foo é incorporada na implantação para o servidor. O projeto B depende da versão 1.1 da biblioteca, foo; A versão 1.1 do foo está incorporada em sua implantação.

Além disso, suponhamos que uma API tenha sido alterada nessa biblioteca entre as versões 1.0 e 1.1. Nesse momento, um desses dois projetos não funcionará mais corretamente.

Para dar resposta a esta preocupação, recomendamos que todos os projetos Maven sejam filhos de um projeto de reator-mãe. Este projeto de reator tem dois objetivos: permite a construção e implantação de todos os projetos em conjunto, se assim o desejar, e contém as declarações de dependência para todos os projetos-filho. O projeto pai define as dependências e suas versões, enquanto os projetos filho declaram apenas as dependências necessárias, herdando a versão do projeto pai.

Nesse cenário, se a equipe que trabalha no Projeto B exigir funcionalidade na versão 1.1 do foo, rapidamente se tornará evidente no ambiente de desenvolvimento que essa alteração quebrará o Projeto A. Neste ponto, as equipes podem discutir essa alteração e tornar o Projeto A compatível com a nova versão ou procurar uma solução alternativa para o Projeto B.

Observe que isso não elimina a necessidade de essas equipes compartilharem essa dependência; apenas destaca os problemas rápida e cedo para que as equipes possam discutir quaisquer riscos e concordar em uma solução.

### Evitando a duplicação de código {#preventing-code-duplication-nbsp-br}

Ao trabalhar em vários projetos, é importante garantir que o código não seja duplicado. A duplicação de código aumenta a probabilidade de ocorrências de defeito, o custo das alterações no sistema e a rigidez geral na base de código. Para evitar a duplicação, refatere a lógica comum em bibliotecas reutilizáveis que podem ser usadas em vários projetos.

Para dar suporte a essa necessidade, recomendamos o desenvolvimento e a manutenção de um projeto principal do qual todas as equipes possam depender e contribuir. Ao fazê-lo, é importante assegurar que este projeto principal não dependa, por sua vez, de qualquer projeto de equipes individuais; isso permite implantabilidade independente, além de promover a reutilização do código.

Alguns exemplos de código que normalmente em um módulo principal incluem:

* Configurações de todo o sistema, como:
   * Configurações do OSGi
   * Filtros de servlet
   * Mapeamentos do ResourceResolver
   * Gasodutos Sling Transformer
   * Manipuladores de erro (ou use o Manipulador de página de erro ACS AEM Commons1)
   * Servlets de autorização para armazenamento em cache sensível a permissão
* Classes de utilitário
* Lógica de negócios principal
* Lógica de integração de terceiros
* Criação de sobreposições da interface do usuário
* Outras personalizações necessárias para a criação, como widgets personalizados
* Iniciadores de fluxo de trabalho
* Elementos de design comuns usados em sites

*Arquitetura de projeto modular*

Isso não elimina a necessidade de várias equipes dependerem e possivelmente atualizarem o mesmo conjunto de códigos. Ao criar um projeto principal, diminuímos o tamanho da base de código compartilhada entre equipes, diminuindo, mas não eliminando a necessidade de recursos compartilhados.

Para garantir que as alterações feitas nesse pacote principal não interfiram na funcionalidade do sistema, recomendamos que um desenvolvedor sênior ou uma equipe de desenvolvedores mantenha a supervisão. Uma opção é ter uma única equipe que gerencie todas as alterações a este pacote; outra é fazer com que as equipes enviem solicitações de pull que são revisadas e mescladas por esses recursos. É importante que um modelo de governança seja projetado e aceito pelas equipes e que os desenvolvedores o sigam.

## Gerenciando Escopo de Implantação {#managing-deployment-scope}

À medida que equipes diferentes implantam seu código no mesmo repositório, é importante que elas não substituam as alterações umas das outras. O AEM tem um mecanismo para controlar isso ao implantar pacotes de conteúdo, o filtro . arquivo xml. É importante que não haja sobreposição entre os filtros.  arquivos xml; caso contrário, a implantação de uma equipe poderia apagar a implantação anterior de outra equipe. Para ilustrar este ponto, consulte os seguintes exemplos de arquivos de filtro bem-criados e problemáticos:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Se cada equipe configurar explicitamente o arquivo de filtro para o(s) site(s) em que está trabalhando, cada equipe poderá implantar seus componentes, bibliotecas de clientes e designs de site independentemente sem apagar as alterações umas das outras.

Como é um caminho de sistema global e não específico de um site, o seguinte servlet deve ser incluído no projeto principal, pois as alterações feitas aqui podem afetar qualquer equipe:

/apps/sling/servlet/errorhandler

### Sobreposições {#overlays}

As sobreposições são frequentemente usadas para estender ou substituir a funcionalidade predefinida do AEM, mas o uso de uma sobreposição afeta todo o aplicativo AEM (ou seja, quaisquer alterações de funcionalidade estarão disponíveis para todos os locatários). Isso seria ainda mais complicado se os locatários tivessem requisitos diferentes para a sobreposição. Idealmente, os grupos de negócios devem trabalhar em conjunto para concordar com a funcionalidade e a aparência dos consoles administrativos do AEM.

Se não for possível chegar a um consenso entre as várias unidades de negócios, uma possível solução seria simplesmente não usar sobreposições. Em vez disso, crie uma cópia personalizada da funcionalidade e a exponha por um caminho diferente para cada locatário. Isso permite que cada locatário tenha uma experiência do usuário completamente diferente, mas essa abordagem também aumenta o custo da implementação e dos esforços de atualização subsequentes.

### Inicializadores do fluxo de trabalho {#workflow-launchers}

O AEM usa inicializadores de fluxo de trabalho para acionar automaticamente a execução do fluxo de trabalho quando alterações especificadas são feitas no repositório. O AEM fornece vários iniciadores prontos, por exemplo, para executar a geração de representação e os processos de extração de metadados em ativos novos e atualizados. Embora seja possível deixar esses lançadores como estão, em um ambiente de vários locatários, se os locatários tiverem requisitos diferentes de iniciador e/ou modelo de fluxo de trabalho, é provável que os lançadores individuais precisem ser criados e mantidos para cada locatário. Esses iniciadores precisarão ser configurados para serem executados nas atualizações do locatário, deixando o conteúdo de outros locatários intocado. Isso pode ser feito facilmente aplicando lançadores a caminhos de repositório especificados que são específicos do locatário.

### URLs personalizadas {#vanity-urls}

O AEM fornece a funcionalidade de URL personalizada que pode ser definida por página. A preocupação com essa abordagem em um cenário de vários locatários é que o AEM não garante exclusividade entre os URLs personalizados configurados dessa maneira. Se dois usuários diferentes configurarem o mesmo caminho personalizado para páginas diferentes, um comportamento inesperado poderá ser encontrado. Por isso, recomendamos usar regras mod_rewrite nas instâncias do Apache Dispatcher, que permitem um ponto central de configuração em conjunto com regras do Resolvedor de Recursos somente de saída.

### Grupos de componentes {#component-groups}

Ao desenvolver componentes e modelos para vários grupos de criação, é importante fazer uso eficaz das propriedades componentGroup e allowedPaths. Ao aproveitá-los com eficiência com designs de site, podemos garantir que os autores da Marca A vejam apenas os componentes e modelos que foram criados para seu site, enquanto os autores da Marca B só visualizam os deles.

### Testes {#testing}

Embora uma boa arquitetura e canais de comunicação abertos possam ajudar a impedir a introdução de defeitos em áreas inesperadas do site, essas abordagens não são infalíveis. Por isso, é importante testar totalmente o que está sendo implantado na plataforma antes de lançar algo na produção. Isso requer a coordenação entre as equipes em seus ciclos de lançamento e reforça a necessidade de um conjunto de testes automatizados que cubram o máximo possível de funcionalidade. Além disso, como um sistema será compartilhado por várias equipes, o desempenho, a segurança e o teste de carga se tornam mais importantes do que nunca.

## Considerações operacionais {#operational-considerations}

### Recursos compartilhados {#shared-resources}

O AEM é executado em uma única JVM; quaisquer aplicativos AEM implantados compartilham recursos inerentemente uns com os outros, além dos recursos já consumidos na execução normal do AEM. No próprio espaço da JVM, não haverá separação lógica de threads, e os recursos finitos disponíveis ao AEM, como memória, CPU e e e/s de disco também serão compartilhados. Qualquer locatário que consuma recursos afetará inevitavelmente outros locatários do sistema.

### Show {#performance}

Caso não siga as práticas recomendadas do AEM, é possível desenvolver aplicativos que consomem recursos além do que é considerado normal. Exemplos disso são o acionamento de muitas operações pesadas de fluxo de trabalho (como o Ativo de atualização do DAM), o uso de operações de push-on-modify do MSM em vários nós ou o uso de queries JCR caros para renderizar conteúdo em tempo real. Elas inevitavelmente terão um impacto no desempenho de outros aplicativos de locatários.

### Logs {#logging}

O AEM fornece interfaces prontas para uso para uma configuração de logger robusta, que pode ser usada para nossa vantagem em cenários de desenvolvimento compartilhados. Ao especificar registradores separados para cada marca, por nome de pacote, podemos alcançar algum grau de separação de log. Embora operações em todo o sistema, como replicação e autenticação, ainda sejam registradas em um local central, o código personalizado não compartilhado pode ser registrado separadamente, facilitando os esforços de monitoramento e depuração para a equipe técnica de cada marca.

### Backup e restauração {#backup-and-restore}

Devido à natureza do repositório JCR, os backups tradicionais funcionam em todo o repositório, em vez de em um caminho de conteúdo individual. Portanto, não é possível separar facilmente os backups por locatário. Por outro lado, a restauração a partir de um backup reverterá o conteúdo e os nós do repositório para todos os locatários do sistema. Embora seja possível realizar backups de conteúdo direcionados, usando ferramentas como o VLT, ou filtrar o conteúdo para restauração por meio da criação de um pacote em um ambiente separado, esses\
as abordagens não abrangem facilmente as configurações ou a lógica do aplicativo e podem ser complicadas de gerenciar.

## Considerações de segurança {#security-considerations}

### ACLs {#acls}

É possível, é claro, usar Listas de Controle de Acesso (ACLs) para controlar quem tem acesso para visualizar, criar e excluir conteúdo com base em caminhos de conteúdo, o que requer a criação e o gerenciamento de grupos de usuários. A dificuldade em manter as ACLs e os grupos depende da ênfase em garantir que cada locatário tenha zero conhecimento de outros e se os aplicativos implantados dependam de recursos compartilhados. Para garantir uma ACL eficiente, administração de usuários e grupos, recomendamos ter um grupo centralizado com a supervisão necessária para garantir que esses controles e entidades de acesso se sobreponham (ou não se sobreponham) de uma forma que promova a eficiência e a segurança.
