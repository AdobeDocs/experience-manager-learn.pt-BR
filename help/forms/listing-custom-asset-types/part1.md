---
title: Registro de tipos de ativos personalizados
description: Habilitação de tipos de ativos personalizados para listagem no Portal do AEM Forms
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '644'
ht-degree: 0%

---

# Registro de tipos de ativos personalizados {#registering-custom-asset-types}

Habilitação de tipos de ativos personalizados para listagem no Portal do AEM Forms

>[!NOTE]
>
>Verifique se o AEM 6.3 com SP1 e o AEM Forms Add On correspondente estão instalados. Esse recurso só funciona com o AEM Forms 6.3 SP1 e superior

## Especificar caminho base {#specify-base-path}

O caminho base é o caminho do repositório de nível superior que compreende todos os ativos que um usuário pode querer listar no componente de pesquisa e lista. Se desejar, o usuário também poderá configurar locais específicos no caminho base da caixa de diálogo de edição de componentes, para que a pesquisa seja acionada em locais específicos, em vez de pesquisar todos os nós no caminho base. Por padrão, o caminho base é usado como critério de caminho de pesquisa para buscar os ativos, a menos que o usuário configure um conjunto de caminhos específicos dentro desse local. É importante ter um valor ideal desse caminho para fazer uma pesquisa eficiente. O valor padrão do caminho base permanecerá como **_/content/dam/formsanddocuments_** porque todos os ativos do AEM Forms residem em **_/content/dam/formsanddocuments._**

Etapas para configurar o caminho base

1. Logon no crx
1. Navegue até **/libs/fd/fp/extensions/querybuilder/basepath**

1. Clique em &quot;Sobrepor nó&quot; na barra de ferramentas
1. Verifique se o local da sobreposição é &quot;/apps/&quot;
1. Clique em Ok
1. Clique em Salvar
1. Navegue até a nova estrutura criada em **/apps/fd/fp/extensions/querybuilder/basepath**

1. Altere o valor da propriedade de caminho para **&quot;/content/dam&quot;**
1. Clique em Salvar

Ao especificar a propriedade do caminho para **&quot;/content/dam&quot;**, você basicamente está definindo o Caminho Base como /content/dam. Isso pode ser verificado abrindo o componente de Pesquisa e Lister.

![basepath](assets/basepath.png)

## Registrar tipos de ativos personalizados {#register-custom-asset-types}

Adicionamos uma nova guia (Listagem de ativos) no componente de pesquisa e lista. Essa guia listará tipos de ativos prontos para uso e tipos de ativos adicionais que você configurar. Por padrão, os seguintes tipos de ativos são listados

1. Adaptive Forms
1. Modelos de formulário
1. PDF forms
1. Documento(PDF estáticos)

**Etapas para registrar o tipo de ativo personalizado**

1. Criar nó de sobreposição de **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Defina o local da sobreposição como &quot;/apps&quot;
1. Navegue até a nova estrutura criada em `/apps/fd/fp/extensions/querybuilder/assettypes`

1. Neste local, crie um nó &quot;nt:unstructured&quot; para o tipo a ser registrado, nomeie o nó **mp4files. Adicione as duas propriedades a seguir a este nó de arquivos mp4**

   1. Adicione a propriedade jcr:title para especificar o nome de exibição do tipo de ativo. Defina o valor de jcr:title como &quot;Arquivos Mp4&quot;.
   1. Adicione a propriedade &quot;type&quot; e defina o valor como &quot;videos&quot;. Esse é o valor que usamos em nosso modelo para listar ativos dos tipos de vídeos. Salve as alterações.

1. Crie um nó do tipo &quot;nt:unstructured&quot; em mp4files. Nomeie este nó como &quot;searchcriteria&quot;
1. Adicione um ou mais filtros nos critérios de pesquisa. Suponha que, se o usuário quiser ter um filtro de pesquisa para listar mp4Files cujo tipo mime seja &quot;video/mp4&quot;, você pode fazer isso aqui
1. Crie um nó do tipo &quot;nt:unstructured&quot; nos critérios de pesquisa do nó. Nomeie este nó como &quot;filetypes&quot;
1. Adicione as 2 propriedades a seguir a esse nó &quot;filetypes&quot;

   1. nome: ./jcr:content/metadata/dc:format
   1. value: video/mp4

1. Isso significa que os ativos com a propriedade dc:format igual a video/mp4 são considerados um tipo de ativo &quot;Vídeos Mp4&quot;. Você pode usar qualquer propriedade listada no nó &quot;jcr:content/metadata&quot; para os critérios de pesquisa

1. **Salve seu trabalho**

Depois de executar as etapas acima, o novo tipo de ativo (Arquivos Mp4) começará a ser exibido na lista suspensa de tipos de ativo do componente de Pesquisa e Lister, como mostrado abaixo

![arquivos mp4](assets/mp4files.png)

[Se você tiver problemas para fazer com que isso funcione, poderá importar o pacote a seguir.](assets/assettypeskt1.zip) O pacote tem dois tipos de ativos personalizados definidos. Arquivos Mp4 e documentos do Word. Sugestão: dê uma olhada em **/apps/fd/fp/extensions/querybuilder/assettypes**

[Instalar o pacote customomeportal](assets/customportalpage.zip). Este pacote contém um exemplo de página do portal. Esta página é usada na parte 2 deste tutorial
