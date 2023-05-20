---
title: Acelere a velocidade do conteúdo com sistemas de estilo AEM
description: Saiba como usar Sistemas de estilo AEM para capacitar designers, autores de conteúdo e desenvolvedores em sua organização a criar e fornecer experiências na velocidade e escala que seus clientes esperam.
solution: Experience Manager
exl-id: 449cd133-6ab6-456e-a0ad-30e3dea9b75b
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 0%

---

# Acelere a velocidade do conteúdo com sistemas de estilo AEM

Neste artigo, você aprenderá a usar sistemas de estilo AEM para capacitar designers, autores de conteúdo e desenvolvedores em sua organização a criar e fornecer experiências na velocidade e escala que seus clientes esperam.

## Visão geral

Sistemas de estilo AEM têm quatro benefícios principais:

* Os autores dos modelos podem definir classes de estilo na política de conteúdo de um componente ou página
* Os autores de conteúdo podem selecionar estilos para aplicar a uma página inteira ou ao editar um componente em uma página
* Os componentes e modelos se tornam mais flexíveis, permitindo que os autores renderizem variações visuais alternativas
* A necessidade de desenvolver um componente personalizado e/ou caixas de diálogo complexas para apresentar variações de componentes é reduzida ou completamente eliminada

## Configuração e uso iniciais

A configuração de 5 etapas é muito semelhante a um fluxo de trabalho padrão de desenvolvimento de componentes.

| **Liderança** | **Designer** | **Desenvolvedor / Arquiteto** | **Autor do modelo** | **Autor de conteúdo** |
| --- | --- | --- | --- | --- |
| Determina o conteúdo e os objetivos desse componente | Determina a apresentação visual e experimental do conteúdo | Desenvolve CSS e JS para oferecer suporte à experiência; define e fornece nomes de classe a serem usados | Configura as políticas do modelo para componentes estilizados, adicionando nomes de classe CSS definidos pelos desenvolvedores. Nomes amigáveis devem ser usados para cada estilo. | Ao criar páginas, o aplica estilos conforme necessário para atingir a aparência desejada |

Embora essa seja a configuração inicial, muitos de nossos clientes obtiveram agilidade adicional simplificando esse processo, por exemplo, carregando o CSS no DAM, o que permite atualizações de estilos sem a necessidade de implantação. Outros clientes têm um conjunto completo de classes de utilitários, o que permite desenvolver componentes e estilos que podem ser aproveitados sem implantação ou desenvolvimento.

Os sistemas de estilo têm alguns sabores diferentes:

1. Estilos de layout

   * Alterações multifacetadas no design e layout

   * Usado para representação bem definida e identificável

1. Estilos de exibição
   * Pequenas variações que não alteram a natureza fundamental do estilo

   * Por exemplo, alterar o esquema de cores, a fonte, a orientação da imagem etc.

1. Estilos informativos

   * Mostrar/ocultar campos

>[!NOTE]
>
>Para ver uma demonstração desses recursos, recomendamos assistir nossa [Webinário de sucesso do cliente](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) com Will Brisbane e Joseph Van Buskirk.

## Práticas recomendadas

* Solidificar primeiro o estilo padrão
   * Layout e exibição do componente quando solto na página antes da aplicação de sistemas de estilo
   * Esta deve ser a representação mais usada
* Tentar mostrar apenas as opções de estilo que têm efeito quando possível
   * Se combinações ineficazes forem expostas, certifique-se de que elas não causem efeitos negativos
   * Por exemplo, Um estilo de layout que determina a posição da imagem e é acompanhado por um estilo de exibição ineficiente que controla a posição da imagem
* Optar por estilos de layout em vez de estilos de exibição combinados
   * Reduz o número de permutas que devem ser verificadas em relação à qualidade
   * Garante que os padrões da marca sejam seguidos
   * Simplifica a criação para autores de conteúdo
   * Ajuda a criar uma identidade consistente da marca do site
* Seja conservador com estilos combinados
   * Tanto em categorias quanto dentro de categorias
* Aloque tempo adequado para testar completamente os estilos combinados
   * Ajuda a evitar efeitos indesejáveis
* Minimizar o número de opções e permutas de estilo
   * Muitas opções podem levar à falta de consistência da marca para aparência
   * Pode causar confusão para os autores de conteúdo sobre quais combinações são necessárias para alcançar o efeito desejado
   * Aumenta as permutações que devem ser verificadas quanto à qualidade
* Usar rótulos e categorias de estilo amigáveis para empresas
   * &quot;Azul&quot; e &quot;Vermelho&quot; em vez de &quot;Primário&quot; e &quot;Secundário&quot;
   * &quot;Card&quot; e &quot;Hero&quot; em vez de &quot;Variation A&quot; e &quot;Variation B&quot;
   * Isso pode ser mais uma generalidade para alguns clientes; a equipe de design, a equipe de negócios e a equipe de conteúdo estão muito familiarizadas com o que são suas cores primária e secundária ou quais variações estão testando. Mas para flexibilidade e qualquer potencial para mudanças futuras, usar termos específicos pode ser mais eficiente.

## Principais aprendizados

Sistemas de estilo reduzem a necessidade de diálogos complexos, mas não são uma substituição de diálogo. Eles ajudam a simplificar as coisas, mas pode haver alguns casos em que você deseje usar propriedades de componentes ou caixas de diálogo em vez de criar um sistema de estilos para elas.

Eles podem simplificar os processos de uma perspectiva de desenvolvimento. Você pode obter várias aparências do mesmo conteúdo com um sistema de estilo. Da mesma forma, de uma perspectiva de criação, em vez de treinar autores, e autores tendo que lembrar qual componente usar em qual palácio, você pode acelerar a velocidade de criação.

As coisas estão simplesmente mais limpas. O HTML dos componentes principais é altamente explícito. Fazer tudo isso no nível CSS torna as criações do componente mais rápidas e o código também fica mais limpo.

Finalmente, o uso de sistemas de estilo é mais arte do que ciência. Conforme discutimos, há várias práticas recomendadas, mas você terá flexibilidade em como personalizar a configuração da sua organização.

Para obter mais informações, consulte nosso [Webinário de sucesso do cliente](https://adobecustomersuccess.adobeconnect.com/pob610c9mffjmp4/) com Will Brisbane e Joseph Van Buskirk.

Saiba mais sobre estratégia e liderança de pensamento na [Sucesso do cliente](https://experienceleague.adobe.com/docs/customer-success/customer-success/overview.html) hub.
