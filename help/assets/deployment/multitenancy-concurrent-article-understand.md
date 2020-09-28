---
title: Compreensão da multitenacidade e do desenvolvimento simultâneo
seo-title: Compreensão da multitenacidade e do desenvolvimento simultâneo
description: 'null'
seo-description: 'null'
uuid: 682093fe-ce55-4ef8-af10-99f7062f8b1b
discoiquuid: 0dfcdf39-7423-459f-8f35-ee5b4b829f2c
feature: connected-assets
topics: authoring, operations, sharing, publishing
audience: all
doc-type: article
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 99f2a8cdfe0b4f5f6f1a149d96affd2a9e8bcf75
workflow-type: tm+mt
source-wordcount: '2009'
ht-degree: 0%

---


# Compreensão da multitenacidade e do desenvolvimento simultâneo {#understanding-multitenancy-and-concurrent-development}

## Introdução {#introduction}

Quando várias equipes estão implantando seu código nos mesmos ambientes AEM, há práticas que devem seguir para garantir que as equipes possam trabalhar de forma tão independente quanto possível, sem pisar nos dedos das outras equipes. Embora nunca possam ser totalmente eliminadas, essas técnicas minimizarão as dependências entre equipes. Para que um modelo de desenvolvimento simultâneo seja bem-sucedido, uma boa comunicação entre as equipes de desenvolvimento é fundamental.

Além disso, quando várias equipes de desenvolvimento trabalham no mesmo ambiente AEM, é provável que haja algum grau de multitenacidade em jogo. Muito foi escrito sobre as considerações práticas de tentar suportar vários locatários em um ambiente AEM, especialmente em torno dos desafios enfrentados ao gerenciar governança, operações e desenvolvimento. Este documento analisa alguns dos desafios técnicos da implementação de AEM em um ambiente de vários locatários, mas muitas dessas recomendações se aplicarão a qualquer organização com várias equipes de desenvolvimento.

É importante observar desde já que, embora AEM possa suportar vários sites e até mesmo várias marcas sendo implantadas em um único ambiente, isso não oferta uma verdadeira multitenacidade. Alguns recursos de sistemas e configurações de ambientes sempre serão compartilhados em todos os sites implantados em um ambiente. Este documento fornece orientações para minimizar os impactos desses recursos compartilhados e sugestões de ofertas para simplificar a comunicação e a colaboração nessas áreas.

## Benefícios e desafios {#benefits-and-challenges}

Há muitos desafios para implementar um ambiente de vários locatários.

Dentre elas:

* Complexidade técnica adicional
* Aumento da sobrecarga de desenvolvimento
* Dependências entre organizações no recurso compartilhado
* Maior complexidade operacional

Apesar dos desafios enfrentados, executar um aplicativo de vários locatários tem benefícios, como:

* Custos reduzidos de hardware
* Redução do tempo de comercialização para sites futuros
* Custos de implementação mais baixos para futuros locatários
* Arquitetura padrão e práticas de desenvolvimento em toda a empresa
* Uma base de códigos comum

Se os negócios exigirem uma verdadeira multilocalidade, sem conhecimento de outros locatários e sem código, conteúdo ou autores comuns compartilhados, as instâncias de autor separadas serão a única opção viável. O aumento global do esforço de desenvolvimento deve ser comparado com a economia em custos de infraestrutura e licença para determinar se essa abordagem é a mais adequada.

## Técnicas de desenvolvimento {#development-techniques}

### Gerenciamento de dependências {#managing-dependencies}

Ao gerenciar as dependências do projeto Maven, é importante que todas as equipes usem a mesma versão de um determinado pacote OSGi no servidor. Para ilustrar o que pode dar errado quando os projetos Maven são mal gerenciados, apresentamos um exemplo:

O projeto A depende da versão 1.0 da biblioteca, foo; a versão 1.0 do foo é incorporada em sua implantação no servidor. O projeto B depende da versão 1.1 da biblioteca, foo; a versão 1.1 do foo está incorporada em sua implantação.

Além disso, vamos supor que uma API foi alterada nesta biblioteca entre as versões 1.0 e 1.1. Neste momento, um destes dois projetos deixará de funcionar corretamente.

Para dar resposta a esta preocupação, recomendamos que todos os projetos Maven sejam filhos de um projeto de reator-mãe. Este projeto de reator tem dois objetivos: permite a construção e implantação de todos os projetos em conjunto, se assim o desejar, e contém as declarações de dependência para todos os projetos-filho. O projeto pai define as dependências e suas versões, enquanto os projetos filho declaram somente as dependências que exigem, herdando a versão do projeto pai.

Nesse cenário, se a equipe que trabalha no Projeto B exigir funcionalidade na versão 1.1 do foo, rapidamente se tornará evidente no ambiente de desenvolvimento que essa alteração quebrará o Projeto A. Nesse ponto, as equipes podem discutir essa mudança e tornar o Projeto A compatível com a nova versão ou procurar uma solução alternativa para o Projeto B.

