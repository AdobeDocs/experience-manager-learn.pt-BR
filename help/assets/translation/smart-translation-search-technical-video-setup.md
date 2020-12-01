---
title: Configurar a pesquisa de tradução inteligente com o AEM Assets
seo-title: Configurar a pesquisa de tradução inteligente com o AEM Assets
description: A pesquisa de tradução inteligente permite o uso de termos de pesquisa que não sejam em inglês para resolver o conteúdo em inglês. Para configurar AEM para a Pesquisa de Tradução Inteligente, o pacote OSGi de Tradução Automática do Apache Oak deve ser instalado e configurado, bem como os pacotes de idiomas Apache Joshua de código aberto e gratuito pertinentes que contêm as regras de tradução.
seo-description: A pesquisa de tradução inteligente permite o uso de termos de pesquisa que não sejam em inglês para resolver o conteúdo em inglês. Para configurar AEM para a Pesquisa de Tradução Inteligente, o pacote OSGi de Tradução Automática do Apache Oak deve ser instalado e configurado, bem como os pacotes de idiomas Apache Joshua de código aberto e gratuito pertinentes que contêm as regras de tradução.
uuid: b0e8dab2-6bc4-4158-91a1-4b9811359798
discoiquuid: 4db1b4db-74f4-4646-b5de-cb891612cc90
topics: authoring, search, metadata, localization
audience: administrator, developer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '929'
ht-degree: 0%

---


# Configurar a pesquisa de tradução inteligente com o AEM Assets{#set-up-smart-translation-search-with-aem-assets}

A pesquisa de tradução inteligente permite o uso de termos de pesquisa que não sejam em inglês para resolver o conteúdo em inglês. Para configurar AEM para a Pesquisa de Tradução Inteligente, o pacote OSGi de Tradução Automática do Apache Oak deve ser instalado e configurado, bem como os pacotes de idiomas Apache Joshua de código aberto e gratuito pertinentes que contêm as regras de tradução.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>A pesquisa de tradução inteligente deve ser configurada em cada instância AEM que a exija.

1. Baixe e instale o pacote Oak Search Machine Translation OSGi
   * [Baixe o ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) pacote OSGi de tradução do Oak Search Machine que corresponde à versão AEM Oak.
   * Instale o pacote Oak Search Machine Translation OSGi baixado em AEM via [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Baixe e atualize os pacotes de idiomas do Apache Joshua
   * Baixe e descompacte os [pacotes de idioma do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) desejados.
   * Edite o arquivo `joshua.config` e comente as 2 linhas que começam com:

      ```
      feature-function = LanguageModel ...
      ```

   * Determine e registre o tamanho da pasta modelo do pacote de idiomas, pois isso influencia o espaço extra AEM será necessário.
   * Mova a pasta do pacote de idiomas Apache Joshua descompactada (com as edições `joshua.config`) para

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Por exemplo:

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Reiniciar AEM com alocação de memória heap atualizada
   * Parar AEM
   * Determine o novo tamanho de heap necessário para AEM

      * AEM tamanho do heap sem idioma + o tamanho do diretório do modelo arredondado para os 2 GB mais próximos
      * Por exemplo: Se os pacotes de idiomas pré-existentes exigirem a execução de 8 GB de heap e a pasta de modelo do pacote de idiomas estiver 3,8 GB descompactada, o novo tamanho do heap será:

         O original `8GB` + ( `3.75GB` arredondado para cima até `2GB` mais próximo, que é `4GB`) para um total de `12GB`
   * Verifique se a máquina tem essa quantidade de memória extra disponível.
   * Atualize AEM scripts de start para ajustar para o novo tamanho do heap

      * Exemplo. `java -Xmx12g -jar cq-author-p4502.jar`
   * Reinicie o AEM com o tamanho do heap aumentado.

   >[!NOTE]
   >
   >O espaço de heap necessário para pacotes de idiomas pode ficar grande, especialmente quando vários pacotes de idiomas forem usados.
   >
   >
   >Certifique-se sempre de que **a instância tenha memória suficiente** para acomodar os aumentos no espaço de heap alocado.
   >
   >
   >O heap de **base deve ser sempre calculado para suportar um desempenho aceitável sem pacotes de idiomas** instalados.

4. Registre os pacotes de idiomas através das configurações OSGi do Provedor de Termos de Query de texto completo do Apache Jackrabbit Oak Machine Translation

   * Para cada pacote de idiomas, [crie um novo Apache Jackrabbit Oak Machine Translation Full-text Query Provider Configuração OSGi](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) através do gerenciador de Configuração do Console da Web AEM.

      * `Joshua Config Path` é o caminho absoluto para o arquivo joshua.config. O processo de AEM deve ser capaz de ler todos os arquivos na pasta do pacote de idiomas.
      * `Node types` são os tipos de nó candidatos cuja pesquisa em texto completo acionará este pacote de idiomas para tradução.
      * `Minimum score` é a pontuação de confiança mínima de um termo traduzido para ser usado.

         * Por exemplo, hombre (espanhol para &quot;homem&quot;) pode traduzir para a palavra inglesa &quot;homem&quot; com uma pontuação de confiança de `0.9` e também traduzir para a palavra inglesa &quot;humano&quot; com uma pontuação de confiança `0.2`. Ajustar a pontuação mínima para `0.3`, manterá a tradução &quot;hombre&quot; para &quot;man&quot;, mas descarte a tradução &quot;hombre&quot; para &quot;humana&quot;, já que essa pontuação de tradução de `0.2` é menor que a pontuação mínima de `0.3`.

5. Executar uma pesquisa em texto completo em ativos
   * Como dam:Asset é o tipo de nó que este pacote de idiomas está registrado novamente, precisamos pesquisar a AEM Assets usando a pesquisa de texto completo para validar isso.
   * Navegue até AEM > Ativos e abra o Omnisearch. Procure um termo no idioma cujo pacote de idiomas foi instalado.
   * Conforme necessário, ajuste a Pontuação mínima nas configurações do OSGi para garantir a precisão dos resultados.

6. Atualização de pacotes de idiomas
   * Os pacotes de idiomas do Apache Joshua são totalmente mantidos pelo projeto Apache Joshua, e sua atualização ou correção é como critério do projeto Apache Joshua.
   * Se um pacote de idiomas for atualizado, para instalar as atualizações no AEM, siga as etapas 2 a 4 acima, ajustando o tamanho do heap para cima ou para baixo conforme necessário.

      * Observe que ao mover o pacote de idiomas descompactado para a pasta crx-quickstart/opt, mova qualquer pasta de pacote de idiomas existente antes de copiar na nova pasta.
   * Se AEM não exigir uma reinicialização, a configuração OSGi do provedor de termos de Query de texto completo de Apache Jackrabbit Oak pertinente que pertencem ao(s) pacote(s) de idiomas atualizado(s) deverá(ão) ser salva novamente para AEM processar os arquivos atualizados.


## Atualização do damAssetLucene Index {#updating-damassetlucene-index}

Para que [AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) seja afetado por AEM Smart Translation, AEM índice `/oak   :index  /damAssetLucene` deve ser atualizado para marcar as predictedTags (o nome do sistema para &quot;Smart Tags&quot;) como parte do índice Lucene de agregação do Ativo.

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

* [Pacote OSGi de Tradução do Apache Oak Machine](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Pacotes de Idiomas do Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Tags inteligentes AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Práticas recomendadas para consulta e indexação](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)