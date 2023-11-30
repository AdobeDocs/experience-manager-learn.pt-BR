---
title: Integrar trabalhadores Assets compute com perfis de processamento AEM
description: O AEM as a Cloud Service integra-se aos funcionários do Asset compute implantados no Adobe I/O Runtime por meio de perfis de processamento do AEM Assets. Os Perfis de processamento são configurados no serviço Autor para processar ativos específicos usando trabalhadores personalizados e armazenar os arquivos gerados pelos trabalhadores como representações de ativos.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 2%

---

# Integrar a perfis de processamento AEM

Para que os trabalhadores do Asset compute gerem representações personalizadas no AEM as a Cloud Service AEM as a Cloud Service, eles devem ser registrados no serviço de Autor do por meio de Perfis de processamento. Todos os ativos sujeitos a esse Perfil de processamento terão o trabalhador chamado após o upload ou o reprocessamento e a representação personalizada será gerada e disponibilizada por meio das representações do ativo.

## Definir um perfil de processamento

Primeiro, crie um novo Perfil de processamento que chamará o trabalhador com os parâmetros configuráveis.

![Processando perfil](./assets/processing-profiles/new-processing-profile.png)

1. Faça logon no serviço de Autor as a Cloud Service do AEM como um __Administrador AEM__. Como este é um tutorial, recomendamos usar um ambiente de desenvolvimento ou um ambiente em uma sandbox.
1. Navegue até __Ferramentas > Ativos > Perfis de processamento__
1. Toque __Criar__ botão
1. Nomeie o perfil de processamento, `WKND Asset Renditions`
1. Toque no __Personalizado__ e toque em __Adicionar novo__
1. Definir o novo serviço
   + __Nome da representação:__ `Circle`
      + O nome do arquivo de representação que foi usado para identificar essa representação no AEM Assets
   + __Extensão:__ `png`
      + A extensão da representação gerada. Defina como `png` como esse é o formato de saída compatível, o serviço da web do worker oferece suporte e resulta em um plano de fundo transparente atrás do círculo recortado.
   + __Ponto de extremidade:__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Este é o URL para o trabalhador obtido via `aio app get-url`. Verifique se o URL aponta para o espaço de trabalho correto com base no ambiente as a Cloud Service AEM.
      + Verifique se o URL do trabalhador aponta para o espaço de trabalho correto. O Estágio as a Cloud Service do AEM deve usar o URL do espaço de trabalho do Estágio, e a Produção as a Cloud Service do AEM deve usar o URL do espaço de trabalho Produção.
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
      + Esses pares de chave/valor passados para o trabalhador do Asset compute e disponíveis via `rendition.instructions` Objeto JavaScript.
   + __Tipos de mime__
      + __Inclui:__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Esses tipos MIME são os únicos dos módulos npm do trabalhador. Esta lista limita os que são processados pelo funcionário personalizado.
      + __Exclui:__ `Leave blank`
         + Nunca processe ativos com esses Tipos MIME usando essa configuração de serviço. Nesse caso, usamos apenas uma lista de permissões.
1. Toque __Salvar__ na parte superior direita

## Aplicar e invocar um perfil de processamento

1. Selecione o perfil de processamento recém-criado, `WKND Asset Renditions`
1. Toque __Aplicar perfil às pastas__ na barra de ação superior
1. Selecione uma pasta à qual aplicar o perfil de processamento, como `WKND` e toque em __Aplicar__
1. Navegue até a pasta à qual o perfil de processamento não foi aplicado por meio de __AEM > Ativos > Arquivos__ e toque em `WKND`.
1. Fazer upload de alguns novos ativos de imagens ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg), e [sample-3.jpg](../assets/samples/sample-3.jpg)) em qualquer pasta na pasta com o Perfil de processamento aplicado e aguarde o ativo carregado ser processado.
1. Toque no ativo para abrir os detalhes
   + As representações padrão podem ser geradas e exibidas mais rapidamente no AEM do que as representações personalizadas.
1. Abra o __Representações__ exibir na barra lateral esquerda
1. Toque no ativo chamado `Circle.png` e revisar a representação gerada

   ![Representação gerada](./assets/processing-profiles/rendition.png)

## Concluído!

Parabéns! Você concluiu o [tutorial](../overview.md) sobre como estender os microsserviços do Asset compute as a Cloud Service AEM! Agora você deve ter a capacidade de configurar, desenvolver, testar, depurar e implantar funcionários do Asset compute personalizados para uso pelo serviço de Autor as a Cloud Service do AEM.

### Revisar o código-fonte completo do projeto no Github

O projeto final do Asset compute está disponível no Github em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_O GitHub contém o estado final do projeto, totalmente preenchido com os casos de trabalhador e de teste, mas não contém credenciais, ou seja, `.env`, `.config.json` ou `.aio`._

## Resolução de problemas

+ [Representação personalizada ausente do ativo no AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [O processamento de ativos falha no AEM](../troubleshooting.md#asset-processing-fails)