Observe que isso não elimina a necessidade dessas equipes compartilharem essa dependência, apenas destaca os problemas de forma rápida e rápida, para que as equipes possam discutir quaisquer riscos e concordar com uma solução.

### Impedindo a duplicação de código {#preventing-code-duplication-nbsp-br}

Ao trabalhar em vários projetos, é importante garantir que o código não seja duplicado. A duplicação de códigos aumenta a probabilidade de ocorrências de defeitos, o custo de alterações no sistema e a rigidez geral na base de códigos. Para evitar a duplicação, reformate a lógica comum em bibliotecas reutilizáveis que podem ser usadas em vários projetos.

Para dar suporte a essa necessidade, recomendamos o desenvolvimento e a manutenção de um projeto principal do qual todas as equipes possam depender e contribuir. Ao fazê-lo, é importante garantir que este projeto principal não dependa, por sua vez, de nenhum dos projetos das equipes individuais; isso permite a implantação independente e, ao mesmo tempo, promove a reutilização do código.

Alguns exemplos de código que costumam ocorrer em um módulo principal incluem:

* Configurações do sistema inteiro, como:
   * Configurações do OSGi
   * Filtros Servlet
   * Mapeamentos do ResourceResolver
   * Gasodutos de Transformação Sling
   * Manipuladores de erros (ou use o Handler de página de erro comum ACS AEM ACS1)
   * Servlets de autorização para armazenamento em cache sensível a permissões
* Classes de utilitário
* Lógica principal da empresa
* Lógica de integração de terceiros
* Criação de sobreposições de interface
* Outras personalizações necessárias para a criação, como widgets personalizados
* Lançadores de fluxo de trabalho
* Elementos de design comuns usados em sites

*Arquitetura de projeto modular*

Isso não elimina a necessidade de várias equipes dependerem e atualizarem o mesmo conjunto de códigos. Ao criar um projeto principal, diminuímos o tamanho da base de códigos que é compartilhada entre equipes, diminuindo, mas não eliminando a necessidade de recursos compartilhados.

Para garantir que as alterações feitas neste pacote principal não interromperão a funcionalidade do sistema, recomendamos que um desenvolvedor sênior ou uma equipe de desenvolvedores mantenha a supervisão. Uma opção é ter uma única equipe que gerencie todas as alterações neste pacote; outra é fazer com que as equipes enviem solicitações que são revisadas e mescladas por esses recursos. É importante que um modelo de governança seja projetado e aceito pelas equipes e que os desenvolvedores o sigam.

## Gerenciamento do Escopo de Implantação {#managing-deployment-scope}

À medida que equipes diferentes implantam seus códigos no mesmo repositório, é importante que elas não substituam as alterações umas das outras. AEM tem um mecanismo para controlar isso ao implantar pacotes de conteúdo, o filtro. arquivo xml. É importante que não haja sobreposição entre o filtro.  arquivos xml; caso contrário, a implantação de uma equipe poderia apagar a implantação anterior de outra equipe. Para ilustrar este ponto, consulte os seguintes exemplos de arquivos de filtro bem-criados e problemáticos:

/apps/my-empresa vs. /apps/my-empresa/my-site

/etc/clientlibs/my-empresa vs. /etc/clientlibs/my-empresa/my-site

/etc/designs/my-empresa vs. /etc/designs/my-empresa/my-site

Se cada equipe configurar explicitamente seu arquivo de filtro para o(s) site(s) em que está trabalhando, cada equipe pode implantar seus componentes, bibliotecas de clientes e designs de site independentemente sem apagar as alterações de cada um.

Como é um caminho de sistema global e não específico de um site, o seguinte servlet deve ser incluído no projeto principal, já que as alterações feitas aqui podem afetar qualquer equipe:

/apps/sling/servlet/handler de erro

### Sobreposições {#overlays}

As sobreposições são usadas com frequência para estender ou substituir a funcionalidade AEM fora da caixa, mas o uso de uma sobreposição afeta todo o aplicativo AEM (ou seja, quaisquer alterações de funcionalidade estarão disponíveis para todos os inquilinos). Isso seria ainda mais complicado se os inquilinos tivessem requisitos diferentes para a sobreposição. Idealmente, os grupos empresariais deveriam trabalhar em conjunto para chegarem a acordo sobre a funcionalidade e aparência dos consoles administrativos do AEM.

Se não for possível chegar a um consenso entre as várias unidades de negócios, uma solução possível seria simplesmente não usar sobreposições. Em vez disso, crie uma cópia personalizada da funcionalidade e exponha-a por um caminho diferente para cada locatário. Isso permite que cada locatário tenha uma experiência de usuário completamente diferente, mas essa abordagem aumenta o custo da implementação e os esforços de atualização subsequentes também.

### Inicializadores do fluxo de trabalho {#workflow-launchers}

