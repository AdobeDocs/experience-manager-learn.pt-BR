---
title: CRXDE Lite
description: 'O CRXDE Lite é uma ferramenta clássica, mas poderosa, para depurar AEM como ambientes Cloud Service Developer. O CRXDE Lite fornece um conjunto de funcionalidades que ajuda a depuração a inspecionar todos os recursos e propriedades, manipulando as partes mutáveis do JCR e investigando permissões. '
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
translation-type: tm+mt
source-git-commit: 10d0970c1b0debfec7cf94cc106bcae414fb55f6
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---


# Depuração de AEM como um Cloud Service com CRXDE Lite

O CRXDE Lite __APENAS__ está disponível em AEM como ambientes de desenvolvimento de Cloud Service (bem como o SDK AEM local).

## Acessar CRXDE Lite no autor de AEM

O CRXDE Lite __só__ pode ser acessado em AEM como ambientes de Desenvolvimento de Cloud Service e __não__ está disponível em ambientes de Palco ou Produção.

Para acessar o CRXDE Lite no autor de AEM:

1. Faça logon no AEM como um serviço de autor de AEM Cloud Service.
1. Navegue até Ferramentas > Geral > CRXDE Lite

Isso abrirá o CRXDE Lite usando as credenciais e permissões usadas para fazer logon no AEM Author.

## Depuração de conteúdo

O CRXDE Lite fornece acesso direto ao JCR. O conteúdo visível via CRXDE Lite é limitado pelas permissões concedidas ao usuário, o que significa que você pode não conseguir ver ou modificar tudo no JCR dependendo do seu acesso.

Observe que `/apps`, `/libs` e `/oak:index` são imutáveis, o que significa que não podem ser alterados por nenhum usuário no tempo de execução. Esses locais no JCR só podem ser modificados por meio de implantações de código.

+ A estrutura do JCR é navegada e manipulada usando o painel de navegação esquerdo
+ A seleção de um nó no painel de navegação esquerdo expõe os objetos de propriedade do nó no painel inferior.
   + As propriedades podem ser adicionadas, removidas ou alteradas do painel
+ Ao clicar no duplo em um nó de arquivo na navegação esquerda, o conteúdo do arquivo é aberto no painel superior direito
+ Toque no botão Salvar tudo, na parte superior esquerda, para persistir em alterar ou na seta para baixo, ao lado de Salvar tudo para reverter quaisquer alterações não salvas.

![CRXDE Lite - Depuração de conteúdo](./assets/crxde-lite/debugging-content.png)

Fazer alterações no conteúdo mutável em tempo de execução em AEM como ambientes de desenvolvimento de Cloud Service através do CRXDE Lite deve ser feito com cuidado.
Qualquer alteração feita diretamente no AEM via CRXDE Lite pode ser difícil de rastrear e governar. Conforme apropriado, verifique se as alterações feitas via CRXDE Lite retornam aos pacotes de conteúdo mutável (`ui.content`) do projeto AEM e se comprometeram com o Git, para garantir que o problema seja resolvido. Idealmente, todas as alterações no conteúdo do aplicativo são originadas da base de código e fluem para AEM por meio de implantações, em vez de fazer alterações diretamente no AEM via CRXDE Lite.

### Depuração de controles de acesso

O CRXDE Lite fornece uma maneira de testar e avaliar o controle de acesso em um nó específico para um usuário ou grupo específico (conhecido como principal).

Para acessar o console Testar Controle de acesso no CRXDE Lite, navegue até:

+ CRXDE Lite > Ferramentas > Testar Controle de acesso ...

![CRXDE Lite - Controle de acesso de teste](./assets/crxde-lite/permissions__test-access-control.png)

1. Usando o campo Caminho, selecione um Caminho JCR para avaliar
1. Usando o campo Principal, selecione o usuário ou grupo para avaliar o caminho
1. Toque no botão Test (Testar)

Os resultados são exibidos abaixo:

+ __Caminho__ reitera o caminho que foi avaliado
+ __O principal__ reitera o usuário ou grupo para o qual o caminho foi avaliado
+ __Os principais__ listas todos os principais dos quais o principal selecionado é parte.
   + Isso é útil para entender as associações de grupo transitivas que podem fornecer permissões por herança
+ __Privilégios no Caminho__ lista todas as permissões do JCR que o principal selecionado tem no caminho avaliado

### Atividades de depuração não suportadas

Veja a seguir atividades de depuração que __não__ podem ser executadas no CRXDE Lite.

### Depuração de configurações OSGi

As configurações de OSGi implantadas não podem ser revisadas via CRXDE Lite. As configurações de OSGi são mantidas no pacote de `ui.apps` códigos do AEM Project em `/apps/example/config.xxx`, no entanto, após a implantação para AEM como ambientes Cloud Service, os recursos de configurações de OSGi não são persistentes para o JCR, portanto não são visíveis via CRXDE Lite.

Em vez disso, use o Console do [desenvolvedor > Configurações](./developer-console.md#configurations) para analisar as configurações do OSGi implantadas.
