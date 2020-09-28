---
title: Integre os funcionários da Asset Compute aos Perfis de processamento AEM
description: AEM como Cloud Service integra-se aos funcionários da Asset Compute implantados na Adobe I/O Runtime por meio dos Perfis de processamento AEM Assets. Os Perfis de processamento são configurados no serviço Autor para processar ativos específicos usando trabalhadores personalizados e armazenar os arquivos gerados pelos trabalhadores como representações de ativos.
feature: asset-compute, processing-profiles
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
translation-type: tm+mt
source-git-commit: 59bfc9ae08acca6c41234f23eaa60f56e2eda890
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 2%

---


# Integrar a Perfis de processamento AEM

Para que os funcionários da Asset Compute gerem representações personalizadas em AEM como um Cloud Service, eles devem estar registrados em AEM como um serviço de autor de Cloud Service via Perfis de processamento. Todos os ativos sujeitos a esse Perfil de processamento terão o trabalhador chamado após o upload ou o reprocessamento, e a representação personalizada será gerada e disponibilizada pelas representações do ativo.

## Definir um Perfil de processamento

Primeiro, crie um novo Perfil de processamento que chamará o trabalhador com os parâmetros configuráveis.

![Perfil de processamento](./assets/processing-profiles/new-processing-profile.png)

1. Faça logon no AEM como um serviço de autor de Cloud Service como um administrador __de__ AEM. Como este é um tutorial, recomendamos usar um ambiente Dev ou um ambiente em uma caixa de proteção.
1. Navegue até __Ferramentas > Ativos > Perfis de processamento__
1. Botão Tocar em __Criar__
1. Nomear o Perfil de processamento, `WKND Asset Renditions`
1. Toque na guia __Personalizado__ e toque em __Adicionar novo__
1. Definir o novo serviço
   + __Nome da representação:__ `Circle`
      + A execução do nome do arquivo que será usada para identificar essa execução no AEM Assets
   + __Extensão:__ `png`
      + A extensão da representação que será gerada. Definido como `png` o formato de saída suportado pelo serviço da Web do trabalhador e que resulta em plano de fundo transparente atrás do recorte do círculo.
   + __Ponto de extremidade:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Esse é o URL para o trabalhador obtido por meio do `aio app get-url`. Verifique se o URL aponta para a área de trabalho correta com base no AEM como um ambiente no qual o Perfil de processamento está sendo configurado. Observe que esse subdomínio corresponde ao espaço de `development` trabalho.
      + Verifique se o URL do trabalhador aponta para a área de trabalho correta. AEM como Cloud Service Stage deve usar o URL da área de trabalho Stage e AEM como um Cloud Service Production deve usar o URL da área de trabalho Production.
   + __Parâmetros de serviço__
      + Toque em __Adicionar parâmetro__
         + Chave: `size`
         + Valor: `1000`
      + Toque em __Adicionar parâmetro__
         + Chave: `contrast`
         + Valor: `0.25`
      + Toque em __Adicionar parâmetro__
         + Chave: `brightness`
         + Valor: `0.10`
      + Esses pares de chave/valor que são passados para o trabalho do Asset Compute e estão disponíveis por meio do objeto `rendition.instructions` JavaScript.
   + __Tipos de mime__
      + __Inclui:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Esses tipos MIME são os únicos compatíveis com o serviço Web do trabalhador, isso limita quais ativos podem ser processados pelo trabalhador personalizado.
      + __Exclui:__ `Leave blank`
         + Nunca processe ativos com esses tipos MIME usando essa configuração de serviço. Neste caso, usamos apenas uma lista de permissões.
1. Toque em __Salvar__ na parte superior direita

## Aplicar e invocar um Perfil de processamento

1. Selecione o Perfil de processamento recém-criado, `WKND Asset Renditions`
1. Toque em __Aplicar Perfil às pastas__ na barra de ação superior
1. Selecione uma pasta à qual aplicar o Perfil de processamento, como `WKND` e toque em __Aplicar__
1. Navegue até a pasta à qual o Perfil de processamento não foi aplicado por meio de __AEM > Ativos > Arquivos__ e toque em `WKND`.
1. Faça upload de alguns novos ativos de imagens ([exemplo-1.jpg](../assets/samples/sample-1.jpg), [exemplo-2.jpg](../assets/samples/sample-2.jpg)e [exemplo-3.jpg](../assets/samples/sample-3.jpg)) em qualquer pasta sob a pasta com o Perfil de processamento aplicado e aguarde o ativo carregado ser processado.
1. Toque no ativo para abrir seus detalhes
   + As renderizações padrão podem gerar e aparecer mais rapidamente em AEM do que as renderizações personalizadas.
1. Abra a visualização __Representações__ na barra lateral esquerda
1. Toque no ativo nomeado `Circle.png` e reveja a representação gerada

   ![Representação gerada](./assets/processing-profiles/rendition.png)

## Concluído!

Parabéns! Você concluiu o [tutorial](../overview.md) sobre como estender AEM como um Cloud Service Asset Compute microservices! Agora você deve ter a capacidade de configurar, desenvolver, testar, depurar e implantar funcionários personalizados da Asset Compute para uso pelo seu AEM como um serviço de Autor de Cloud Service.

## Resolução de problemas

### Representação personalizada ausente do ativo

+ __Erro:__ O processo de ativos novos e reprocessados foi bem-sucedido, mas a execução personalizada está ausente

#### Perfil de processamento não aplicado à pasta anterior

+ __Causa:__ O ativo não existe em uma pasta com o Perfil Processamento que usa o trabalhador personalizado
+ __Resolução:__ Aplicar o Perfil de processamento a uma pasta ancestral do ativo

#### Perfil de processamento substituído pelo Perfil de processamento mais baixo

+ __Causa:__ O ativo existe abaixo de uma pasta com o Perfil Processamento de trabalho personalizado aplicado, no entanto, um Perfil de processamento diferente que não usa o trabalhador do cliente foi aplicado entre essa pasta e o ativo.
+ __Resolução:__ Combine ou reconcilie os dois Perfis de processamento e remova o Perfil de processamento intermediário

### Falha no processamento do ativo

+ __Erro:__ Processamento de ativos Falha ao exibir o selo no ativo
+ __Causa:__ Ocorreu um erro na execução do trabalhador personalizado
+ __Resolução:__ Siga as instruções em [depurar o Adobe I/O Runtime ativação](../test-debug/debug.md#aio-app-logs) usando `aio app logs`.