---
title: Ferramenta de desenvolvimento de assets compute
description: A Ferramenta de desenvolvimento de Assets compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset Compute no Adobe I/O Runtime.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6283
thumbnail: 40241.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: cbe08570-e353-4daf-94d1-a91a8d63406d
duration: 171
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Ferramenta de desenvolvimento de assets compute

A Ferramenta de desenvolvimento de Assets compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset Compute no Adobe I/O Runtime.

## Executar a Ferramenta de desenvolvimento de Assets compute

A Ferramenta de desenvolvimento de Assets compute pode ser executada a partir da raiz do projeto Asset Compute por meio do comando terminal:

```
$ aio app run
```

Isso iniciará a Ferramenta de Desenvolvimento em __http://localhost:9000__ e a abrirá automaticamente em uma janela do navegador. Para que a Ferramenta de Desenvolvimento seja executada, [um devToolToken válido e gerado automaticamente deve ser fornecido por meio de um parâmetro de consulta](#troubleshooting__devtooltoken).

## Compreender a interface das Ferramentas de desenvolvimento do Asset Compute{#interface}

![Ferramenta de desenvolvimento do Asset Compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Arquivo Source:__ A seleção do arquivo de origem é usada para:
   + Selecionado o binário do ativo que atua como o binário `source` passado para o trabalhador do Asset compute
   + Fazer upload de arquivos de origem
1. __Definição de perfil(is) de Asset compute:__ Define o trabalhador de Asset compute a ser executado, incluindo parâmetros: incluindo o ponto de extremidade da URL do trabalhador, o nome de representação resultante e quaisquer parâmetros
1. __Executar:__ O botão Executar executa o perfil do Asset Compute conforme definido no editor de perfil de configuração do Asset Compute
1. __Anular:__ o botão Anular cancela uma execução iniciada ao tocar no botão Executar
1. __Solicitação/Resposta:__ fornece a solicitação HTTP e a resposta para/do trabalhador do Asset Compute em execução no Adobe I/O Runtime. Isso pode ser útil para depuração
1. __Logs de Ativação:__ os logs que descrevem a execução do trabalhador do Asset compute, juntamente com os erros. Essas informações também estão disponíveis na `aio app run` padrão
1. __Representações:__ exibe todas as representações geradas pela execução do trabalhador do Asset compute
1. __parâmetro de consulta devToolToken:__ o token da Ferramenta de Desenvolvimento de Assets compute requer que um parâmetro de consulta `devToolToken` válido esteja presente. Este token é gerado automaticamente sempre que uma nova Ferramenta de desenvolvimento é gerada

### Executar um trabalhador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through de execução de um trabalho de Asset compute na Ferramenta de desenvolvimento (Sem áudio)_

1. Verifique se a Ferramenta de Desenvolvimento de Asset compute foi iniciada a partir da raiz do projeto usando o comando `aio app run`.
1. Na Ferramenta de Desenvolvimento de Assets compute, carregue ou selecione um [arquivo de imagem de exemplo](../assets/samples/sample-file.jpg)
   + Verifique se o arquivo está selecionado na lista suspensa __Arquivo Source__
1. Revise a área de texto __definição de perfil do Asset Compute__
   + A chave `worker` define a URL para o trabalhador do Asset compute implantado
   + A chave `name` define o nome da representação a ser gerada
   + Outros valores/chaves podem ser fornecidos neste objeto JSON e estão disponíveis no trabalho sob o objeto `rendition.instructions`
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
1. A __seção Representações__ será preenchida com um espaço reservado para representação
1. Quando o trabalhador for concluído, o espaço reservado para representação exibirá a representação gerada

Fazer alterações de código no código do trabalhador enquanto a Ferramenta de desenvolvimento está em execução &quot;implantará&quot; as alterações. A &quot;implantação ativa&quot; leva vários segundos, portanto, permita que a implantação seja concluída antes de executar novamente o trabalho na Ferramenta de desenvolvimento.

## Resolução de problemas

+ [Recuo YAML incorreto](../troubleshooting.md#incorrect-yaml-indentation)
+ [O limite memorySize está definido como muito baixo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [A Ferramenta de desenvolvimento não pode ser iniciada devido à falta de private.key](../troubleshooting.md#missing-private-key)
+ [Lista suspensa de arquivos do Source incorreta](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parâmetro de consulta devToolToken ausente ou inválido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Não foi possível remover os arquivos de origem](../troubleshooting.md#unable-to-remove-source-files)
+ [Representação retornada parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