AEM usa inicializadores de fluxo de trabalho para acionar automaticamente a execução do fluxo de trabalho quando alterações especificadas são feitas no repositório. AEM fornece vários iniciadores prontos para uso, por exemplo, para executar processos de geração de execução e extração de metadados em ativos novos e atualizados. Embora seja possível deixar esses iniciadores como estão, em um ambiente multilocatário, se os locatários tiverem diferentes requisitos de iniciador e/ou modelo de fluxo de trabalho, é provável que os lançadores individuais precisem ser criados e mantidos para cada locatário. Esses iniciadores precisarão ser configurados para serem executados nas atualizações do locatário, deixando o conteúdo de outros locatários intocado. Isso pode ser feito facilmente aplicando iniciadores a caminhos de repositório específicos que sejam específicos do locatário.

### URLs personalizadas {#vanity-urls}

AEM oferece a funcionalidade vanity URL que pode ser definida por página. A preocupação com essa abordagem em um cenário de vários locatários é que AEM não garante a exclusividade entre os URLs personalizados configurados dessa maneira. Se dois usuários diferentes configurarem o mesmo caminho personalizado para páginas diferentes, um comportamento inesperado pode ser encontrado. Por esse motivo, recomendamos usar regras mod_rewrite nas instâncias do Apache dispatcher, que permitem um ponto central de configuração em conjunto com regras do Resolvedor de recursos somente de saída.

### Grupos de componentes {#component-groups}

Ao desenvolver componentes e modelos para vários grupos de criação, é importante utilizar de forma eficaz as propriedades componentGroup e allowPaths. Ao aproveitá-los com eficiência com designs de site, podemos garantir que os autores da Marca A só vejam os componentes e modelos criados para o site, enquanto os autores da Marca B só visualizam os seus.

### Testes {#testing}

Embora uma boa arquitetura e canais de comunicação abertos possam ajudar a impedir a introdução de defeitos em áreas inesperadas do site, essas abordagens não são inadequadas. Por essa razão, é importante testar completamente o que está sendo implantado na plataforma antes de liberar qualquer coisa para a produção. Isso requer a coordenação entre as equipes em seus ciclos de lançamento e reforça a necessidade de um conjunto de testes automatizados que cubram a maior quantidade de funcionalidade possível. Além disso, como um sistema será compartilhado por várias equipes, o desempenho, a segurança e o teste de carga se tornam mais importantes do que nunca.

## Considerações operacionais {#operational-considerations}

### Recursos compartilhados {#shared-resources}

AEM é executado em um único JVM; quaisquer aplicativos AEM implantados compartilham recursos uns com os outros, além dos recursos já consumidos na execução normal da AEM. Dentro do próprio espaço JVM, não haverá separação lógica de threads e os recursos finitos disponíveis para AEM, como memória, CPU e i/s de disco também serão compartilhados. Qualquer inquilino que consuma recursos afetará inevitavelmente outros inquilinos do sistema.

### Show {#performance}

Caso não siga AEM práticas recomendadas, é possível desenvolver aplicativos que consomem recursos além do que é considerado normal. Exemplos disso são o acionamento de muitas operações de fluxo de trabalho pesadas (como o Ativo de atualização de DAM), o uso de operações de push-on-MoSM em vários nós ou o uso de query JCR caros para renderizar conteúdo em tempo real. Elas terão inevitavelmente um impacto no desempenho de outros aplicativos de locatários.

### Logs {#logging}

AEM oferece interfaces prontas para uma configuração robusta de logger que pode ser usada para nossa vantagem em cenários de desenvolvimento compartilhados. Ao especificar os loggers separados para cada marca, por nome de pacote, podemos alcançar algum grau de separação de logs. Embora operações do sistema como replicação e autenticação ainda sejam registradas em um local central, o código personalizado não compartilhado pode ser registrado separadamente, facilitando os esforços de monitoramento e depuração para a equipe técnica de cada marca.

### Backup e restauração {#backup-and-restore}

Devido à natureza do repositório JCR, os backups tradicionais funcionam em todo o repositório, em vez de em um caminho de conteúdo individual. Por conseguinte, não é possível separar facilmente os backups por locatário. Por outro lado, a restauração a partir de um backup reverterá o conteúdo e os nós do repositório para todos os locatários do sistema. Embora seja possível realizar backups de conteúdo direcionado, usando ferramentas como VLT, ou para selecionar o conteúdo a ser restaurado criando um pacote em um ambiente separado, esses\
as abordagens não abrangem facilmente as configurações ou a lógica do aplicativo e podem ser complicadas de gerenciar.

## Considerações sobre segurança {#security-considerations}

### ACLs {#acls}

É possível, é claro, usar Listas de Controles de acesso (ACLs) para controlar quem tem acesso à visualização, criar e excluir conteúdo com base em caminhos de conteúdo, o que requer a criação e o gerenciamento de grupos de usuários. A dificuldade em manter as ACLs e grupos depende da ênfase em garantir que cada locatário tenha conhecimento zero de outros e se os aplicativos implantados dependem de recursos compartilhados. Para garantir uma ACL eficiente, administração de usuários e grupos, recomendamos ter um grupo centralizado com a supervisão necessária para garantir que esses controles de acesso e principais se sobreponham (ou não se sobreponham) de uma forma que promova a eficiência e a segurança.
