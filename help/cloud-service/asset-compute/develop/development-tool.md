---
title: Ferramenta de desenvolvimento de assets compute
description: A Ferramenta de desenvolvimento de Asset compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset compute no Adobe I/O Runtime.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# Ferramenta de desenvolvimento de assets compute

A Ferramenta de desenvolvimento de Asset compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset compute no Adobe I/O Runtime.

## Executar a ferramenta de desenvolvimento de Assets compute

A Ferramenta de desenvolvimento de Assets compute pode ser executada a partir da raiz do projeto Asset compute por meio do comando terminal :

```
$ aio app run
```

Isso iniciará a Ferramenta de desenvolvimento em __http://localhost:9000__ e a abrirá automaticamente em uma janela do navegador. Para que a Ferramenta de desenvolvimento seja executada, [um devToolToken válido gerado automaticamente deve ser fornecido por meio de um parâmetro de consulta](#troubleshooting__devtooltoken).

## Entender a interface das Ferramentas de desenvolvimento do Asset compute{#interface}

![Ferramenta de desenvolvimento de assets compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Arquivo de origem:__ A seleção do arquivo de origem é usada para:
   + Selecionado o binário de ativo que será o binário `source` passado para o trabalhador do Asset compute
   + Upload de arquivos de origem
1. __Definição de perfil(s) de asset compute:__ Define o trabalhador do Asset compute a ser executado, incluindo parâmetros: incluindo o ponto final do URL do trabalhador, o nome da representação resultante e quaisquer parâmetros
1. __Executar:__ O botão Executar executa o perfil do Asset compute, conforme definido no editor de perfil de configuração do Asset compute
1. __Abortar:__ o botão Abortar cancela uma execução iniciada a partir do toque no botão Executar
1. __Solicitação/resposta:__ fornece a solicitação e a resposta HTTP para/do trabalhador do Asset compute em execução no Adobe I/O Runtime. Isso pode ser útil para depurar
1. __Logs de ativação:__ os logs que descrevem a execução do trabalhador do Asset compute, juntamente com quaisquer erros. Essas informações também estão disponíveis no padrão `aio app run`
1. __Representações:__ Exibe todas as representações geradas pela execução do Asset compute
1. __parâmetro de consulta devToolToken:__ o token de Ferramenta de Desenvolvimento de Asset compute requer que um parâmetro de  `devToolToken` consulta válido esteja presente. Esse token é gerado automaticamente sempre que uma nova Ferramenta de desenvolvimento é gerada

### Executar um trabalhador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through da execução de um trabalho de Asset compute na Ferramenta de desenvolvimento (Sem áudio)_

1. Certifique-se de que a Ferramenta de desenvolvimento de Assets compute foi iniciada a partir da raiz do seu projeto usando o comando `aio app run`.
1. Na Ferramenta de desenvolvimento de Assets compute, carregue ou selecione um [arquivo de imagem de amostra](../assets/samples/sample-file.jpg)
   + Verifique se o arquivo está selecionado na lista suspensa __Source file__
1. Revise a área de texto __Definição de perfil de Asset compute__
   + A chave `worker` define o URL para o trabalhador do Asset compute implantado
   + A chave `name` define o nome da representação a ser gerada
   + Outros valores/chaves podem ser fornecidos neste objeto JSON e estarão disponíveis no trabalhador sob o objeto `rendition.instructions`
      + Opcionalmente, adicione valores para `size`, `contrast` e `brightness`:

         ```json
         {
             "renditions": [
                 {
                     "worker": "...",
                     "name": "rendition.png",
                     "size":"800",
                     "contrast": "0.30",
                     "brightness": "-0.15"
                 }
             ]
         }
         ```

1. Toque no botão __Executar__
1. A seção __Representações__ será preenchida com um espaço reservado de representação
1. Quando o trabalhador for concluído, o espaço reservado da representação exibirá a representação gerada

Fazer alterações de código no código de trabalho enquanto a Ferramenta de desenvolvimento está em execução &quot;implantará as alterações com o sistema em operação&quot;. A &quot;implantação dinâmica&quot; leva vários segundos, portanto, permite que a implantação seja concluída antes de executar o trabalhador novamente na Ferramenta de desenvolvimento.

## Resolução de problemas

+ [Recuo YAML incorreto](../troubleshooting.md#incorrect-yaml-indentation)
+ [o limite memorySize está definido como muito baixo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [A Ferramenta de Desenvolvimento não pode ser iniciada devido à falta de private.key](../troubleshooting.md#missing-private-key)
+ [Lista suspensa de arquivos de origem incorreta](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parâmetro de consulta devToolToken ausente ou inválido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Não é possível remover arquivos de origem](../troubleshooting.md#unable-to-remove-source-files)
+ [Representação retornada parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
