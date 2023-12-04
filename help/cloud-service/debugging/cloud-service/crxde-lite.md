---
title: CRXDE Lite
description: O CRXDE Lite é uma ferramenta clássica, mas poderosa, para depurar ambientes de desenvolvedor as a Cloud Service com AEM. O CRXDE Lite fornece um conjunto de funcionalidades que auxilia a depuração da inspeção de todos os recursos e propriedades, manipulação das partes mutáveis do JCR e investigação de permissões.
feature: Developer Tools
version: Cloud Service
doc-type: Tutorial
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
duration: 168
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Depuração do AEM as a Cloud Service com CRXDE Lite

CRXDE Lite é __SOMENTE__ disponível em ambientes de desenvolvimento as a Cloud Service AEM (bem como no SDK AEM local).

## Acesso ao CRXDE Lite no AEM Author

CRXDE Lite é __somente__ acessível em ambientes de desenvolvimento as a Cloud Service com AEM, e é __não__ disponível em ambientes de Preparo ou Produção.

Para acessar o CRXDE Lite no AEM Author:

1. Faça logon no serviço de Autor do AEM AEM as a Cloud Service.
1. Navegue até Ferramentas > Geral > CRXDE Lite

Isso abrirá o CRXDE Lite usando as credenciais e permissões usadas para fazer logon no AEM Author.

## Depuração de conteúdo

O CRXDE Lite fornece acesso direto ao JCR. O conteúdo visível via CRXDE Lite é limitado pelas permissões concedidas ao usuário, o que significa que você pode não conseguir ver ou modificar tudo no JCR, dependendo do seu acesso.

Observe que `/apps`, `/libs` e `/oak:index` são imutáveis, o que significa que não podem ser alteradas em tempo de execução por nenhum usuário. Esses locais no JCR só podem ser modificados por meio de implantações de código.

+ A estrutura JCR é navegada e manipulada usando o painel de navegação esquerdo
+ Selecionar um nó no painel de navegação esquerdo expõe as propriedades do nó no painel inferior.
   + As propriedades podem ser adicionadas, removidas ou alteradas no painel
+ Clicar duas vezes em um nó de arquivo na navegação à esquerda abre o conteúdo do arquivo no painel superior direito
+ Toque no botão Salvar tudo na parte superior esquerda para manter as alterações ou na seta para baixo ao lado de Salvar tudo para Reverter as alterações não salvas.

![CRXDE Lite - Depuração de conteúdo](./assets/crxde-lite/debugging-content.png)

Fazer alterações no conteúdo mutável no tempo de execução em ambientes de desenvolvimento AEM as a Cloud Service via CRXDE Lite deve ser feito com cuidado.
Quaisquer alterações feitas diretamente no AEM via CRXDE Lite podem ser difíceis de rastrear e administrar. Conforme apropriado, garantir que as alterações feitas por meio do CRXDE Lite retornem aos pacotes de conteúdo mutável do projeto AEM (`ui.content`) e comprometido com o Git, para garantir que o problema seja resolvido. Idealmente, todas as alterações no conteúdo do aplicativo se originam da base de código e fluem para o AEM por meio de implantações, em vez de fazer alterações diretamente no AEM por CRXDE Lite.

### Depuração de controles de acesso

O CRXDE Lite fornece uma maneira de testar e avaliar o controle de acesso em um nó específico para um usuário ou grupo específico (também conhecido como principal).

Para acessar o console Testar controle de acesso no CRXDE Lite, navegue até:

+ CRXDE Lite > Ferramentas > Testar controle de acesso ...

![CRXDE Lite - Testar o controle de acesso](./assets/crxde-lite/permissions__test-access-control.png)

1. Usando o campo Caminho, selecione um Caminho JCR para avaliar
1. Usando o campo Principal, selecione o usuário ou grupo para avaliar o caminho em relação a
1. Toque no botão Testar

Os resultados são exibidos abaixo:

+ __Caminho__ reitera o caminho que foi avaliado
+ __Principal__ reitera o usuário ou grupo cujo caminho foi avaliado
+ __Principais__ lista todos os principais dos quais o principal selecionado faz parte.
   + Isso é útil para entender as associações de grupo transitivo que podem fornecer permissões por herança
+ __Privilégios no caminho__ lista todas as permissões JCR que o principal selecionado tem no caminho avaliado

### Atividades de depuração não suportadas

A seguir estão atividades de depuração que podem __não__ ser executado em CRXDE Lite.

### Depuração de configurações OSGi

As configurações OSGi implantadas não podem ser revisadas via CRXDE Lite. As configurações de OSGi são mantidas nas configurações do projeto AEM `ui.apps` pacote de códigos em `/apps/example/config.xxx`No entanto, após a implantação em ambientes as a Cloud Service AEM, os recursos de configurações do OSGi não são mantidos para o JCR e, portanto, não são visíveis por CRXDE Lite.

Use o botão [Console do desenvolvedor > Configurações](./developer-console.md#configurations) para analisar configurações OSGi implantadas.
