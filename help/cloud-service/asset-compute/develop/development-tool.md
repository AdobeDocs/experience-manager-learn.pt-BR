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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '535'
ht-degree: 0%

---

# Ferramenta de desenvolvimento de assets compute

A Ferramenta de desenvolvimento de Asset compute é um recurso da Web local que permite aos desenvolvedores configurar e executar os trabalhadores do Asset Computer localmente, fora do contexto do SDK do AEM em relação aos recursos do Asset compute no Adobe I/O Runtime.

## Executar a ferramenta de desenvolvimento de Assets compute

A Ferramenta de desenvolvimento de Assets compute pode ser executada a partir da raiz do projeto Asset compute por meio do comando terminal :

```
$ aio app run
```

Isso iniciará a Ferramenta de desenvolvimento em __http://localhost:9000__ e abra-a automaticamente em uma janela do navegador. Para que a ferramenta de desenvolvimento seja executada, [um devToolToken válido gerado automaticamente deve ser fornecido por meio de um parâmetro de consulta](#troubleshooting__devtooltoken).

## Entender a interface das Ferramentas de desenvolvimento do Asset compute{#interface}

![Ferramenta de desenvolvimento de assets compute](./assets/development-tool/asset-compute-dev-tool.png)

1. __Arquivo de origem:__ A seleção do arquivo de origem é usada para:
   + Selecionado o binário de ativo que atua como o `source` binário passado para o trabalhador do Asset compute
   + Upload de arquivos de origem
1. __Definição do(s) perfil(s) de asset compute:__ Define o trabalhador do Asset compute a ser executado, incluindo parâmetros: incluindo o ponto final do URL do trabalhador, o nome da representação resultante e quaisquer parâmetros
1. __Executar:__ O botão Executar executa o perfil do Asset compute, conforme definido no editor de perfil de configuração do Asset compute
1. __Abortar:__ O botão Abortar cancela uma execução iniciada a partir do toque no botão Executar
1. __Solicitação/Resposta:__ Fornece a solicitação HTTP e a resposta para/do trabalhador do Asset compute em execução no Adobe I/O Runtime. Isso pode ser útil para depurar
1. __Logs de ativação:__ Os registros que descrevem a execução do trabalhador do Asset compute, juntamente com quaisquer erros. Essas informações também estão disponíveis na seção `aio app run` padrão
1. __Representações:__ Exibe todas as representações geradas pela execução do Asset compute
1. __parâmetro de consulta devToolToken:__ O token da Ferramenta de Desenvolvimento de Assets compute requer um `devToolToken` parâmetro de consulta a estar presente. Esse token é gerado automaticamente sempre que uma nova Ferramenta de desenvolvimento é gerada

### Executar um trabalhador personalizado

>[!VIDEO](https://video.tv.adobe.com/v/40241?quality=12&learn=on)

_Click-through da execução de um trabalho de Asset compute na Ferramenta de desenvolvimento (Sem áudio)_

1. Certifique-se de que a Ferramenta de desenvolvimento de Assets compute seja iniciada a partir da raiz do projeto usando o `aio app run` comando.
1. Na Ferramenta de desenvolvimento de Assets compute, faça upload ou selecione um [arquivo de imagem de exemplo](../assets/samples/sample-file.jpg)
   + Verifique se o arquivo está selecionado na __Arquivo de origem__ lista suspensa
1. Revise o __Definição de perfil de asset compute__ área de texto
   + O `worker` chave define o URL para o trabalhador do Asset compute implantado
   + O `name` chave define o nome da representação a ser gerada
   + Outros valores/chaves podem ser fornecidos neste objeto JSON e estão disponíveis no trabalhador sob a função `rendition.instructions` objeto
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
1. O __Seção Representações__ será preenchido com um espaço reservado de representação
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
