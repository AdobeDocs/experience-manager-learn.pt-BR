---
title: CRXDE Lite
description: O CRXDE Lite é uma ferramenta clássica, mas poderosa, para depurar AEM as a Cloud Service Developer environment. O CRXDE Lite fornece um conjunto de funcionalidades que auxilia a depuração a inspecionar todos os recursos e propriedades, manipular as partes mutáveis do JCR e investigar permissões.
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: KT-5481
thumbnail: kt-5481.jpg
topic: Development
role: Developer
level: Beginner
exl-id: f3f2c89f-6ec1-49d3-91c7-10a42b897780
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 0%

---

# Depuração de AEM como um Cloud Service com CRXDE Lite

O CRXDE Lite é __SOMENTE__ disponível no AEM como um ambiente de desenvolvimento de Cloud Service (bem como o SDK AEM local).

## Acessar o CRXDE Lite no autor do AEM

O CRXDE Lite é __acessível apenas__ em AEM como ambientes de desenvolvimento de Cloud Service e __não__ está disponível em ambientes de Preparo ou Produção.

Para acessar o CRXDE Lite no AEM Author:

1. Faça logon no AEM como um serviço de Autor do AEM Cloud Service.
1. Navegue até Ferramentas > Geral > CRXDE Lite

Isso abrirá o CRXDE Lite usando as credenciais e as permissões usadas para fazer logon no AEM Author.

## Depuração de conteúdo

O CRXDE Lite fornece acesso direto ao JCR. O conteúdo visível por meio do CRXDE Lite é limitado pelas permissões concedidas ao usuário, o que significa que você não pode ver ou modificar tudo no JCR dependendo do seu acesso.

Observe que `/apps`, `/libs` e `/oak:index` são imutáveis, o que significa que não podem ser alteradas no tempo de execução por qualquer usuário. Essas localizações no JCR só podem ser modificadas por meio de implantações de código.

+ A estrutura do JCR é navegada e manipulada usando o painel de navegação esquerdo
+ Selecionar um nó no painel de navegação esquerdo expõe as propriedades do nó no painel inferior.
   + As propriedades podem ser adicionadas, removidas ou alteradas do painel
+ Clicar duas vezes em um nó de arquivo na navegação à esquerda abre o conteúdo do arquivo no painel superior direito
+ Toque no botão Salvar tudo, na parte superior esquerda, para continuar alterado, ou na seta para baixo, ao lado de Salvar tudo, para reverter quaisquer alterações não salvas.

![CRXDE Lite - Depuração de conteúdo](./assets/crxde-lite/debugging-content.png)

Fazer alterações no conteúdo mutável em tempo de execução no AEM como ambientes de desenvolvimento de Cloud Service por meio do CRXDE Lite deve ser feito com cuidado.
Qualquer alteração feita diretamente no AEM via CRXDE Lite pode ser difícil de rastrear e governar. Conforme apropriado, assegure-se de que as alterações feitas via CRXDE Lite retornem aos pacotes de conteúdo mutável do projeto AEM (`ui.content`) e sejam comprometidas com o Git, para garantir que o problema seja resolvido. Idealmente, todas as alterações de conteúdo do aplicativo são originadas da base de código e fluem para o AEM por meio de implantações, em vez de fazer alterações diretamente no AEM via CRXDE Lite.

### Depuração de controles de acesso

O CRXDE Lite fornece uma maneira de testar e avaliar o controle de acesso em um nó específico de um usuário ou grupo específico (também conhecido como principal).

Para acessar o console Controle de acesso de teste no CRXDE Lite, navegue até:

+ CRXDE Lite > Ferramentas > Testar controle de acesso ...

![CRXDE Lite - Testar controle de acesso](./assets/crxde-lite/permissions__test-access-control.png)

1. Usando o campo Caminho , selecione um Caminho JCR para avaliar
1. Usando o campo Principal, selecione o usuário ou grupo para avaliar o caminho
1. Toque no botão Test

Os resultados são exibidos abaixo:

+ ____ Caminha o caminho que foi avaliado
+ ____ Principalmente reitera o usuário ou grupo para o qual o caminho foi avaliado
+ ____ Os principais listam todos os principais dos quais o principal selecionado faz parte.
   + Isso é útil para entender as associações de grupo transitivas que podem fornecer permissões por herança
+ __Privilégios em__ Caminhos lista todas as permissões do JCR que o principal selecionado tem no caminho avaliado

### Atividades de depuração não suportadas

A seguir estão as atividades de depuração que podem __not__ ser executadas no CRXDE Lite.

### Depuração das configurações do OSGi

As configurações de OSGi implantadas não podem ser revisadas via CRXDE Lite. As configurações OSGi são mantidas no pacote de código `ui.apps` do AEM Project em `/apps/example/config.xxx`. No entanto, após a implantação no AEM como um ambiente Cloud Service, os recursos de configurações OSGi não são persistentes para o JCR, portanto, não são visíveis via CRXDE Lite.

Em vez disso, use o [Console do Desenvolvedor > Configurações](./developer-console.md#configurations) para revisar as configurações do OSGi implantadas.
