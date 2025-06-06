---
title: Integrar os trabalhadores do Asset Compute aos perfis de processamento do AEM
description: O AEM as a Cloud Service integra-se aos funcionários da Asset Compute implantados no Adobe I/O Runtime por meio de perfis de processamento do AEM Assets. Os Perfis de processamento são configurados no serviço Autor para processar ativos específicos usando trabalhadores personalizados e armazenar os arquivos gerados pelos trabalhadores como representações de ativos.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 126
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 0%

---

# Integrar a perfis de processamento do AEM

Para que os trabalhadores do Asset Compute gerem representações personalizadas no AEM as a Cloud Service, eles devem ser registrados no serviço de Autor do AEM as a Cloud Service por meio de Perfis de processamento. Todos os ativos sujeitos a esse Perfil de processamento terão o trabalhador chamado após o upload ou o reprocessamento e a representação personalizada será gerada e disponibilizada por meio das representações do ativo.

## Definir um perfil de processamento

Primeiro, crie um novo Perfil de processamento que chamará o trabalhador com os parâmetros configuráveis.

![Processando perfil](./assets/processing-profiles/new-processing-profile.png)

1. Faça logon no serviço de Autor do AEM as a Cloud Service como um __Administrador do AEM__. Como este é um tutorial, recomendamos usar um ambiente de desenvolvimento ou um ambiente em uma sandbox.
1. Navegue até __Ferramentas > Assets > Processando Perfis__
1. Botão __Criar__
1. Nomeie o Perfil de Processamento, `WKND Asset Renditions`
1. Toque na guia __Personalizado__ e em __Adicionar novo__
1. Definir o novo serviço
   + __Nome da representação:__ `Circle`
      + O nome do arquivo de representação que foi usado para identificar essa representação no AEM Assets
   + __Extensão:__ `png`
      + A extensão da representação gerada. Defina como `png`, pois esse é o formato de saída com suporte pelo serviço Web do trabalhador e resulta em plano de fundo transparente atrás do círculo recortado.
   + __Ponto de extremidade:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Esta é a URL para o trabalhador obtida via `aio app get-url`. Verifique se o URL aponta para o espaço de trabalho correto com base no ambiente do AEM as a Cloud Service.
      + Verifique se o URL do trabalhador aponta para o espaço de trabalho correto. O AEM as a Cloud Service Stage deve usar o URL do espaço de trabalho Stage, e o AEM as a Cloud Service Production deve usar o URL do espaço de trabalho Production.
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
      + Esses pares de chave/valor passados para o Asset Compute Worker e disponíveis por meio do objeto JavaScript `rendition.instructions`.
   + __Tipos MIME__
      + __Inclui:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Esses tipos MIME são os únicos dos módulos npm do trabalhador. Esta lista limita os que são processados pelo funcionário personalizado.
      + __Exclusões:__ `Leave blank`
         + Nunca processe ativos com esses Tipos MIME usando essa configuração de serviço. Nesse caso, usamos apenas uma lista de permissões.
1. Toque em __Salvar__ na parte superior direita

## Aplicar e invocar um perfil de processamento

1. Selecione o perfil de processamento recém-criado, `WKND Asset Renditions`
1. Toque em __Aplicar perfil às pastas__ na barra de ações superior
1. Selecione uma pasta à qual aplicar o Perfil de Processamento, como `WKND`, e toque em __Aplicar__
1. Navegue até a pasta à qual o Perfil de Processamento não foi aplicado via __AEM > Assets > Arquivos__ e toque em `WKND`.
1. Carregue alguns novos ativos de imagens ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) e [sample-3.jpg](../assets/samples/sample-3.jpg)) em qualquer pasta na pasta com o Perfil de Processamento aplicado e aguarde até que o ativo carregado seja processado.
1. Toque no ativo para abrir os detalhes
   + As representações padrão podem ser geradas e exibidas mais rapidamente no AEM do que as representações personalizadas.
1. Abra a exibição __Representações__ na barra lateral esquerda
1. Toque no ativo chamado `Circle.png` e revise a representação gerada

   ![Representação gerada](./assets/processing-profiles/rendition.png)

## Concluído!

Parabéns! Você concluiu o [tutorial](../overview.md) sobre como estender os microsserviços do AEM as a Cloud Service Asset Compute! Agora você deve ter a capacidade de configurar, desenvolver, testar, depurar e implantar trabalhadores personalizados do Asset Compute para uso pelo serviço de autor do AEM as a Cloud Service.

### Revisar o código-fonte completo do projeto no Github

O projeto final do Asset Compute está disponível no Github em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contém é o estado final do projeto, totalmente preenchido com os casos de trabalho e de teste, mas não contém credenciais, ou seja, `.env`, `.config.json` ou `.aio`._

## Resolução de problemas

+ [Representação personalizada ausente do ativo no AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [O processamento de ativos falha no AEM](../troubleshooting.md#asset-processing-fails)
