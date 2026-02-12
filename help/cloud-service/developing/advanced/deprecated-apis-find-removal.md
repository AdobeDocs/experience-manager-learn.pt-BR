---
title: Encontrar e remover APIs obsoletas no AEM as a Cloud Service
description: Saiba como localizar e remover APIs obsoletas no AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
exl-id: 287894ea-9cc1-4c27-ac7e-967ad46f4789
source-git-commit: effacd58725dab502f6fb6a4750646c1ea956de2
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 3%

---

# Encontrar e remover APIs obsoletas no AEM as a Cloud Service

Saiba como localizar e remover APIs obsoletas no AEM as a Cloud Service.

## Visão geral

O **Centro de Ações** da AEM as a Cloud Service notifica você sobre _APIs obsoletas_ em seu projeto. Para garantir que seu aplicativo esteja seguro e com desempenho e que você possa continuar implantando o código usando os pipelines do Cloud Manager, remova as APIs obsoletas do seu projeto.

Neste tutorial, você aprenderá a localizar e remover APIs obsoletas do ambiente do AEM as a Cloud Service usando o [Plug-in Maven do AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

## Como encontrar APIs obsoletas

Siga estas etapas para encontrar APIs obsoletas no projeto do AEM as a Cloud Service.

1. **Usar o plug-in Maven mais recente do AEM Analyzer**

   Em seu projeto do AEM, use a versão mais recente do [Plug-in Maven do AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

   - No `pom.xml` principal, a versão do plug-in geralmente é declarada. Compare sua versão com a mais recente [versão lançada](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin).

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - O plug-in verifica a AEM SDK mais recente disponível. Use a versão mais recente do AEM SDK no arquivo `pom.xml` do seu projeto. Ajuda a exibir as APIs obsoletas como avisos do IDE.

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - Verifique se o módulo `all` executa o plug-in na fase `verify`.

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **Executar uma compilação e verificar avisos**

   Quando você executa o `mvn clean install`, o analisador relata APIs obsoletas como **[AVISO]** mensagens na saída. Por exemplo:

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   É fácil ignorar essas mensagens ao se concentrar no sucesso ou na falha da build.

3. **Obter uma lista clara de APIs obsoletas**

   A etapa acima também fornece as mesmas informações. No entanto, execute a fase `verify` no módulo `all` para ver todas as mensagens de **[AVISO]** em um local. Por exemplo:

   ```shell
   $ mvn clean verify -pl all
   ```

   As mensagens **[AVISO]** na saída da compilação listam as APIs obsoletas em seu projeto.

## Como remover APIs obsoletas

O AEM Analyzer relata **o que** está obsoleto e fornece a **recomendação** sobre como corrigi-lo. No entanto, use a tabela abaixo para escolher a ação correta e siga a documentação vinculada quando precisar de mais detalhes.

### Estratégia de correção de API obsoleta

| Tipo de aviso do analisador | O que ele indica | Ação recomendada | Referência |
| --------------------- | ----------------- | ------------------ | --------- |
| API do AEM obsoleta | A API deve ser removida do AEM as a Cloud Service | Substituir o uso pela API pública compatível | [Diretrizes para remoção da API](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Pacote ou classe do AEM obsoleto | Não há mais suporte para pacote ou classe | Refatorar código para usar a alternativa recomendada | [APIs obsoletas](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| Biblioteca de terceiros obsoleta | A biblioteca não será compatível com SDKs futuros | Atualizar a dependência e o uso de refatoração | [Diretrizes gerais](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Padrões Sling/OSGi obsoletos | Anotações ou APIs herdadas detectadas | Migrar para APIs modernas do Sling e OSGi | [Remoção de padrões Sling/OSGi](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Remoção planejada (data futura) | A API ainda funciona, mas a remoção é imposta posteriormente | Agendar limpeza antes de aplicar o pipeline | [Notas de versão](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/release-notes/home) |

### Orientação prática

- Trate os avisos do analisador como **falhas futuras do pipeline**, não como mensagens opcionais.
- Corrija APIs obsoletas localmente usando a **última SDK do AEM**.
- Mantenha os resultados do analisador limpos para evitar problemas durante atualizações futuras do AEM.

A correção antecipada de APIs obsoletas mantém o projeto **seguro para atualização e pronto para implantação**.

## Recursos adicionais

- [Plug-in Maven do AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [Recursos e APIs obsoletos e removidos](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)
