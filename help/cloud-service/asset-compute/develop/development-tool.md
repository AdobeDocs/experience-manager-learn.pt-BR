---
title: Ferramenta de desenvolvimento de computação de ativos
description: A Asset Compute Development Tool é um recurso da Web local que permite aos desenvolvedores configurar e executar os funcionários do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset Compute no Adobe I/O Runtime.
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


# Ferramenta de desenvolvimento de computação de ativos

A Asset Compute Development Tool é um recurso da Web local que permite aos desenvolvedores configurar e executar os funcionários do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset Compute no Adobe I/O Runtime.

## Execute a ferramenta de desenvolvimento Asset Compute

A Ferramenta de desenvolvimento de computação de ativos pode ser executada a partir da raiz do projeto Computação de ativos por meio do comando terminal:

```
$ aio app run
```

Isso start a ferramenta de desenvolvimento em __http://localhost:9000__ e a abre automaticamente em uma janela do navegador. Para que a ferramenta de desenvolvimento seja executada, [um devToolToken válido gerado automaticamente deve ser fornecido por meio de um parâmetro](#troubleshooting__devtooltoken)de query.

## Entenda a interface das ferramentas de desenvolvimento da Asset Compute{#interface}

![Ferramenta de desenvolvimento de computação de ativos](./assets/development-tool/asset-compute-dev-tool.png)

1. __Arquivo de origem:__ A seleção do arquivo de origem é usada para:
   + Selecionado o binário de ativo que será o `source` binário passado para o funcionário de Computação de ativos
   + Carregar arquivos de origem
1. __Definição dos perfis de computação de ativos:__ Define o trabalhador do Asset Compute a ser executado incluindo parâmetros: incluindo o ponto final do URL do trabalhador, o nome da representação resultante e quaisquer parâmetros
1. __Executar:__ O botão Executar executa o perfil Asset Compute, conforme definido no editor do perfil de configuração Asset Compute
1. __Abortar:__ O botão Abortar cancela uma execução iniciada ao tocar no botão Executar
1. __Solicitação/resposta:__ Fornece a solicitação HTTP e a resposta para/do trabalhador Asset Compute em execução no Adobe I/O Runtime. Isso pode ser útil para depurar
1. __Logs de ativação:__ Os registros que descrevem a execução do trabalhador do Asset Compute, juntamente com quaisquer erros. Essas informações também estão disponíveis no `aio app run` padrão
1. __Representações:__ Exibe todas as representações geradas pela execução do trabalhador do Asset Compute
1. __parâmetro do query devToolToken:__ O token Asset Compute Development Tool requer que um parâmetro de `devToolToken` query válido esteja presente. Esse token é gerado automaticamente sempre que uma nova ferramenta de desenvolvimento é gerada

### Executar um trabalhador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through de executar um trabalho de Computação de ativos na Ferramenta de desenvolvimento (Sem áudio)_

1. Certifique-se de que a Ferramenta de desenvolvimento de computação de ativo seja iniciada da raiz do seu projeto usando o `aio app run` comando.
1. Na Ferramenta de desenvolvimento de computação de ativos, carregue ou selecione um arquivo de imagem de [amostra](../assets/samples/sample-file.jpg)
   + Verifique se o arquivo está selecionado na lista suspensa Arquivo ____ de origem
1. Revise a área de texto de definição __do perfil__ Asset Compute
   + A `worker` chave define o URL para o trabalhador implantado do Asset Compute
   + A `name` chave define o nome da representação a ser gerada
   + Outras chaves/valores podem ser fornecidos neste objeto JSON e estarão disponíveis no trabalhador sob o `rendition.instructions` objeto
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
1. A seção ____ Representações será preenchida com um espaço reservado para representação
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
