---
title: Configurar a pesquisa de tradução inteligente com o AEM Assets
description: A Pesquisa de tradução inteligente permite o uso de termos de pesquisa em outros idiomas para resolver para conteúdo em inglês. Para configurar o AEM para a Pesquisa de tradução inteligente, o pacote OSGi da Tradução automática de pesquisa do Apache Oak deve ser instalado e configurado, bem como os pacotes de idiomas pertinentes do Apache Joshua, gratuitos e de código aberto, que contêm as regras de tradução.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 0%

---

# Configurar a pesquisa de tradução inteligente com o AEM Assets{#set-up-smart-translation-search-with-aem-assets}

A Pesquisa de tradução inteligente permite o uso de termos de pesquisa em outros idiomas para resolver para conteúdo em inglês. Para configurar o AEM para a Pesquisa de tradução inteligente, o pacote OSGi da Tradução automática de pesquisa do Apache Oak deve ser instalado e configurado, bem como os pacotes de idiomas pertinentes do Apache Joshua, gratuitos e de código aberto, que contêm as regras de tradução.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>A Pesquisa de tradução inteligente deve ser configurada em cada instância do AEM que a exija.

1. Baixe e instale o pacote OSGi de tradução automática do Oak Search
   * [Baixe o pacote OSGi de tradução automática do Oak Search](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) que corresponde à versão AEM Oak.
   * Instale o pacote OSGi baixado da tradução automática do Oak Search no AEM via [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Baixar e atualizar os pacotes de idiomas do Apache Joshua
   * Baixe e descompacte o arquivo desejado [Pacotes de idioma do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Edite o `joshua.config` arquive e comente as 2 linhas que começam com:

     ```
     feature-function = LanguageModel ...
     ```

   * Determine e registre o tamanho da pasta de modelo do pacote de idioma, pois isso influencia a quantidade de espaço extra de heap que o AEM exigirá.
   * Mova a pasta descompactada do pacote de idiomas Apache Joshua (com o `joshua.config` edições) a

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     Por exemplo:

     ```
      .../crx-quickstart/opt/es-en
     ```

3. Reiniciar o AEM com alocação de memória heap atualizada
   * Parar AEM
   * Determine o novo tamanho de heap necessário para o AEM

      * Tamanho do heap de pré-idioma do AEM + o tamanho do diretório modelo arredondado para os 2 GB mais próximos
      * Por exemplo: se os pacotes de pré-idioma para a instalação do AEM exigirem 8 GB de heap para serem executados e a pasta de modelo do pacote de idioma for 3,8 GB descompactada, o novo tamanho do heap será:

        O original `8GB` + ( `3.75GB` arredondado para cima até o mais próximo `2GB`, que é `4GB`) para um total de `12GB`

   * Verifique se o computador tem essa quantidade de memória extra disponível.
   * Atualize os scripts de inicialização do AEM para ajustar o novo tamanho da pilha

      * Exemplo. `java -Xmx12g -jar cq-author-p4502.jar`

   * Reinicie o AEM com o tamanho de heap aumentado.

   >[!NOTE]
   >
   >O espaço de heap necessário para pacotes de idiomas pode aumentar, especialmente quando vários pacotes de idiomas são usados.
   >
   >
   >Sempre verifique se **a instância tem memória suficiente** para acomodar os aumentos no espaço de heap alocado.
   >
   >
   >A variável **o heap base sempre deve ser calculado para suportar o desempenho aceitável sem nenhum pacote de idioma** instalado.

4. Registrar os pacotes de idiomas por meio das configurações OSGi do Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider

   * Para cada pacote de idioma, [criar uma nova configuração OSGi do provedor de termos de consulta de texto completo da tradução automática do Apache Jackrabbit Oak](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) através do gerenciador de configuração do console da Web AEM.

      * `Joshua Config Path` é o caminho absoluto para o arquivo joshua.config. O processo AEM deve ser capaz de ler todos os arquivos na pasta do pacote de idioma.
      * `Node types` são os tipos de nó candidato cuja pesquisa de texto completo envolverá este pacote de idiomas para tradução.
      * `Minimum score` é a pontuação de confiança mínima para um termo traduzido para ser usado.

         * Por exemplo, hombre (espanhol para &quot;homem&quot;) pode traduzir para a palavra em inglês &quot;man&quot; com uma pontuação de confiança de `0.9` e também traduzir para a palavra em inglês &quot;human&quot; com uma pontuação de confiança `0.2`. Ajuste da pontuação mínima para `0.3`, manteria a tradução de &quot;hombre&quot; para &quot;man&quot;, mas descartaria a tradução de &quot;hombre&quot; para &quot;human&quot;, pois essa pontuação de tradução de `0.2` é menor que a pontuação mínima de `0.3`.

5. Executar uma pesquisa de texto completo em relação a ativos
   * Como dam:Asset é o tipo de nó no qual esse pacote de idioma está registrado novamente, devemos pesquisar o AEM Assets usando a pesquisa de texto completo para validar isso.
   * Navegue até AEM > Assets e abra o Omnisearch. Procure por um termo no idioma cujo pacote de idioma foi instalado.
   * Conforme necessário, ajuste a Pontuação mínima nas configurações do OSGi para garantir a precisão dos resultados.

6. Atualizando pacotes de idiomas
   * Pacotes de linguagem Apache Joshua são totalmente mantidos pelo projeto Apache Joshua, e sua atualização ou correção é como a discrição do projeto Apache Joshua.
   * Se um pacote de idiomas for atualizado, para instalar as atualizações no AEM, siga as etapas 2 a 4 acima, ajustando o tamanho da pilha para cima ou para baixo, conforme necessário.

      * Observe que ao mover o pacote de idioma descompactado para a pasta crx-quickstart/opt, mova qualquer pasta existente de pacote de idioma antes de copiar na nova pasta.

   * Se o AEM não exigir uma reinicialização, as configurações relevantes de OSGi do provedor de termos de consulta de texto completo da tradução de máquina Apache Jackrabbit Oak que pertencem aos pacotes de idiomas atualizados devem ser salvas novamente para que o AEM processe os arquivos atualizados.

## Atualização do índice damAssetLucene {#updating-damassetlucene-index}

A fim de [Tags inteligentes do AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) ser afetado pela tradução inteligente do AEM, AEM `/oak   :index  /damAssetLucene` O índice deve ser atualizado para marcar as predictedTags (o nome do sistema para &quot;Tags inteligentes&quot;) como parte do índice Lucene agregado do ativo.

Em `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, verifique se a configuração é a seguinte:

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## Recursos adicionais{#additional-resources}

* [Pacote OSGi de tradução automática da pesquisa do Apache Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Pacotes de idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Tags inteligentes do AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Práticas recomendadas para consulta e indexação](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
