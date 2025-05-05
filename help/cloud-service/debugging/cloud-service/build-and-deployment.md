---
title: Criar e implantar
description: O Adobe Cloud Manager facilita a criação de código e as implantações no AEM as a Cloud Service. Podem ocorrer falhas durante as etapas no processo de criação, exigindo uma ação para resolvê-las. Este guia aborda as falhas comuns no na implantação e a melhor maneira de abordá-las.
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
duration: 534
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 0%

---

# Depuração da build e de implantações do AEM as a Cloud Service

O Adobe Cloud Manager facilita a criação de código e as implantações no AEM as a Cloud Service. Podem ocorrer falhas durante as etapas no processo de criação, exigindo uma ação para resolvê-las. Este guia aborda as falhas comuns no na implantação e a melhor maneira de abordá-las.

![Pipeline de compilação de gerenciamento de nuvem](./assets/build-and-deployment/build-pipeline.png)

## Validação

A etapa de validação simplesmente garante a validade das configurações básicas do Cloud Manager. Falhas comuns de validação incluem:

### O ambiente está em um estado inválido

+ __Mensagem de erro:__ o ambiente está em um estado inválido.
  ![O ambiente está em um estado inválido](./assets/build-and-deployment/validation__invalid-state.png)
+ __Causa:__ o ambiente de destino do pipeline está em um estado de transição e, nesse momento, não pode aceitar novas compilações.
+ __Solução:__ aguarde até que o estado seja resolvido para um estado em execução (ou atualização disponível). Se o ambiente estiver sendo excluído, recrie-o ou escolha outro ambiente para o qual criar.

### Não foi possível encontrar o ambiente associado ao pipeline

