---
title: Integre os trabalhadores do Asset Compute aos Perfis de processamento do AEM
description: O AEM as a Cloud Service integra-se aos trabalhadores do Asset Compute implantados no Adobe I/O Runtime por meio de Perfis de processamento do AEM Assets. Os Perfis de processamento são configurados no serviço Autor para processar ativos específicos usando trabalhadores personalizados e armazenar os arquivos gerados pelos trabalhadores como representações de ativos.
feature: Microserviços do Asset Compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrações, desenvolvimento
role: Desenvolvedor
level: Intermediário, Experienciado
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 2%

---


# Integrar a Perfis de processamento AEM

Para que os trabalhadores do Asset Compute gerem representações personalizadas no AEM as a Cloud Service, eles devem ser registrados no serviço de Autor do AEM as a Cloud Service por meio de Perfis de processamento. Todos os ativos sujeitos a esse Perfil de processamento terão o trabalhador chamado ao fazer upload ou reprocessar, e a representação personalizada será gerada e disponibilizada por meio das representações do ativo.

## Definir um perfil de processamento

Primeiro crie um novo Perfil de processamento que chamará o trabalhador com os parâmetros configuráveis.

![Perfil de processamento](./assets/processing-profiles/new-processing-profile.png)

1. Faça logon no AEM as a Cloud Service Author como um __Administrador do AEM__. Como este é um tutorial, recomendamos usar um ambiente de desenvolvimento ou um ambiente em uma sandbox.
1. Navegue até __Ferramentas > Ativos > Perfis de processamento__
1. Toque no botão __Criar__
1. Nomeie o Perfil de processamento, `WKND Asset Renditions`
1. Toque na guia __Personalizado__ e toque em __Adicionar novo__
1. Definir o novo serviço
   + __Nome da representação:__ `Circle`
      + A representação do nome do arquivo que será usada para identificar essa representação no AEM Assets
   + __Extensão:__ `png`
      + A extensão da representação que será gerada. Defina como `png`, pois esse é o formato de saída compatível que o serviço da Web do trabalhador suporta, e resulta em um plano de fundo transparente atrás do recorte do círculo.
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Esse é o URL para o trabalhador obtido por meio de `aio app get-url`. Verifique se o URL aponta para o espaço de trabalho correto com base no ambiente do AEM as a Cloud Service.
      + Certifique-se de que o URL do trabalhador aponte para o espaço de trabalho correto. O AEM as a Cloud Service Stage deve usar o URL do espaço de trabalho Stage e o AEM as a Cloud Service Production deve usar o URL do espaço de trabalho Production.
   + __Parâmetros de serviço__
      + Toque em __Adicionar Parâmetro__
         + Chave: `size`
         + Valor: `1000`
      + Toque em __Adicionar Parâmetro__
         + Chave: `contrast`
         + Valor: `0.25`
      + Toque em __Adicionar Parâmetro__
         + Chave: `brightness`
         + Valor: `0.10`
      + Esses pares de chave/valor são passados para o trabalhador do Asset Compute e estão disponíveis por meio de `rendition.instructions` objeto JavaScript.
   + __Tipos de mime__
      + __Inclui:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Esses tipos MIME são os únicos que os módulos npm do trabalhador. Essa lista limita quais ativos serão processados pelo trabalhador personalizado.
      + __Exclui:__ `Leave blank`
         + Nunca processe ativos com esses tipos MIME usando essa configuração de serviço. Nesse caso, usamos apenas uma lista de permissões.
1. Toque em __Salvar__ no canto superior direito

## Aplicar e invocar um perfil de processamento

1. Selecione o Perfil de processamento recém-criado, `WKND Asset Renditions`
1. Toque em __Aplicar perfil à(s) pasta(s)__ na barra de ação superior
1. Selecione uma pasta à qual aplicar o Perfil de processamento, como `WKND` e toque em __Aplicar__
1. Navegue até a pasta à qual o Perfil de processamento não foi aplicado por meio de __AEM > Ativos > Arquivos__ e toque em `WKND`.
1. Faça upload de alguns novos ativos de imagens ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) e [sample-3.jpg](../assets/samples/sample-3.jpg)) em qualquer pasta sob a pasta com o Perfil de processamento aplicado e aguarde o ativo carregado ser processado.
1. Toque no ativo para abrir seus detalhes
   + As renderizações padrão podem gerar e aparecer mais rapidamente no AEM do que as renderizações personalizadas.
1. Abra a exibição __Representações__ na barra lateral esquerda
1. Toque no ativo chamado `Circle.png` e revise a representação gerada

   ![Representação gerada](./assets/processing-profiles/rendition.png)

## Concluído!

Parabéns! Você terminou o [tutorial](../overview.md) sobre como estender os microsserviços do Asset Compute do AEM as a Cloud Service! Agora você deve ter a capacidade de configurar, desenvolver, testar, depurar e implantar trabalhadores do Asset Compute personalizados para uso pelo serviço de autor do AEM as a Cloud Service.

### Revise o código-fonte completo do projeto no Github

O projeto final do Asset Compute está disponível no Github em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_O Github contém é o estado final do projeto, totalmente preenchido com o trabalhador e casos de teste, mas não contém nenhuma credencial, ou seja, `.env`,  `.config.json` ou  `.aio`._

## Resolução de problemas

+ [Representação personalizada ausente do ativo no AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [O processamento de ativos falha no AEM](../troubleshooting.md#asset-processing-fails)
