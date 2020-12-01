---
title: Registrando tipos de ativos personalizados
seo-title: Registrando tipos de ativos personalizados
description: Ativar tipos de ativos personalizados para listagem no AEMForms Portal
seo-description: Ativar tipos de ativos personalizados para listagem no AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '666'
ht-degree: 2%

---


# Registrando tipos de ativos personalizados {#registering-custom-asset-types}

Ativar tipos de ativos personalizados para listagem no AEMForms Portal

>[!NOTE]
>
>Verifique se você tem AEM 6.3 com SP1 e o AEM Forms Add On instalado correspondente. Este recurso só funciona com o AEM Forms 6.3 SP1 e superior

## Especificar caminho básico {#specify-base-path}

O caminho base é o caminho de repositório de nível superior que compreende todos os ativos que um usuário pode querer lista no componente de pesquisa e lister. Se desejado, o usuário também pode configurar locais específicos dentro do caminho base da caixa de diálogo de edição de componentes, para que a pesquisa seja acionada em locais específicos, em vez de pesquisar todos os nós dentro do caminho base. Por padrão, o caminho base é usado como critério de caminho de pesquisa para buscar os ativos, a menos que o usuário configure um conjunto de caminhos específicos a partir desse local. É importante ter um valor ótimo desse caminho para fazer uma pesquisa de desempenho. O valor padrão do caminho base permanecerá como **_/content/dam/formsanddocuments_** porque todos os ativos AEM Forms residem em **_/content/dam/formsanddocuments._**

Etapas para configurar o caminho básico

1. Logon no crx
1. Navegue até **/libs/fd/fp/extensions/querybuilder/basepath**

1. Clique em &quot;Nó de sobreposição&quot; na barra de ferramentas
1. Verifique se o local da sobreposição é &quot;/apps/&quot;
1. Clique em Ok
1. Clique em Salvar
1. Navegue até a nova estrutura criada em **/apps/fd/fp/extensions/querybuilder/basepath**

1. Altere o valor da propriedade path para **&quot;/content/dam&quot;**
1. Clique em Salvar

Ao especificar a propriedade path para **&quot;/content/dam&quot;**, você está basicamente definindo o Caminho básico como /content/dam. Isso pode ser verificado abrindo o componente de Pesquisa e Lister.

![base](assets/basepath.png)

## Registrar tipos de ativos personalizados {#register-custom-asset-types}

Adicionamos uma nova guia (Listagem de ativos) no componente de pesquisa e lister. Esta guia será lista fora da caixa de tipos de ativos e tipos de ativos adicionais configurados. Por padrão, os seguintes tipos de ativos são listados

1. Formulários adaptáveis
1. Modelos de formulário
1. PDF forms
1. Documento(PDFs estáticos)

**Etapas para registrar o tipo de ativo personalizado**

1. Criar nó de sobreposição de **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Defina o local da sobreposição como &quot;/apps&quot;
1. Navegue até a nova estrutura criada em **/apps/fd/fp/extensions/querybuilder/assettypes **

1. Nesse local, crie um nó &#39;nt:unstructed&#39; para o tipo a ser registrado, nomeie o nó **mp4files. Adicione as duas propriedades a seguir a este nó mp4files**

   1. Adicione a propriedade jcr:title para especificar o nome de exibição do tipo de ativo. Defina o valor de jcr:title como &quot;Arquivos Mp4&quot;.
   1. Adicione a propriedade &quot;type&quot; e defina seu valor como &quot;videos&quot;. Esse é o valor que usamos em nosso modelo para lista de ativos do tipo vídeos. Salve as alterações.

1. Crie um nó do tipo &quot;nt:unstructed&quot; em mp4files. Nomeie este nó como &quot;critérios de pesquisa&quot;
1. Adicione um ou mais filtros sob critérios de pesquisa. Suponha que, se o usuário quiser ter um filtro de pesquisa para lista mp4Files cujo tipo mime seja &quot;video/mp4&quot;, você pode fazer isso aqui
1. Crie um nó do tipo &quot;nt:unstructed&quot; nos critérios de pesquisa do nó. Nomeie este nó como &quot;filetypes&quot;
1. Adicione as duas propriedades a seguir a este nó &quot;filetypes&quot;

   1. name: ./jcr:content/metadata/dc:format
   1. valor: video/mp4

1. Isso significa que ativos com a propriedade dc:format igual a video/mp4 serão considerados um tipo de ativo &quot;Vídeos Mp4&quot;. Você pode usar qualquer propriedade listada no nó &quot;jcr:content/metadata&quot; para os critérios de pesquisa

1. **Certifique-se de salvar seu trabalho**

Após executar as etapas acima, o novo tipo de ativo (Arquivos Mp4) será exibido na lista suspensa de tipos de ativos do componente de Pesquisa e Lister, como mostrado abaixo

![mp4files](assets/mp4files.png)

[Se tiver problemas para fazer isso funcionar, você pode importar o seguinte pacote.](assets/assettypeskt1.zip) O pacote tem dois tipos de ativos personalizados definidos. Arquivos Mp4 e documentos do Word. Sugestão: analise os **/apps/fd/fp/extensions/querybuilder/assettypes**

[Instale o pacote](assets/customportalpage.zip) customeportal. Este pacote contém uma página de portal de exemplo. Esta página será usada na parte2 deste tutorial

