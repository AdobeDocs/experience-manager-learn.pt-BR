---
title: Hierarquia do site, taxonomia e dicas de marcação
seo-title: Overview of Authoring in AEM Sites
description: O vídeo a seguir fornece uma visão geral dos conceitos básicos de criação em um ambiente de criação de AEM. Usa o console Sites como base.
seo-description: The following video provides an overview of basic concepts of authoring in an AEM author environment. It uses the Sites console as a basis.
feature: Page Editor, Editable Templates
topics: authoring, publishing
audience: author, marketer
doc-type: feature video
activity: use
version: 6.4, 6.5, Cloud Service
kt: 4242
thumbnail: 33594.jpg
topic: Content Management
role: User
level: Beginner
hide: true
hidefromtoc: true
source-git-commit: 3335069883db38a2748af114ab7505cc34dec270
workflow-type: tm+mt
source-wordcount: '2033'
ht-degree: 0%

---


# Tags, taxonomia e práticas recomendadas de metadados: Resumo de alto nível

Metadados e tags são fundamentais para aumentar a eficiência do AEM. Usuários, líderes e gerentes percebem a necessidade de uma estratégia holística, mas acham difícil avançar. Muitas vezes o conhecimento é dividido entre os usuários, dificultando a estratégia holística — e torna os ajustes ainda mais problemáticos.

Qual é a diferença entre metadados e tags? Quais são os aspectos comerciais que devem ser considerados ao orientar sua estratégia?

## Qual é a finalidade dos metadados?

Metadados adicionam estrutura a conteúdo menos estruturado.
Exemplo: Uma imagem básica tem pixels. Podemos chamar esses &quot;dados principais&quot;. São os metadados que descrevem o formato, a categoria, os detalhes de licenciamento, etc.
Os metadados são usados com mais frequência para ativos. Mas há um grande número de casos de uso para metadados em páginas de conteúdo ou fragmentos de experiência também.

## Fontes de metadados

Estas são as categorias das quais os metadados podem ser gerados:

* Metadados extraídos - As informações já estão disponíveis no documento, por exemplo, em linguagem natural.
* Metadados derivados - As informações não estão disponíveis nos dados originais, mas podem ser derivadas por referência cruzada a conhecimento anterior.
* Metadados adicionados manualmente - São metadados que não se enquadram em nenhuma das primeiras categorias e precisam ser adicionados manualmente por um humano.

## Tipos de metadados

Dentro das categorias listadas acima, há quatro tipos principais:

* Metadados técnicos e descritivos: Fornece informações sobre detalhes técnicos do conteúdo (ou seja, título, idioma etc.)
* Metadados operacionais: Documenta o ciclo de vida de um ativo (ou seja, aprovado, no creative, campaign)
* Metadados administrativos: O status ou o estado de um ativo em uma organização (ou seja, informações sobre licença, propriedade)
* Metadados estruturais: Ajuda a categorizar ativos ou páginas para suavizar o processo de negócios (aplica-se à maioria das tags e taxonomias)

## Nomes de pastas e arquivos

As pastas são uma maneira natural de navegar e navegar pelo conteúdo no AEM. Como é que os seus participantes vão interagir com AEM? Isso determinará como suas pastas serão estruturadas. Normalmente, a estrutura de pastas foi arquitetada com uma (ou duas) das seguintes ideias:

* Navegação
* Navegação
* Categorização
* Controle de acesso

Para o AEM Sites, a navegação é a chave. As pastas são usadas para controlar o acesso aos ativos e páginas.

Quais níveis de autores precisarão acessar as páginas iniciais? E quanto às páginas do produto? Ou campanha? Use as permissões mais a estrutura de pastas para colocar no controle correto.

## Armazenamento de metadados

Há três maneiras de armazenar metadados:

