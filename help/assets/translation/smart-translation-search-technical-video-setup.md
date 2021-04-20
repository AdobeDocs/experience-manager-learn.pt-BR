---
title: Configurar a pesquisa de tradução inteligente com o AEM Assets
description: A Pesquisa de tradução inteligente permite o uso de termos de pesquisa que não sejam em inglês para resolver para conteúdo em inglês. Para configurar o AEM para a Pesquisa de tradução inteligente, o pacote OSGi da Tradução do Apache Oak Machine deve ser instalado e configurado, bem como os pacotes de idioma Apache Joshua de código livre e aberto pertinentes que contêm as regras de tradução.
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---


# Configurar a pesquisa de tradução inteligente com o AEM Assets{#set-up-smart-translation-search-with-aem-assets}

A Pesquisa de tradução inteligente permite o uso de termos de pesquisa que não sejam em inglês para resolver para conteúdo em inglês. Para configurar o AEM para a Pesquisa de tradução inteligente, o pacote OSGi da Tradução do Apache Oak Machine deve ser instalado e configurado, bem como os pacotes de idioma Apache Joshua de código livre e aberto pertinentes que contêm as regras de tradução.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>A Pesquisa de tradução inteligente deve ser configurada em cada instância do AEM que precise dela.

1. Baixe e instale o pacote OSGi de Tradução do Oak Search Machine
   * [Baixe o ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) pacote OSGi da Tradução do Oak Search Machine que corresponde à versão Oak do AEM.
   * Instale o pacote Oak Search Machine Translation OSGi baixado no AEM via [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Baixe e atualize os pacotes de idioma do Apache Joshua
   * Baixe e descompacte os [Pacotes de idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) desejados.
   * Edite o arquivo `joshua.config` e comente as 2 linhas que começam com:

      ```
      feature-function = LanguageModel ...
      ```

   * Determine e registre o tamanho da pasta modelo do pacote de idiomas, pois isso influencia o espaço extra de heap que o AEM precisará.
   * Mova a pasta do pacote de idiomas Apache Joshua descompactada (com as edições `joshua.config`) para

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Por exemplo:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Reinicie o AEM com a alocação de memória heap atualizada
   * Parar o AEM
   * Determine o novo tamanho de heap necessário para o AEM

      * O tamanho do heap pré-falha de idioma do AEM + o tamanho do diretório do modelo arredondado para os 2 GB mais próximos
      * Por exemplo: Se os pacotes de pré-idioma da instalação do AEM precisarem de 8 GB de heap para serem executados e a pasta de modelo do pacote de idiomas estiver 3,8 GB descompactada, o novo tamanho do heap será:

         O `8GB` + original ( `3.75GB` arredondado para o `2GB` mais próximo, que é `4GB`) para um total de `12GB`
   * Verifique se a máquina tem essa quantidade de memória disponível extra.
   * Atualize os scripts de inicialização do AEM para ajustá-los para o novo tamanho de heap

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
   >O **heap base deve ser sempre calculado para suportar um desempenho aceitável sem qualquer pacote de idiomas** instalado.

4. Registre os pacotes de idiomas por meio das configurações OSGi do Provedor de Termos de Consulta de Texto Completo do Apache Jackrabbit Oak Machine Translation

   * Para cada pacote de idiomas, [crie um novo Apache Jackrabbit Oak Machine Translation Full-text Query Provider Configuração OSGi](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) por meio do Gerenciador de Configuração do AEM Web Console.

      * `Joshua Config Path` é o caminho absoluto para o arquivo joshua.config. O processo do AEM deve ser capaz de ler todos os arquivos na pasta do pacote de idiomas.
      * `Node types` são os tipos de nó candidatos cuja pesquisa de texto completo irá envolver este pacote de idiomas para tradução.
      * `Minimum score` é a pontuação de confiança mínima de um termo traduzido para que seja usado.

         * Por exemplo, hombre (Espanhol para &quot;homem&quot;) pode traduzir para a palavra inglesa &quot;man&quot; com uma pontuação de confiança de `0.9` e também traduzir para a palavra inglesa &quot;human&quot; com uma pontuação de confiança `0.2`. Ajustar a pontuação mínima para `0.3`, manteria a tradução &quot;hombre&quot; para &quot;man&quot;, mas descartaria a tradução &quot;hombre&quot; para &quot;humana&quot;, pois essa pontuação de tradução de `0.2` é menor que a pontuação mínima de `0.3`.

5. Executar uma pesquisa de texto completo em ativos
   * Como dam:Asset é o tipo de nó que este pacote de idiomas está registrado novamente, devemos procurar AEM Assets usando a pesquisa de texto completo para validar isso.
   * Navegue até AEM > Assets e abra o Omnisearch. Procure um termo no idioma cujo pacote de idiomas foi instalado.
   * Conforme necessário, ajuste a Pontuação mínima nas configurações de OSGi para garantir a precisão dos resultados.

6. Atualização de pacotes de idiomas
   * Os pacotes de idiomas do Apache Joshua são totalmente mantidos pelo projeto Apache Joshua e sua atualização ou correção é o critério do projeto Apache Joshua.
   * Se um pacote de idiomas for atualizado, para instalar as atualizações no AEM, siga as etapas 2 a 4 acima, ajustando o tamanho do heap para cima ou para baixo conforme necessário.

      * Observe que ao mover o pacote de idiomas descompactado para a pasta crx-quickstart/opt , mova qualquer pasta de pacote de idiomas existente antes de copiar na nova pasta.
   * Se o AEM não exigir uma reinicialização, a configuração do Provedor de Termos de Consulta de Texto Completo do Apache Jackrabbit Oak relevante para os pacotes de idiomas atualizados deverá ser salva novamente para que o AEM processe os arquivos atualizados.


## Atualização do índice damAssetLucene {#updating-damassetlucene-index}

Para que [Tags inteligentes do AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) sejam afetadas pela tradução inteligente do AEM, o índice `/oak   :index  /damAssetLucene` do AEM deve ser atualizado para marcar as Tags previstas (o nome do sistema para &quot;Tags inteligentes&quot;) como parte do índice Lucene agregado do AEM.

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