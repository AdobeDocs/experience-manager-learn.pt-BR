---
title: Ferramenta de desenvolvimento Asset Compute
description: A Ferramenta de desenvolvimento Asset Compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset Compute no Adobe I/O Runtime.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---


# Ferramenta de desenvolvimento Asset Compute

A Ferramenta de desenvolvimento Asset Compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset Compute no Adobe I/O Runtime.

## Executar a ferramenta de desenvolvimento Asset Compute

A Ferramenta de desenvolvimento Asset Compute pode ser executada a partir da raiz do projeto Asset Compute por meio do comando terminal:

```
$ aio app run
```

Isso iniciará a Ferramenta de desenvolvimento em __http://localhost:9000__ e a abrirá automaticamente em uma janela do navegador. Para que a Ferramenta de desenvolvimento seja executada, [um devToolToken válido gerado automaticamente deve ser fornecido por meio de um parâmetro de consulta](#troubleshooting__devtooltoken).

## Entender a interface das Ferramentas de desenvolvimento do Asset Compute{#interface}

![Ferramenta de desenvolvimento Asset Compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Arquivo de origem:__ A seleção do arquivo de origem é usada para:
   + Selecionado o binário de ativo que será o binário `source` passado para o trabalhador do Asset Compute
   + Upload de arquivos de origem
1. __Definição de perfil(s) do Asset Compute:__ Define o trabalhador do Asset Compute a ser executado, incluindo parâmetros: incluindo o ponto final do URL do trabalhador, o nome da representação resultante e quaisquer parâmetros
1. __Executar:__ O botão Executar executa o perfil do Asset Compute, conforme definido no editor de perfil de configuração do Asset Compute
1. __Abortar:__ o botão Abortar cancela uma execução iniciada a partir do toque no botão Executar
1. __Solicitação/resposta:__ fornece a solicitação e a resposta HTTP para/do trabalhador do Asset Compute em execução no Adobe I/O Runtime. Isso pode ser útil para depurar
1. __Logs de ativação:__ os logs que descrevem a execução do trabalhador do Asset Compute, juntamente com quaisquer erros. Essas informações também estão disponíveis no padrão `aio app run`
1. __Representações:__ exibe todas as representações geradas pela execução do trabalhador do Asset Compute
1. __parâmetro de consulta devToolToken:__ o token da Ferramenta de Desenvolvimento do Asset Compute requer a presença de um parâmetro de  `devToolToken` consulta válido. Esse token é gerado automaticamente sempre que uma nova Ferramenta de desenvolvimento é gerada

### Executar um trabalhador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through de executar um trabalho do Asset Compute na Ferramenta de Desenvolvimento (Sem áudio)_

1. Certifique-se de que a Ferramenta de desenvolvimento do Asset Compute foi iniciada a partir da raiz do projeto usando o comando `aio app run`.
1. Na Ferramenta de desenvolvimento Asset Compute, carregue ou selecione um [arquivo de imagem de amostra](../assets/samples/sample-file.jpg)
   + Verifique se o arquivo está selecionado na lista suspensa __Source file__
1. Revise a área de texto __Definição de perfil do Asset Compute__
   + A chave `worker` define o URL para o trabalhador implantado do Asset Compute
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