* Binário: Formato binário relacionado à natureza do ativo (Photoshop, InDesign, PNG, JPG).
* Nó de ativo: Esses são metadados no próprio ativo, independentemente do sistema ou processo que está sendo usado.
* Localização externa: Metadados que não estão diretamente no ativo, mas podem ser usados como um descritor do &quot;estado&quot; de um ativo (por exemplo: um fluxo de trabalho que pode afetar um ativo, mas não é aplicado diretamente a ele)

## Modelo de metadados

A estrutura de como os metadados são capturados e formatados é chamada de modelo de metadados ou esquema de metadados. Isso deve ser acordado antes da assimilação de ativos ou páginas no sistema.

Um modelo de metadados geralmente é arquitetado para atender aos seguintes casos de uso:

* Pesquisar e recuperar: Ajuda a armazenar os principais aspectos do conteúdo, o que ajuda a fácil recuperação por parte dos negócios.
* Reutilizar: Ajuda a utilizar ativos antigos para fins de redefinição (economizando tempo e dinheiro)
* Gerenciamento de licenças: Rastreia a propriedade do ativo pela organização (geralmente por motivos legais)
* Distribuir: Disponibilize conteúdo aos consumidores ou associe ativos a parceiros comerciais.
* Arquivar: Metadados que indicam que um ativo está desatualizado (sempre a prática recomendada é colocar um sinalizador &quot;arquivado&quot; no ativo para não perder informações vitais)/
* Referência cruzada: Metadados associados que capturam a relação de dois ou mais ativos entre si (a síntese de metadados permite a referência cruzada e a organização de grupos coerente)
* Navegar: A estrutura de pastas na qual os ativos são armazenados (usada para recuperar informações ao navegar)

*Os metadados do autor são compatíveis principalmente com processos operacionais. A publicação suporta casos de uso de recuperação e distribuição.*

## Uso de tags como termos predefinidos

Uma tag é uma palavra-chave ou termo atribuído a uma informação.vPor exemplo, em vez de inserir &quot;car&quot;, &quot;veículo&quot;, &quot;automóvel&quot;, um sistema de tags permite escolher apenas um valor, tornando a pesquisa mais previsível.  As tags normalizam e simplificam a categorização de ativos.

*Observação: Embora a AEM permita a marcação ad hoc, é uma prática recomendada manter-se disto, pois poderia levar a uma taxonomia indefinida e incontrolável.*

Uso comum de tags:

* Pesquisa de palavra-chave: Uma tag pode descrever que um recurso pertence a um determinado grupo de entidades. Por exemplo, uma tag &quot;image/subject/car&quot; descreve o recurso que pertence ao conjunto de imagens que mostram um carro.
* Relações de condução: Todos os recursos que compartilham a mesma tag podem ser considerados conectados. A marcação em vez de vinculação direta é especialmente útil em sites que têm muito conteúdo dinâmico e conectado.
* Navegação da unidade: As tags pedidas na taxonomia hierárquica podem criar navegação, um link ou para documentos semelhantes.
As tags também devem ser pesquisadas com informações que conectam vários tipos de dados, com base em termos comerciais, e não em propriedades técnicas.

## Aplicativos comuns de tags

Quando usadas em AEM, as tags podem ajudar a alcançar uma implementação muito mais curta do recurso complexo, como:

* Pesquisa de facetas
* Navegação personalizada
* Conteúdo Relacionado
* Referências do conteúdo
* Otimização do mecanismo de pesquisa
* Destacar os principais conceitos

## Taxonomias

Uma Taxonomia é um sistema de organização de tags com base em características compartilhadas, que geralmente são estruturadas hierarquicamente de acordo com a necessidade organizacional. A estrutura pode ajudar a encontrar uma tag mais rapidamente ou impor uma generalização.
Exemplo: É necessário subcategorizar as imagens de existências dos automóveis.  A taxonomia pode se parecer com:

/subject/car/ /subject/car/sportscar /subject/car/sportscar/porsche /subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen ... /subject/car/limousine ...

