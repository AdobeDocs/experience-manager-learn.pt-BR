---
title: Ferramenta de desenvolvimento de assets compute
description: A Ferramenta de desenvolvimento de Assets compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset compute no Adobe I/O Runtime.
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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# Ferramenta de desenvolvimento de assets compute

A Ferramenta de desenvolvimento de Assets compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset compute no Adobe I/O Runtime.

## Executar a Ferramenta de desenvolvimento de Assets compute

A Ferramenta de desenvolvimento de Assets compute pode ser executada a partir da raiz do projeto Asset compute por meio do comando terminal:

```
$ aio app run
```

Isso iniciará a Ferramenta de desenvolvimento em __http://localhost:9000__ e automaticamente em uma janela do navegador. Para que a Ferramenta de desenvolvimento seja executada, [um devToolToken válido e gerado automaticamente deve ser fornecido por meio de um parâmetro de consulta](#troubleshooting__devtooltoken).

## Compreender a interface das Ferramentas de desenvolvimento do Asset compute{#interface}

![Ferramenta de desenvolvimento de assets compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Arquivo de origem:__ A seleção do arquivo de origem é usada para:
   + Selecionado o binário do ativo que atua como o `source` binário passado ao trabalhador do Asset compute
   + Fazer upload de arquivos de origem
1. __Definição de perfil(is) de asset compute:__ Define o trabalhador de Asset compute a ser executado, incluindo os parâmetros: incluindo o ponto final da URL do trabalhador, o nome da representação resultante e quaisquer parâmetros
1. __Executar:__ O botão Executar executa o perfil do Asset compute conforme definido no editor de perfil de configuração do Asset compute
1. __Anular:__ O botão Abort cancela uma execução iniciada ao tocar no botão Run
1. __Solicitação/Resposta:__ Fornece a solicitação HTTP e a resposta para/do trabalhador do Asset compute em execução no Adobe I/O Runtime. Isso pode ser útil para depuração
1. __Logs de ativação:__ Os logs que descrevem a execução do trabalhador do Asset compute, juntamente com quaisquer erros. Essas informações também estão disponíveis no `aio app run` saída padrão
1. __Representações:__ Exibe todas as representações geradas pela execução do trabalhador do Asset compute
1. __parâmetro de consulta devToolToken:__ O token da Ferramenta de desenvolvimento de Assets compute requer um `devToolToken` parâmetro de consulta a estar presente. Este token é gerado automaticamente sempre que uma nova Ferramenta de desenvolvimento é gerada

### Executar um trabalhador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through da execução de um trabalho de Asset compute na Ferramenta de desenvolvimento (sem áudio)_

1. Verifique se a Ferramenta de desenvolvimento de Asset compute foi iniciada a partir da raiz do projeto usando o `aio app run` comando.
1. Na Ferramenta de desenvolvimento do Asset compute, carregue ou selecione um [arquivo de imagem de exemplo](../assets/samples/sample-file.jpg)
   + Verifique se o arquivo está selecionado no __Arquivo de origem__ lista suspensa
1. Revise o __definição do perfil do Asset compute__ área de texto
   + A variável `worker` chave define o URL para o trabalhador de Asset compute implantado
   + A variável `name` define o nome da representação a ser gerada
   + Outros valores/chaves podem ser fornecidos neste objeto JSON e estão disponíveis no trabalhador sob o `rendition.instructions` objeto
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

1. Toque no __Executar__ botão
1. A variável __Seção Representações__ será preenchido com um espaço reservado de representação
1. Quando o trabalhador for concluído, o espaço reservado para representação exibirá a representação gerada

Fazer alterações de código no código do trabalhador enquanto a Ferramenta de desenvolvimento está em execução &quot;implantará&quot; as alterações. A &quot;implantação ativa&quot; leva vários segundos, portanto, permita que a implantação seja concluída antes de executar novamente o trabalho na Ferramenta de desenvolvimento.

## Resolução de problemas

+ [Recuo YAML incorreto](../troubleshooting.md#incorrect-yaml-indentation)
+ [O limite memorySize está definido como muito baixo](../troubleshooting.md#memorysize-limit-is-set-too-low)
+ [A Ferramenta de desenvolvimento não pode ser iniciada devido à falta de private.key](../troubleshooting.md#missing-private-key)
+ [Lista suspensa de arquivos de origem incorreta](../troubleshooting.md#source-files-dropdown-incorrect)
+ [Parâmetro de consulta devToolToken ausente ou inválido](../troubleshooting.md#missing-or-invalid-devtooltoken-query-parameter)
+ [Não foi possível remover os arquivos de origem](../troubleshooting.md#unable-to-remove-source-files)
+ [Representação retornada parcialmente desenhada/corrompida](../troubleshooting.md#rendition-returned-partially-drawn-or-corrupt)
