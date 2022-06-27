---
title: Acelere a velocidade do conteúdo com sistemas de estilo AEM
description: Saiba como usar AEM Style Systems para capacitar designers, autores de conteúdo e desenvolvedores em sua organização a criar e fornecer experiências na velocidade e escala que seus clientes esperam.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: 471f0fe940abb8241428beb14896d83e140136b3
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 0%

---

# Acelere a velocidade do conteúdo com sistemas de estilo AEM

Neste artigo, você aprenderá a usar sistemas de estilo AEM para capacitar designers, autores de conteúdo e desenvolvedores em sua organização a criar e fornecer experiências na velocidade e escala que seus clientes esperam.

## Visão geral

AEM Style Systems tem quatro benefícios principais:

* Os autores de modelos podem definir classes de estilo na política de conteúdo de um componente ou página
* Os autores de conteúdo podem selecionar estilos para aplicar a uma página inteira ou ao editar um componente em uma página
* Os componentes e modelos são mais flexíveis ao permitir que os autores renderizem variações visuais alternativas
* A necessidade de desenvolver um componente personalizado e/ou caixas de diálogo complexas para apresentar variações de componentes é reduzida ou totalmente eliminada

## Configuração inicial e uso

A configuração de 5 etapas é muito semelhante a um fluxo de trabalho de desenvolvimento de componente padrão.

| **Liderança** | **Designer** | **Desenvolvedor/Arquiteto** | **Autor do modelo** | **Autor do conteúdo** |
| --- | --- | --- | --- | --- |
| Determina o conteúdo e os objetivos desse componente | Determina a apresentação visual e experimental do conteúdo | Desenvolve CSS e JS para oferecer suporte à experiência; define e fornece nomes de classe a serem usados | Configura políticas de modelo para componentes estilizados adicionando nomes de classe CSS definidos por desenvolvedores. Nomes amigáveis devem ser usados para cada estilo. | Ao criar páginas, aplica estilos conforme necessário para atingir a aparência desejada |

Embora essa seja a configuração inicial, muitos de nossos clientes tiveram agilidade adicional ao simplificar esse processo, por exemplo, ao fazer upload de seu CSS no DAM, o que permite atualizações em estilos sem a necessidade de implantação. Outros clientes têm um conjunto completo de classes de utilitários, o que permite desenvolver componentes e estilos que podem ser aproveitados sem implantação ou desenvolvimento.

Os sistemas de estilos apresentam alguns tipos diferentes:

1. Estilos de layout

   * Alterações multifacetadas no design e layout

   * Usado para representação bem definida e identificável

1. Exibir estilos
   * Pequenas variações que não alteram a natureza fundamental do estilo

   * Por exemplo, alteração do esquema de cores, fonte, orientação da imagem etc.

1. Estilos informativos

   * Mostrar/ocultar campos

>[!NOTE]
>
>Para uma demonstração desses recursos, recomendamos assistir ao [Webinário de sucesso do cliente](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) com Will Brisbane e Joseph Van Buskirk.

## Práticas recomendadas

* Solidificar primeiro o estilo padrão
   * Layout e exibição do componente quando solto na página antes da aplicação de sistemas de estilos
   * Essa deve ser a representação mais usada
* Tente mostrar apenas as opções de estilo que têm um efeito quando possível
   * Se as combinações ineficazes estiverem expostas, certifique-se de que não causam efeitos negativos
   * Por exemplo, um estilo de layout que determina a posição da imagem e é acompanhado por um estilo de exibição ineficaz que controla a posição da imagem
* Optar por estilos de layout em vez de estilos de exibição combinados
   * Reduz o número de permutas que devem ser verificadas por qualidade
   * Garante o cumprimento dos padrões da marca
   * Simplifica a criação para autores de conteúdo
   * Ajuda a criar uma identidade de marca de site consistente
* Seja conservador com estilos combinados
   * Em todas as categorias e dentro delas
* Aloque o tempo adequado para testar estilos combinados completamente
   * Ajuda a evitar efeitos indesejáveis
* Minimize o número de opções e permutas de estilo
   * Muitas opções podem levar à falta de consistência da marca para aparência e comportamento
   * Pode causar confusão para os autores de conteúdo em que as combinações são necessárias para atingir o efeito desejado
   * Aumenta as permutas que devem ser verificadas por qualidade
* Usar rótulos e categorias de estilo comerciais amigáveis ao usuário
   * &quot;Azul&quot; e &quot;Vermelho&quot; em vez de &quot;Primário&quot; e &quot;Secundário
   * &quot;Cartão&quot; e &quot;Herói&quot; em vez de &quot;Variação A&quot; e &quot;Variação B&quot;
   * Tal pode ser mais generalista para alguns clientes; a equipe de design, a equipe de negócios e a equipe de conteúdo estão familiarizadas com as cores primárias e secundárias ou com que variações estão testando. Mas, para a flexibilidade e para qualquer potencial de mudanças futuras, a utilização de termos específicos pode ser mais eficiente.

## Capturas de chave

Os sistemas de estilos reduzem a necessidade de caixas de diálogo complexas, mas não são uma substituição de caixa de diálogo. Eles ajudam a simplificar as coisas, mas pode haver alguns casos em que você deseja usar as propriedades do componente ou a caixa de diálogo, em vez de criar um sistema de estilos para ele.

Eles podem simplificar processos de uma perspectiva de desenvolvimento. Você pode obter várias aparências do mesmo conteúdo com um sistema de estilos. Da mesma forma, de uma perspectiva de criação, em vez de treinar autores e autores que precisam lembrar qual componente usar em qual palácio, você pode acelerar a velocidade de criação.

As coisas são simplesmente mais limpas. A HTML dentro dos componentes principais é altamente detalhada. Fazer tudo isso no nível de CSS torna as criações do componente mais rápidas e o código também é mais limpo.

Por fim, o uso de Sistemas de Estilo é mais arte do que ciência. Conforme discutimos, há várias práticas recomendadas, mas você terá flexibilidade para personalizar a configuração de sua organização.

Para obter mais informações, consulte nossa [Webinar de sucesso do cliente](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) com Will Brisbane e Joseph Van Buskirk.

Saiba mais sobre estratégia e liderança de pensamento no [Sucesso do cliente](https://experienceleague.corp.adobe.com/docs/customer-success/customer-success/overview.html) cubo.