+ __Mensagem de erro:__ o ambiente está marcado como excluído.
  ![O ambiente está marcado como excluído](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __Causa:__ O ambiente que o pipeline está configurado para usar foi excluído.
Mesmo que um novo ambiente com o mesmo nome seja recriado, o Cloud Manager não reassociará automaticamente o pipeline a esse ambiente com o mesmo nome.
+ __Solução:__ edite a configuração do pipeline e selecione novamente o ambiente no qual implantar.

### Não foi possível encontrar a ramificação Git associada ao pipeline

+ __Mensagem de erro:__ Pipeline inválido: XXXXXX. Motivo=Branch=xxxx não encontrado no repositório.
  ![Pipeline inválido: XXXXXX. Motivo=Branch=xxxx não encontrado no repositório](./assets/build-and-deployment/validation__branch-not-found.png)
+ __Causa:__ A ramificação Git que o pipeline está configurado para usar foi excluída.
+ __Solução:__ recrie a ramificação Git ausente usando exatamente o mesmo nome ou reconfigure o pipeline para compilar a partir de uma ramificação diferente existente.

## Teste de build e unidade

![Teste de Compilação e Unidade](./assets/build-and-deployment/build-and-unit-testing.png)

A fase de Teste de compilação e unidade executa uma compilação Maven (`mvn clean package`) do projeto retirado da ramificação Git configurada do pipeline.

Os erros identificados nesta fase devem ser reproduzíveis na criação do projeto localmente, com as seguintes exceções:

+ Uma dependência maven não disponível em [Maven Central](https://search.maven.org/) é usada, e o repositório Maven que contém a dependência é:
   + Inacessível do Cloud Manager, como um repositório Maven interno privado ou o repositório Maven requer autenticação e as credenciais incorretas foram fornecidas.
   + Não registrado explicitamente no `pom.xml` do projeto. Observe que a inclusão de repositórios Maven não é incentivada, pois aumenta os tempos de compilação.
+ Falha nos testes de unidade devido a problemas de tempo. Isso pode ocorrer quando os testes de unidade são sensíveis ao tempo. Um indicador forte depende de `.sleep(..)` no código de teste.
+ O uso de plug-ins Maven não compatíveis.

## Varredura de código

![Verificação de código](./assets/build-and-deployment/code-scanning.png)

A varredura de código executa análise de código estático usando uma combinação de práticas recomendadas específicas do Java e da AEM.

A varredura de código resulta em falha na build se houver vulnerabilidades críticas de segurança no código. Violações menores podem ser substituídas, mas é recomendável que sejam corrigidas. Observe que a verificação de código é imperfeita e pode resultar em [falsos positivos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html?lang=pt-BR#dealing-with-false-positives).

Para resolver problemas de verificação de código, baixe o relatório em formato CSV fornecido pela Cloud Manager por meio do botão **Detalhes do Download** e revise todas as entradas.

Para obter mais detalhes, consulte as regras específicas do AEM, consulte as [regras de verificação de código personalizadas específicas do AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html?lang=pt-BR) das documentações do Cloud Manager.

## Imagens de build

![Criar Imagens](./assets/build-and-deployment/build-images.png)

A imagem de build é responsável por combinar os artefatos de código criados na etapa de Teste de build e unidade com a versão do AEM, para formar um único artefato implantável.

Embora qualquer problema de compilação e criação de código seja encontrado durante o Teste de compilação e unidade, pode haver problemas estruturais ou de configuração identificados ao tentar combinar o artefato de compilação personalizado com a versão do AEM.

### Duplicar configurações de OSGi

Quando várias configurações de OSGi são resolvidas por meio do modo de execução para o ambiente AEM de destino, a etapa Criar imagem falha com o erro:

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### Causa 1

+ __Causa:__ todos os pacotes do projeto do AEM contêm vários pacotes de código, e a mesma configuração OSGi é fornecida por mais de um dos pacotes de código, resultando em um conflito, o que resulta na etapa Criar Imagem não poder decidir qual deve ser usado, desta forma falhando na compilação. Observe que isso não se aplica às configurações de fábrica OSGi, desde que tenham nomes exclusivos.
+ __Solução:__ revise todos os pacotes de código (incluindo todos os pacotes de código de terceiros incluídos) que estão sendo implantados como parte do aplicativo AEM, procurando configurações OSGi duplicadas que resolvem, via modo de execução, para o ambiente de destino. A orientação da mensagem de erro de &quot;definir o sinalizador mergeConfigurations como true&quot; não é possível no AEM as a Cloud Service e deve ser ignorada.

#### Causa 2

+ __Causa:__ o projeto do AEM inclui incorretamente o mesmo pacote de código duas vezes, resultando na duplicação de qualquer configuração OSGi contida nesse pacote.
+ __Solução:__ revise todos os pom.xmls de pacotes inseridos no projeto all e verifique se eles têm a `filevault-package-maven-plugin` [configuração](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=pt-BR#cloud-manager-target) definida como `<cloudManagerTarget>none</cloudManagerTarget>`.

### Script de repoinit malformado

Os scripts de repoinit definem o conteúdo básico, os usuários, as ACLs, etc. No AEM as a Cloud Service, os scripts de repoinit são aplicados durante a imagem da build, no entanto, na inicialização rápida local do AEM SDK eles são aplicados quando a configuração de fábrica de repoinit do OSGi é ativada. Por causa disso, os scripts de Repoinit podem falhar silenciosamente (com o registro) na inicialização rápida local do AEM SDK e, mas causar falha na etapa Criar imagem, interromper a implantação.

+ __Causa:__ um script de repoinit está malformado. Isso pode deixar o repositório em um estado incompleto, pois qualquer script de repoinit após o script com falha não é executado no repositório.
+ __Solução:__ revise o quickstart local do AEM SDK quando a configuração OSGi do script de repoinit for implantada para determinar se e quais são os erros.

### Dependência de conteúdo de repoinit não satisfeita

Os scripts de repoinit definem o conteúdo básico, os usuários, as ACLs, etc. Na inicialização rápida local do AEM SDK, os scripts de repoinit são aplicados quando a configuração de fábrica OSGi de repoinit é ativada ou, em outras palavras, depois que o repositório está ativo e pode ter incorrido em alterações de conteúdo diretamente ou por meio de pacotes de conteúdo. No AEM as a Cloud Service, os scripts de repoinit são aplicados durante a criação de imagem em um repositório que pode não conter conteúdo do qual o script de repoinit depende.

+ __Causa:__ um script de repoinit depende de um conteúdo que não existe.
+ __Solução:__ verifique se o conteúdo do qual o script repoinit depende existe. Geralmente, isso indica uma definição inadequada de scripts de repoinit que faltam diretivas que definem essas estruturas de conteúdo ausentes, mas necessárias. Isso pode ser reproduzido localmente excluindo o AEM, descompactando o Jar e adicionando a configuração OSGi de repoinit contendo o script de repoinit à pasta de instalação, e iniciando o AEM. O erro se apresentará no error.log do AEM SDK local quickstart.


### A versão dos Componentes principais do aplicativo é superior à versão implantada

_Esse problema afeta apenas ambientes não relacionados à produção que NÃO são atualizados automaticamente para a versão mais recente do AEM._

O AEM as a Cloud Service inclui automaticamente a versão mais recente dos Componentes principais em cada versão do AEM, ou seja, depois que um ambiente do AEM as a Cloud Service é atualizado, automática ou manualmente, ele recebe a versão mais recente dos Componentes principais implantada.

É possível para a etapa Criar imagem que falhará quando:

+ O aplicativo de implantação atualiza a versão de dependência do Maven dos Componentes principais no projeto `core` (pacote OSGi)
+ O aplicativo de implantação é então implantado em um ambiente de sandbox (não produção) do AEM as a Cloud Service que não foi atualizado para usar uma versão do AEM que contém essa nova versão dos Componentes principais.

Para evitar essa falha, sempre que uma Atualização do ambiente do AEM as a Cloud Service estiver disponível, inclua a atualização como parte da próxima build/implantação e sempre verifique se as atualizações estão incluídas após incrementar a versão dos Componentes principais na base de código do aplicativo.

+ __Sintomas:__
A etapa Criar Imagem falha com um ERRO informando que o(s) pacote(s) `com.adobe.cq.wcm.core.components...` em intervalos de versões específicos não puderam ser importados pelo projeto `core`.

  ```
  [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
  [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  ```

+ __Causa:__ o pacote OSGi do aplicativo (definido no projeto `core`) importa classes Java da dependência principal dos Componentes Principais, em um nível de versão diferente do que está implantado no AEM as a Cloud Service.
+ __Resolução:__
   + Usando o Git, reverta para uma confirmação de trabalho que existe antes do incremento de versão do Componente principal. Envie essa confirmação para uma ramificação Git do Cloud Manager e execute uma Atualização do ambiente a partir dessa ramificação. Isso atualizará o AEM as a Cloud Service para a versão mais recente do AEM, que incluirá a versão mais recente dos Componentes principais. Depois que o AEM as a Cloud Service for atualizado para a versão mais recente do AEM, que terá a versão mais recente dos Componentes principais, reimplante o código que originalmente estava falhando.
   + Para reproduzir esse problema localmente, verifique se a versão do AEM SDK é a mesma versão do AEM que o ambiente do AEM as a Cloud Service está usando.


### Criar um caso de suporte do Adobe

Se as abordagens de solução de problemas acima não resolverem o problema, crie um caso de suporte do Adobe, via:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Guia Suporte > Criar caso

  _Se você for membro de várias organizações da Adobe, verifique se a organização da Adobe que tem pipeline com falha está selecionada no alternador de organizações da Adobe antes de criar o caso._

## Implantar em

A etapa Implantar em é responsável por obter o artefato de código gerado na Imagem de compilação, iniciar os novos serviços de Autor e Publicação do AEM usando-o e, após o sucesso, remover todos os serviços antigos de Autor e Publicação do AEM. Os pacotes e índices de conteúdo mutável também são instalados e atualizados nesta etapa.

Familiarize-se com [logs do AEM as a Cloud Service](./logs.md) antes de depurar a etapa Implantar em. O log `aemerror` contém informações sobre a inicialização e o desligamento de pods que podem ser pertinentes à Implantação para problemas. Observe que o log disponível por meio do botão Baixar log na etapa Implantar em do Cloud Manager não é o log `aemerror` e não contém informações detalhadas relacionadas à inicialização dos aplicativos.

![Implantar em](./assets/build-and-deployment/deploy-to.png)

Os três principais motivos pelos quais a etapa Implantar em podem falhar:

### O pipeline do Cloud Manager contém uma versão antiga do AEM

+ __Causa:__ um pipeline do Cloud Manager contém uma versão do AEM mais antiga do que a implantada no ambiente de destino. Isso pode acontecer quando um pipeline é reutilizado e apontado para um novo ambiente que está executando uma versão posterior do AEM. Isso pode ser identificado ao verificar se a versão do AEM do ambiente é superior à versão do AEM do pipeline.
  ![O pipeline do Cloud Manager contém uma versão antiga do AEM](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __Resolução:__
   + Se o ambiente de destino tiver uma Atualização disponível, selecione Atualizar nas ações do ambiente e execute o build novamente.
   + Se o ambiente de destino não tiver uma Atualização disponível, significa que ele está executando a versão mais recente do AEM. Para resolver isso, exclua o pipeline e recrie-o.


### O Cloud Manager atinge o limite

O código em execução durante a inicialização do serviço AEM recém-implantado leva tanto tempo que o Cloud Manager atinge o tempo limite antes que a implantação possa ser concluída. Nesses casos, a implantação pode ser bem-sucedida, mesmo que o status do Cloud Manager seja considerado como Falha.

+ __Causa:__ o código personalizado pode executar operações, como consultas grandes ou passagens de conteúdo, acionadas antecipadamente no pacote OSGi ou ciclos de vida de Componente que atrasam significativamente o tempo de inicialização do AEM.
+ __Solução:__ revise a implementação do código que é executado antecipadamente no ciclo de vida do Pacote OSGi e revise os logs `aemerror` dos serviços de Autor e Publicação do AEM no momento da falha (tempo de log em GMT), conforme mostrado pela Cloud Manager, e procure mensagens de log indicando quaisquer processos de execução de log personalizados.

### Código ou configuração incompatível

A maioria das violações de código e configuração é capturada anteriormente na build. No entanto, é possível que o código ou a configuração personalizada seja incompatível com o AEM as a Cloud Service e não seja detectada até que seja executada no container.

+ __Causa:__ o código personalizado pode invocar operações demoradas, como consultas grandes ou percursos de conteúdo, acionadas antecipadamente no pacote OSGi ou ciclos de vida de Componente que atrasam significativamente o tempo de inicialização do AEM.
+ __Solução:__ revise os `aemerror` logs dos serviços de Autor e Publicação do AEM por volta da hora (hora do log em GMT) da falha, conforme mostrado pela Cloud Manager.
   1. Revise os logs para qualquer ERRO lançado pelas classes Java fornecidas pelo aplicativo personalizado. Se algum problema for encontrado, resolva, envie o código corrigido e recrie o pipeline.
   1. Revise os logs para qualquer ERRO relatado por aspectos do AEM que você está estendendo/interagindo com no aplicativo personalizado e investigue-os; esses ERROS podem não ser diretamente atribuídos a classes Java. Se algum problema for encontrado, resolva, envie o código corrigido e recrie o pipeline.

### Inclusão de /var no pacote de conteúdo

`/var` é mutável e contém uma variedade de conteúdos transitórios, de tempo de execução. Incluindo `/var` em pacotes de conteúdo (por exemplo, `ui.content`) implantado via Cloud Manager pode causar falha na etapa Implantar.

Esse problema é difícil de identificar, pois não resulta em falha na implantação inicial, somente em implantações subsequentes. Os sintomas perceptíveis incluem:

+ A implantação inicial é bem-sucedida, no entanto, o conteúdo mutável novo ou alterado que faz parte da implantação parece não existir no serviço de Publicação do AEM.
+ A ativação/desativação de conteúdo no AEM Author está bloqueada
+ As implantações subsequentes falham na etapa Implantar em, com a etapa Implantar em falhando após aproximadamente 60 minutos.

Para validar esse problema, é a causa do comportamento com falha:

1. Determinando que pelo menos um pacote de conteúdo faça parte da implantação, grava em `/var`.
1. Verifique se a fila de distribuição primária (em negrito) está bloqueada em:
   + Autor do AEM > Ferramentas > Implantação > Distribuição

     ![Fila de distribuição bloqueada](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. Ao falhar na implantação subsequente, baixe os logs &quot;Implantar em&quot; da Cloud Manager usando o botão Baixar log:

   ![Baixar implantação para logs](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ... e verifique se há aproximadamente 60 minutos entre as instruções de registro:

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... e ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   Observe que esse log não conterá esses indicadores nas implantações iniciais que relatam como bem-sucedido, apenas em implantações subsequentes com falha.

+ __Causa:__ o usuário do serviço de replicação da AEM usado para implantar pacotes de conteúdo no serviço de Publicação do AEM não pode gravar em `/var` no AEM Publish. Isso resulta na falha da implantação do pacote de conteúdo no serviço de Publicação do AEM.
+ __Solução:__ as seguintes maneiras de resolver esses problemas são listadas na ordem de preferência:
   1. Se os recursos `/var` não forem necessários, remova todos os recursos em `/var` dos pacotes de conteúdo que são implantados como parte do aplicativo.
   2. Se os recursos `/var` forem necessários, defina as estruturas de nó usando [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=pt-BR#repoinit). Os scripts de repoinit podem ser direcionados para o Autor do AEM, Publicação do AEM ou ambos, por meio dos modos de execução OSGi.
   3. Se os recursos `/var` forem necessários apenas no autor do AEM e não puderem ser modelados razoavelmente usando [repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=pt-BR#repoinit), mova-os para um pacote de conteúdo distinto, que só seja instalado no AEM Author por [incorporação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=pt-BR#embeddeds) no pacote `all` em uma pasta de modo de execução do AEM Author (`<target>/apps/example-packages/content/install.author</target>`).
   4. Forneça ACLs apropriadas ao usuário do serviço `sling-distribution-importer`, conforme descrito nesta [Base de Dados de Conhecimento do Adobe](https://helpx.adobe.com/in/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html).

### Criar um caso de suporte do Adobe

Se as abordagens de solução de problemas acima não resolverem o problema, crie um caso de suporte do Adobe, via:

+ [Adobe Admin Console](https://adminconsole.adobe.com) > Guia Suporte > Criar caso

  _Se você for membro de várias organizações da Adobe, verifique se a organização da Adobe que tem pipeline com falha está selecionada no alternador de organizações da Adobe antes de criar o caso._
