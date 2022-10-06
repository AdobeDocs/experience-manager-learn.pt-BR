---
title: Integre os trabalhadores do Asset compute aos Perfis de processamento de AEM
description: O AEM as a Cloud Service integra-se aos trabalhadores do Asset compute implantados no Adobe I/O Runtime por meio de Perfis de processamento do AEM Assets. Os Perfis de processamento são configurados no serviço Autor para processar ativos específicos usando trabalhadores personalizados e armazenar os arquivos gerados pelos trabalhadores como representações de ativos.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# Integrar a Perfis de processamento de AEM

Para que os trabalhadores do Asset compute gerem representações personalizadas em AEM as a Cloud Service, eles devem ser registrados AEM serviço de Autor as a Cloud Service por meio de Perfis de processamento. Todos os ativos sujeitos a esse Perfil de processamento terão o trabalhador chamado ao fazer upload ou reprocessar, e a representação personalizada será gerada e disponibilizada por meio das representações do ativo.

## Definir um perfil de processamento

Primeiro crie um novo Perfil de processamento que chamará o trabalhador com os parâmetros configuráveis.

![Perfil de processamento](./assets/processing-profiles/new-processing-profile.png)

1. Faça logon AEM serviço de autor as a Cloud Service como um __Administrador AEM__. Como este é um tutorial, recomendamos usar um ambiente de desenvolvimento ou um ambiente em uma sandbox.
1. Navegar para __Ferramentas > Ativos > Perfis de processamento__
1. Toque __Criar__ botão
1. Nomeie o perfil de processamento, `WKND Asset Renditions`
1. Toque no __Personalizado__ e toque em __Adicionar novo__
1. Definir o novo serviço
   + __Nome da representação:__ `Circle`
      + O nome de arquivo da representação que usou para identificar essa representação no AEM Assets
   + __Extensão:__ `png`
      + A extensão da representação gerada. Defina como `png` como este é o formato de saída compatível que o serviço Web do trabalhador suporta e resulta em um plano de fundo transparente atrás do corte do círculo.
   + __Endpoint:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Esse é o URL para o trabalhador obtido por meio de `aio app get-url`. Verifique se o URL aponta para o espaço de trabalho correto com base no ambiente as a Cloud Service AEM.
      + Certifique-se de que o URL do trabalhador aponte para o espaço de trabalho correto. AEM as a Cloud Service Stage deve usar o URL do espaço de trabalho Stage e AEM Produção as a Cloud Service deve usar o URL do espaço de trabalho Production.
   + __Parâmetros de serviço__
      + Toque __Adicionar parâmetro__
         + Chave: `size`
         + Valor: `1000`
      + Toque __Adicionar parâmetro__
         + Chave: `contrast`
         + Valor: `0.25`
      + Toque __Adicionar parâmetro__
         + Chave: `brightness`
         + Valor: `0.10`
      + Esses pares chave/valor são passados para o trabalhador do Asset compute e disponibilizados por meio de `rendition.instructions` Objeto JavaScript.
   + __Tipos de mime__
      + __Inclui:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Esses tipos MIME são os únicos que os módulos npm do trabalhador. Essa lista limita os limites que são processados pelo trabalhador personalizado.
      + __Exclui:__ `Leave blank`
         + Nunca processe ativos com esses tipos MIME usando essa configuração de serviço. Nesse caso, usamos apenas uma lista de permissões.
1. Toque __Salvar__ no canto superior direito

## Aplicar e invocar um perfil de processamento

1. Selecione o Perfil de processamento recém-criado, `WKND Asset Renditions`
1. Toque __Aplicar perfil às pastas__ na barra de ação superior
1. Selecione uma pasta para aplicar o Perfil de processamento, como `WKND` e tocar __Aplicar__
1. Navegue até a pasta à qual o Perfil de processamento não foi aplicado por meio de __AEM > Ativos > Arquivos__ e toque em `WKND`.
1. Faça upload de alguns novos ativos de imagens ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg)e [sample-3.jpg](../assets/samples/sample-3.jpg)) em qualquer pasta da pasta com o Perfil de processamento aplicado e aguarde o ativo carregado ser processado.
1. Toque no ativo para abrir seus detalhes
   + As representações padrão podem gerar e aparecer mais rapidamente em AEM do que as representações personalizadas.
1. Abra o __Representações__ exibir na barra lateral esquerda
1. Toque no ativo nomeado `Circle.png` e revisar a representação gerada

   ![Representação gerada](./assets/processing-profiles/rendition.png)

## Concluído!

Parabéns. Você terminou o [tutorial](../overview.md) sobre como estender AEM microsserviços de Asset compute as a Cloud Service! Agora você deve ter a capacidade de configurar, desenvolver, testar, depurar e implantar trabalhadores do Asset compute personalizados para uso pelo seu serviço de Autor as a Cloud Service AEM.

### Revise o código-fonte completo do projeto no Github

O projeto final do Asset compute está disponível no Github em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_O Github contém é o estado final do projeto, totalmente preenchido com o trabalhador e casos de teste, mas não contém nenhuma credencial, ou seja, `.env`, `.config.json` ou `.aio`._

## Resolução de problemas

+ [Representação personalizada ausente do ativo no AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [O processamento de ativos falha no AEM](../troubleshooting.md#asset-processing-fails)