Agora um usuário pode escolher se quer procurar imagens de carros esportivos em geral ou um &quot;Porsche&quot; em particular. Afinal, ambos são carros de esporte.
Prática recomendada: Evite taxonomias planas. As taxonomias planas não beneficiam dos benefícios acima descritos e exigem uma manutenção constante

**Usar uma taxonomia como dicionário de sinônimos.**  Quando um usuário pesquisa por uma palavra-chave, o sistema cria uma segunda pesquisa para todos os sinônimos encontrados lá.
Além disso, em vez de digitar &quot;carro&quot; manualmente, o sistema pode fornecer uma lista de palavras-chave para melhorar a consistência.

**Usar uma taxonomia como um dicionário.** Em vez de apenas imprimir &quot;carro&quot;, você pode expandir a única tag e usar todos os sinônimos da tag.

**Várias Categorias.** Em contraste com uma hierarquia de pasta, as tags podem ser usadas para expressar várias categorizações ao mesmo tempo. Um ativo marcado com:

/subject/car/minivan/mercedes /subject/people/family /color/red

## Metadados vs tag

Nem todos os metadados devem ser considerados como candidatos a sistema de marcação. Os metadados técnicos podem, desnecessariamente, duplicar as informações. O melhor candidato para tags são os metadados comerciais. .Tags são uma boa opção para impor vocabulário consistente, pesquisa com facetas e navegação.

## Gerenciamento de tags

Benefícios do Tag Management de uma equipe principal dedicada. Os novos membros devem aprender primeiro o propósito e a função da taxonomia antes de adicionar novas tags.  Os especialistas experientes, atuando como porteiros para novas tags, reduzirão a inconsistência a longo prazo.

## Criação de tags

As taxonomias devem ser empregadas por autores de conteúdo e compreendidas pelos usuários finais. Eles devem ser criados antes do processo de criação de conteúdo. Quaisquer atalhos resultarão em esforço adicional para gerenciamento e manutenção.

## Manutenção Contínua

As coisas mudam, e as necessidades da lista de tags também mudarão. Crie um processo de manutenção sólido que reduzirá a duplicação.

Certifique-se de que os colaboradores de conteúdo saibam como propor alterações e que os editores ou gerentes de conteúdo estejam revisando os termos regularmente.

## Práticas recomendadas com tags e taxonomias

**Padronizar tags.** Crie um glossário que forneça um vocabulário autoritativo. Sem a definição de normas, a duplicação apresentará problemas. Além disso, é recomendável auditar não apenas a taxonomia, mas também o uso das tags.

**Não fique com tags extras.** As tags podem perder o significado se forem distribuídas com frequência.Remova tags irrelevantes para obter eficiência ideal.

**Reavalie as tags ao longo do tempo.** Lembre-se de que a terminologia comercial e o contexto comercial raramente permanecem estáticos. Talvez seja necessário padronizar e reaplicar tags.

**Uso de tags inteligentes alimentadas por IA.** Marcação inteligente [ver link] é um recurso de IA no AEM para reduzir o esforço de marcar ativos manualmente. A marcação inteligente usa uma IA para descobrir informações sobre o assunto de uma imagem. Ele gera tags descritivas que descrevem o conteúdo de uma imagem.

## Qualidade e manutenção dos metadados

Entender os requisitos comerciais é uma etapa importante na execução de um modelo de gerenciamento de metadados. Sem definição, as informações não podem ser armazenadas. Será necessário rever periodicamente o modelo. Trata-se de uma atividade vital de controlo de qualidade.

Além disso, os metadados devem ser capturados o mais rápido possível no processo de criação de conteúdo. Se os metadados não forem &quot;aplicados na hora certa, há poucas chances de aplicá-los retroativamente.

**Utilizar metadados** para melhorar a colaboração: Utilize o Adobe Asset Link, o Adobe Bridge e o AEM Desktop para unir o processo criativo e utilizar metadados para simplificar os fluxos de trabalho criativos. Usar essas ferramentas enriquecerá os metadados e a experiência do usuário em todo o seu processo criativo.

