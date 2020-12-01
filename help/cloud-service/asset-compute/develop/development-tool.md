---
title: Ferramenta de desenvolvimento de asset computes
description: A Ferramenta de desenvolvimento de Asset computes é um recurso da Web local que permite aos desenvolvedores configurar e executar os funcionários do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset compute no Adobe I/O Runtime.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---


# Ferramenta de desenvolvimento de asset computes

A Ferramenta de desenvolvimento de Asset computes é um recurso da Web local que permite aos desenvolvedores configurar e executar os funcionários do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset compute no Adobe I/O Runtime.

## Execute a ferramenta de desenvolvimento de Asset computes

A Ferramenta de desenvolvimento de Asset compute pode ser executada a partir da raiz do projeto de Asset compute por meio do comando terminal:

```
$ aio app run
```

Isso start a ferramenta de desenvolvimento em __http://localhost:9000__ e a abre automaticamente em uma janela do navegador. Para que a ferramenta de desenvolvimento seja executada, [um devToolToken válido gerado automaticamente deve ser fornecido por meio de um parâmetro de query](#troubleshooting__devtooltoken).

## Entenda a interface das Ferramentas de Desenvolvimento de Asset computes{#interface}

![Ferramenta de desenvolvimento de asset computes](./assets/development-tool/asset-compute-dev-tool.png)

1. __Arquivo de origem:__ A seleção do arquivo de origem é usada para:
   + Selecionado o binário de ativo que será o binário `source` passado para o trabalhador do Asset compute
   + Carregar arquivos de origem
1. __Definição de perfil(s) de asset compute:__ Define o Asset compute a ser executado, incluindo parâmetros: incluindo o ponto final do URL do trabalhador, o nome da representação resultante e quaisquer parâmetros
1. __Executar:__ O botão Executar executa o perfil do Asset compute, conforme definido no editor do perfil de configuração do Asset compute
1. __Abortar:__ o botão Abortar cancela uma execução iniciada ao tocar no botão Executar
1. __Solicitação/resposta:__ fornece a solicitação HTTP e a resposta para/do trabalhador do Asset compute em execução no Adobe I/O Runtime. Isso pode ser útil para depurar
1. __Logs de ativação:__ os registros que descrevem a execução do funcionário do Asset compute, juntamente com quaisquer erros. Essas informações também estão disponíveis no padrão `aio app run`
1. __Representações:__ Exibe todas as execuções geradas pela execução do trabalhador do Asset compute
1. __parâmetro de query devToolToken:__ O token da Ferramenta de Desenvolvimento de Asset computes requer que um parâmetro de  `devToolToken` query válido esteja presente. Esse token é gerado automaticamente sempre que uma nova ferramenta de desenvolvimento é gerada

### Executar um trabalhador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through de execução de um trabalho de Asset compute na Ferramenta de Desenvolvimento (Sem áudio)_

1. Certifique-se de que a Ferramenta de Desenvolvimento de Asset computes seja iniciada a partir da raiz do seu projeto usando o comando `aio app run`.
1. Na Ferramenta de desenvolvimento de Asset computes, carregue ou selecione um [arquivo de imagem de amostra](../assets/samples/sample-file.jpg)
   + Verifique se o arquivo está selecionado na lista suspensa __Arquivo de origem__
1. Revise a área de texto __definição do perfil do Asset compute__
   + A chave `worker` define o URL para o trabalhador de Asset compute implantado
   + A chave `name` define o nome da representação a ser gerada
   + Outras chaves/valores podem ser fornecidos neste objeto JSON e estarão disponíveis no trabalhador sob o objeto `rendition.instructions`
      + Como opção, adicione valores para `size`, `contrast` e `brightness`:

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

Fazer alterações de código no código de trabalho enquanto a Ferramenta de Desenvolvimento estiver em execução &quot;implantará&quot; as alterações em tempo real. A &quot;implantação ativa&quot; leva vários segundos, portanto, permita que a implantação seja concluída antes de reexecutar o funcionário da ferramenta de desenvolvimento.

## Resolução de problemas

+ [Recuo YAML incorreto](../troubleshooting.md#incorrect-yaml-indentation)
+ [o limite memorySize está definido como muito baixo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [A Ferramenta de Desenvolvimento não pode start devido à falta de private.key](../troubleshooting.md#missing-private-key)
+ [Menu suspenso de arquivos de origem incorreto](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parâmetro de query devToolToken ausente ou inválido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Não é possível remover arquivos de origem](../troubleshooting.md#unable-to-remove-source-files)
+ [Representação devolvida parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
