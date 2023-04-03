---
title: Configurar a pesquisa de tradução inteligente com o AEM Assets
description: A Pesquisa de tradução inteligente permite o uso de termos de pesquisa que não sejam em inglês para resolver para conteúdo em inglês. Para configurar AEM para a Pesquisa de tradução inteligente, o pacote OSGi da Tradução do Apache Oak Search Machine deve ser instalado e configurado, bem como os pacotes de idioma Apache Joshua de código livre e aberto pertinentes que contêm as regras de tradução.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 0%

---

# Configurar a pesquisa de tradução inteligente com o AEM Assets{#set-up-smart-translation-search-with-aem-assets}

A Pesquisa de tradução inteligente permite o uso de termos de pesquisa que não sejam em inglês para resolver para conteúdo em inglês. Para configurar AEM para a Pesquisa de tradução inteligente, o pacote OSGi da Tradução do Apache Oak Search Machine deve ser instalado e configurado, bem como os pacotes de idioma Apache Joshua de código livre e aberto pertinentes que contêm as regras de tradução.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>A Pesquisa de tradução inteligente deve ser configurada em cada instância de AEM que a exija.

1. Baixe e instale o pacote OSGi de Tradução do Oak Search Machine
   * [Baixe o pacote OSGi da Tradução do Oak Search Machine](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) que corresponde AEM versão Oak.
   * Instale o pacote Oak Search Machine Translation OSGi baixado em AEM via [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Baixe e atualize os pacotes de idioma do Apache Joshua
   * Baixe e descompacte o arquivo desejado [Pacotes de idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * Edite o `joshua.config` e comente as 2 linhas que começam com:

      ```
      feature-function = LanguageModel ...
      ```

   * Determine e registre o tamanho da pasta modelo do pacote de idiomas, pois isso influencia o espaço extra de heap que AEM precisará.
   * Mova a pasta do pacote de idiomas Apache Joshua descompactado (com o `joshua.config` edits) a

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Por exemplo:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Reinicie o AEM com a alocação de memória heap atualizada
   * Parar AEM
   * Determine o novo tamanho de heap necessário para AEM

      * AEM tamanho de heap sem idioma + o tamanho do diretório do modelo arredondado para o número de GB mais próximo
      * Por exemplo: Se os pacotes de pré-idioma da instalação do AEM precisarem de 8 GB de heap para serem executados e a pasta de modelo do pacote de idiomas estiver 3,8 GB descompactada, o novo tamanho do heap será:

         O original `8GB` + ( `3.75GB` arredondado para cima e para o mais próximo `2GB`, que `4GB`) para um total de `12GB`
   * Verifique se a máquina tem essa quantidade de memória disponível extra.
   * Atualize AEM scripts de inicialização para ajustá-los para o novo tamanho de heap

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
   >O **o heap base deve ser sempre calculado para oferecer suporte ao desempenho aceitável sem qualquer pacote de idiomas** instalado.

4. Registre os pacotes de idiomas por meio das configurações OSGi do Provedor de Termos de Consulta de Texto Completo do Apache Jackrabbit Oak Machine Translation

   * Para cada pacote de idiomas, [crie uma nova configuração OSGi do Provedor de Termos de Consulta de Texto Completo de Tradução Automática no Apache Jackrabbit Oak](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) pelo gerenciador de Configuração do Console da Web AEM.

      * `Joshua Config Path` é o caminho absoluto para o arquivo joshua.config. O processo de AEM deve ser capaz de ler todos os arquivos na pasta do pacote de idiomas.
      * `Node types` são os tipos de nó candidatos cuja pesquisa de texto completo irá envolver este pacote de idiomas para tradução.
      * `Minimum score` é a pontuação de confiança mínima de um termo traduzido para que seja usado.

         * Por exemplo, hombre (Espanhol para &quot;homem&quot;) pode traduzir para a palavra inglesa &quot;homem&quot; com uma pontuação de confiança de `0.9` e também traduzir para a palavra inglesa &quot;human&quot; com uma pontuação de confiança `0.2`. Ajustar a pontuação mínima para `0.3`, manteria a tradução &quot;hombre&quot; para &quot;man&quot;, mas rejeitaria a tradução &quot;hombre&quot; para &quot;humana&quot;, pois esta tradução `0.2` é menor que a pontuação mínima de `0.3`.

5. Executar uma pesquisa de texto completo em ativos
   * Como dam:Asset é o tipo de nó que este pacote de idiomas está registrado novamente, devemos procurar o AEM Assets usando a pesquisa de texto completo para validar isso.
   * Navegue até AEM > Ativos e abra o Omnisearch. Procure um termo no idioma cujo pacote de idiomas foi instalado.
   * Conforme necessário, ajuste a Pontuação mínima nas configurações de OSGi para garantir a precisão dos resultados.

6. Atualização de pacotes de idiomas
   * Os pacotes de idiomas do Apache Joshua são totalmente mantidos pelo projeto Apache Joshua e sua atualização ou correção é o critério do projeto Apache Joshua.
   * Se um pacote de idiomas for atualizado, para instalar as atualizações no AEM, siga as etapas 2 - 4 acima, ajustando o tamanho do heap para cima ou para baixo conforme necessário.

      * Observe que ao mover o pacote de idiomas descompactado para a pasta crx-quickstart/opt , mova qualquer pasta de pacote de idiomas existente antes de copiar na nova pasta.
   * Se AEM não exigir uma reinicialização, a configuração relevante do provedor OSGi de termos de consulta de texto completo do Apache Jackrabbit Oak que pertence aos pacotes de idiomas atualizados deverá ser salva novamente para que AEM processe os arquivos atualizados.


## Atualização do índice damAssetLucene {#updating-damassetlucene-index}

Para [Tags inteligentes do AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) ser afetados pela AEM Smart Translation, AEM `/oak   :index  /damAssetLucene` O índice deve ser atualizado para marcar o predictedTags (o nome do sistema para &quot;Tags inteligentes&quot;) para fazer parte do índice Lucene agregado do ativo.

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

* [Pacote OSGi de tradução do Apache Oak Search Machine](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Pacotes de idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Tags inteligentes do AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Práticas recomendadas para consulta e indexação](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