## Práticas recomendadas para o gerenciamento de metadados

* Atribuir Equipe Principal com Forte Mandato Executivo: Forneça uma equipe principal de metadados que tenha total compreensão do ecossistema de negócios e um forte mandato do gerenciamento da organização.
* Defina a estratégia e a governança de metadados: Uma boa estratégia de metadados pode ajudar as organizações a explicar a necessidade e os benefícios dos metadados.  Uma estratégia consiste em, esquema(s) de metadados, taxonomia, processos de negócios (para qualidade e captura de dados), funções e responsabilidades e processos de governança. *
* Definir e comunicar o modelo de metadados consistente: A estratégia e o raciocínio definidos devem ser bem documentados e comunicados no seio das organizações.
* Convenção de nomenclatura padrão: Crie uma convenção de nomenclatura de arquivo consistente e descritiva para melhorar a identidade visual, o gerenciamento de informações e a usabilidade.
* Caracteres de segurança em nomes de arquivo: O nome de arquivo deve ser interpretado por todos os sistemas operacionais comuns. É seguro usar caracteres, números, tremas, espaço e sublinhado. O símbolo de menos também é seguro, mas se você cortar e colar, ele pode parecer um &quot;traço&quot;.
* Convenção de nomenclatura da versão: O AEM oferece alguns recursos para manter versões anteriores dos ativos. Em alguns casos, convém manter várias versões. No entanto, você deve se certificar de que o esquema de versão é consistente.

## Metadados organizacionais vs. descritivos

Algumas diretrizes podem ajudá-lo a decidir como categorizar metadados:

**Descrição** - Se os dados descreverem o ativo ou o conteúdo, eles deverão fazer parte dos metadados anexados.

**Pesquisar** - Se os metadados forem utilizados na pesquisa, devem ser anexados.

**Exposição** - Se estiver expondo os metadados em uma plataforma de distribuição a terceiros, não exponha também os metadados &quot;internos&quot;.

**Duração** - Quanto mais tempo os metadados forem supostamente utilizados, mais provável será que sejam bons candidatos a metadados anexados.

**Processos de negócios relacionados** - É definitivamente útil ter uma ID de produto permanente como parte dos metadados. Mas a categoria de um item em relação ao catálogo de produtos é um metadado questionável para o ativo.

**Organização e processamento** - Se a natureza dos metadados for de natureza organizacional, como um estado em um fluxo de trabalho de aprovação ou a propriedade de um determinado departamento, os metadados externos devem ser considerados ao anexar os metadados ao ativo.

*Para criar a estratégia, faça as seguintes perguntas:*

* Que tipo de conteúdo e &quot;informações adicionais&quot; (= metadados) são necessárias para resolver problemas de negócios / perguntas comerciais / problemas de negócios?
* Quais são as variáveis, os &quot;campos&quot; no esquema e quais são os valores possíveis? Quais variáveis precisam de uma entrada de texto livre, quais podem ser restritas por tipo (número, data, booleano, ...), um conjunto de valores fixos (por exemplo, países) ou tags de uma determinada taxonomia. Quantas tags são necessárias, são permitidas?
* Quais problemas/problemas/perguntas técnicos podem ser resolvidos por metadados?
* Como você pode obter / criar esse conteúdo / metadados? Quanto custará obter/criar esses metadados?
* Quais tipos de metadados são necessários para um grupo de usuários específico?
* Como os metadados serão mantidos e atualizados?
* Quem é responsável por que parte do processo?
* Como você pode garantir que os processos de negócios acordados sejam seguidos?
* Que normas deve seguir? Se você adotar e modificar um padrão do setor (Dublin Core, ISO 1915, PRISM, etc.) ou deverá a organização criar as suas próprias normas?
* Onde está documentada a estratégia? Como você pode garantir que todas as partes interessadas tenham acesso? Como você pode se certificar de que o pessoal recém-integrado adere ao padrão acordado (por exemplo, treinamento de visita antes de obter acesso?)
