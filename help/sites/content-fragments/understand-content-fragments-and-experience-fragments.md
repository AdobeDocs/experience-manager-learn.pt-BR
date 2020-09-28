---
title: Compreender fragmentos de conteúdo e fragmentos de experiência
description: Os Fragmentos de conteúdo e Fragmentos de experiência da Adobe Experience Manager podem parecer semelhantes na superfície, mas cada um desempenha funções chave em casos de uso diferentes. Saiba como Fragmentos de conteúdo e Fragmentos de experiência são semelhantes, diferentes e quando e como usar cada um.
sub-product: ativos, sites, serviços de conteúdo
feature: content fragments, experience fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
translation-type: tm+mt
source-git-commit: 03db12de4d95ced8fabf36b8dc328581ec7a2749
workflow-type: tm+mt
source-wordcount: '998'
ht-degree: 3%

---


# Compreender fragmentos de conteúdo e fragmentos de experiência

Os Fragmentos de conteúdo e Fragmentos de experiência da Adobe Experience Manager podem parecer semelhantes na superfície, mas cada um desempenha funções chave em casos de uso diferentes. Saiba como Fragmentos de conteúdo e Fragmentos de experiência são semelhantes, diferentes e quando e como usar cada um.

## Comparação de fragmentos de conteúdo e fragmentos de experiência

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragmentos de conteúdo (CF)</strong></td>
<td><strong>Fragmentos de experiência (XF)</strong></td>
</tr><tr><td><strong>Definição</strong></td>
<td><ul>
<li>Reutilizável, <strong>conteúdo</strong>agnóstico de apresentação, composto de elementos de dados estruturados (texto, datas, referências etc.)</li>
</ul>
</td>
<td><ul>
<li>Um composto reutilizável de um ou mais AEM Componentes que definem o conteúdo e a apresentação que formam uma <strong>experiência</strong> que faz sentido por si só</li>
</ul>
</td>
</tr><tr><td><strong>Locatários principais</strong></td>
<td><ul>
<li>Centralizado ao conteúdo</li>
<li>Definido por um modelo de dados <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">estruturado, baseado em forma.</a></li>
<li>Design e layout agnósticos.</li>
<li>O canal possui a apresentação do conteúdo do Fragmento de conteúdo (layout e design)</li>
</ul>
</td>
<td><ul>
<li>Centrado na apresentação</li>
<li>Definido pela composição não estruturada de Componentes AEM</li>
<li>Define o design e o layout do conteúdo</li>
<li>Usado "no estado em que se encontra" em canais</li>
</ul>
</td>
</tr><tr><td><strong>Detalhes técnicos</strong></td>
<td><ul>
<li>Implementado como um <strong>dam:Asset</strong></li>
<li>Definido por um modelo de fragmento <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">de conteúdo</a></li>
</ul>
</td>
<td><ul>
<li>Implementado como um <strong>cq:Page</strong></li>
<li>Definido por modelos editáveis</li>
<li>Execução HTML nativa</li>
</ul>
</td>
</tr><tr><td><strong>Variações</strong></td>
<td><ul>
<li>A variação Principal é a variação canônica</li>
<li>As variações são específicas de maiúsculas e minúsculas, que podem se alinhar aos canais.</li>
</ul>
</td>
<td><ul>
<li>As variações são específicas de canal ou contexto</li>
<li>As variações são mantidas em sincronia por meio do AEM Live Copy</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Os blocos</a> de construção permitem a reutilização do conteúdo entre variações</li>
</ul>
</td>
</tr><tr><td><strong>Recursos</strong></td>
<td><ul>
<li>Variações</li>
<li>Versões</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank">Sincronização</a> de conteúdo entre variações</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Diferença</a> visual das versões do Fragmento de conteúdo</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank">Anotações</a> de elementos de texto de várias linhas</li>
<li>Resumo <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank"></a> inteligente de elementos de texto de várias linhas.</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">Tradução/localização</a></li>
</ul>
</td>
<td><ul>
<li>Variações</li>
<li>Variações como Live Copies</li>
<li>Versões</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">Elementos básicos</a></li>
<li>Anotações</li>
<li>Layout responsivo e pré-visualização</li>
<li>Tradução/localização</li>
</ul>
</td>
</tr><tr><td><strong>Uso</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">Componentes principais AEM componente</a> Fragmento de conteúdo para uso no AEM Sites, AEM Screens ou em Fragmentos de experiência.</li>
<li>Exportação JSON via <a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a> para consumo de terceiros</li>
<li>JSON por meio AEM APIs de ativos HTTP para consumo de terceiros.</li>
</ul>
</td>
<td><ul>
<li>AEM componente Fragmento de experiência para uso no AEM Sites, AEM Screens ou outros Fragmentos de experiência.</li>
<li>Exportar como HTML <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank"></a> simples para uso por sistemas de terceiros</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">Exportação de HTML para Adobe Target</a> para ofertas direcionadas</li>
<li>Exportação JSON para Adobe Target para ofertas direcionadas</li>
</ul>
</td>
</tr><tr><td><strong>Casos de uso frequente</strong></td>
<td><ul>
<li>Conteúdo altamente estruturado baseado em formulários/entrada de dados</li>
<li>Conteúdo editorial de forma longa (elemento multilinha)</li>
<li>Conteúdo gerenciado fora do ciclo de vida dos canais que o entregam</li>
</ul>
</td>
<td><ul>
<li>Gerenciamento centralizado de materiais promocionais de vários canais usando variações por canal.</li>
<li>Conteúdo reutilizado em várias páginas em um site.</li>
<li>Cromo do site (ex. cabeçalho e rodapé)</li>
<li>Uma experiência gerenciada fora do ciclo de vida dos canais que a entregam</li>
</ul>
</td>
</tr><tr><td><strong>Documentação</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guia do usuário de fragmentos de conteúdo AEM</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">Uso de fragmentos de conteúdo em AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">Documentação do Adobe em fragmentos de experiência</a></li>
</ul>
</td>
</tr></tbody></table>

## Arquitetura de fragmentos de conteúdo

O diagrama a seguir ilustra a arquitetura geral para AEM Fragmentos de conteúdo

!![Arquitetura de fragmentos de conteúdo](./assets/content-fragments-architecture.png)

+ **Modelos** de fragmento de conteúdo definem os elementos (ou campos) que definem qual conteúdo o Fragmento de conteúdo pode capturar e expor.
+ O Fragmento **de** conteúdo é uma instância de um Modelo de fragmento de conteúdo que representa uma entidade de conteúdo lógico.
+ No entanto, **as variações** do Fragmento do conteúdo seguem o Modelo do fragmento do conteúdo, mas têm variações no conteúdo.
+ Fragmentos de conteúdo podem ser expostos/consumidos por:
   + Uso de fragmentos de conteúdo no **AEM Sites** (ou AEM Screens) por meio do componente Fragmento de conteúdo dos componentes principais do AEM WCM.
   + Como incorporar um fragmento de conteúdo em um fragmento **de** experiência por meio do componente Fragmento de conteúdo dos componentes principais do AEM WCM, para uso em qualquer caso de uso do Fragmento de experiência.
   + Expor um fragmento de conteúdo altera o conteúdo como JSON por meio de **AEM Content Services** e páginas de API para casos de uso somente leitura.
   + Exposição direta do conteúdo do fragmento de conteúdo (todas as variações) como JSON por meio de chamadas diretas para a AEM Assets por meio da API **HTTP da** AEM Assets para casos de uso de CRUD.

## Arquitetura de fragmentos de experiência

!![Arquitetura de fragmentos de experiência](./assets/experience-fragments-architecture.png)

+ **Modelos** editáveis, que por sua vez são definidos por Tipos **de modelo** editáveis e uma implementação **de componente de Página** AEM, definem os componentes AEM permitidos que podem ser usados para compor um Fragmento de experiência.
+ O Fragmento **de** experiência é uma instância de um Modelo editável que representa uma experiência lógica.
+ Entretanto, **as variações** do fragmento de experiência seguem o modelo editável, mas têm variações na experiência (conteúdo e design).
+ Fragmentos de experiência podem ser expostos/consumidos por:
   + Uso de fragmentos de experiência no AEM Sites (ou AEM Screens) por meio do componente AEM do Fragmento de experiência.
   + A exposição de um fragmento de experiência altera o conteúdo como JSON (com HTML incorporado) por meio de **AEM Content Services** e páginas de API.
   + Exposição direta de uma variação do Fragmento de experiência como **&quot;HTML simples&quot;**.
   + Exportar fragmentos de experiência para o **Adobe Target** como ofertas HTML ou JSON.
   + A AEM Sites suporta nativamente ofertas HTML, no entanto, as ofertas JSON exigem desenvolvimento personalizado.

## Materiais de suporte para fragmentos de conteúdo

+ [Guia do usuário de fragmentos de conteúdo](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Uso de fragmentos de conteúdo em AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [Componente de fragmento de conteúdo dos componentes principais do AEM WCM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Uso de fragmentos de conteúdo e AEM serviços de conteúdo](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [Introdução ao AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## Materiais de suporte para fragmentos de experiência

+ [Documentação do Adobe em fragmentos de experiência](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [Como entender AEM fragmentos de experiência](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [Uso AEM fragmentos de experiência](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Uso de AEM fragmentos de experiência com o Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
