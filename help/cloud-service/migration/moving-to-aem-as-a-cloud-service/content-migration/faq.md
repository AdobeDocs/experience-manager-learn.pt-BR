---
title: Perguntas frequentes sobre migração de conteúdo para o AEM as a Cloud Service
description: Obtenha respostas para perguntas frequentes sobre a migração de conteúdo para o AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
doc-type: article
topic: Migration
feature: Migration
role: Architect, Developer
level: Beginner
jira: KT-11200
thumbnail: kt-11200.jpg
exl-id: bdec6cb0-34a0-4a28-b580-4d8f6a249d01
duration: 399
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1884'
ht-degree: 0%

---

# Perguntas frequentes sobre migração de conteúdo para o AEM as a Cloud Service

Obtenha respostas para perguntas frequentes sobre a migração de conteúdo para o AEM as a Cloud Service.

## Terminologia

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html?lang=pt-BR)
+ **BPA**: [Analisador de práticas recomendadas](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html?lang=pt-BR)
+ **CTT**: [Ferramenta de Transferência de Conteúdo](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=pt-BR)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=pt-BR)
+ **IMS**: [Sistema Identity Management](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=pt-BR)
+ **DM**: [Mídia dinâmica](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html?lang=pt-BR)

Use o modelo abaixo para fornecer mais detalhes ao criar tíquetes de suporte da Adobe relacionados à CTT.

![Modelo de tíquete de suporte da Adobe para migração de conteúdo](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## Perguntas gerais sobre migração de conteúdo

### P: Quais são os diferentes métodos para migrar o conteúdo para o AEM as a Cloud Services?

Há três métodos diferentes disponíveis

+ Usar a ferramenta Transferência de conteúdo (AEM 6.3+ → AEMaaCS)
+ Por meio do Gerenciador de pacotes (AEM → AEMaaCS)
+ Serviço de importação em massa pronto para uso do Assets (S3/Azure → AEMaaCS)

### P: Há um limite para a quantidade de conteúdo que pode ser transferida usando a CTT?

Não. A CTT como uma ferramenta poderia extrair da origem do AEM e assimilar no AEMaaCS. No entanto, há limites específicos na plataforma AEMaaCS que devem ser considerados antes da migração.

Para obter mais informações, consulte [pré-requisitos de migração da nuvem](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html?lang=pt-BR).

### P: Tenho o relatório de BPA mais recente do meu sistema de origem. O que devo fazer com ele?

Exporte o relatório como CSV e carregue-o para o Cloud Acceleration Manager, [associado à sua Organização IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html?lang=pt-BR). Em seguida, passe pelo processo de revisão como [descrito na Fase de preparação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/cam-readiness-phase.html?lang=pt-BR).

Revise a avaliação de complexidade de código e conteúdo fornecida pela ferramenta e anote os itens de ação associados que levam ao acúmulo de refatoração de código ou à Avaliação de migração na nuvem.

### P: É recomendável extrair no autor de origem e assimilar no autor e na publicação do AEMaaCS?

É sempre recomendável executar uma extração e assimilação 1:1 entre os níveis de criação e publicação. Dito isso, é aceitável extrair o autor de produção de origem e assimilá-lo no Dev, Stage e Production CS.

### P: Há uma maneira de estimar o tempo que leva para migrar o conteúdo do AEM de origem para o AEMaaCS usando a CTT?

Como o processo de migração depende da largura de banda da Internet, do heap alocado para o processo CTT, da memória livre disponível e da E/S de disco que são subjetivos para cada sistema de origem, é recomendável executar as migrações de Prova de Antecedência e extrapolar esses pontos de dados para gerar estimativas.

### P: Como o desempenho do meu AEM de origem será afetado se eu iniciar o processo de extração da CTT?

A ferramenta CTT é executada em seu próprio processo Java™, que ocupa até um heap de 4 gb, que é configurável por meio da configuração OSGi. Esse número pode mudar, mas você pode pular para o processo Java™ e descobrir isso.

Se o AZCopy estiver instalado e/ou a opção de Pré-cópia/recurso de validação estiver ativado, o processo do AZCopy consumirá os ciclos do CPU.

Além do jvm , a ferramenta também usa E/S de disco para armazenar os dados em um espaço temporário de transição e que será limpo após o ciclo de extração. Além da RAM, CPU e Disk IO, a ferramenta CTT também usa a largura de banda da rede do sistema de origem para carregar dados no armazenamento de blobs do Azure.

A quantidade de recursos que o processo de extração da CTT utiliza depende do número de nós, do número de blobs e do seu tamanho agregado. É difícil fornecer uma fórmula e, portanto, é recomendável executar uma pequena Prova de migração para determinar os requisitos de upsize do servidor de origem.

Se os ambientes de clonagem forem usados para migração, eles não afetarão a utilização de recursos do servidor de produção em tempo real, mas terão seus próprios inconvenientes em relação à sincronização de conteúdo entre a produção em tempo real e o clone

### P: O que significam os termos &quot;limpar&quot; e &quot;substituir&quot; no contexto da CTT?

No contexto da [fase de extração](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=pt-BR#extraction-setup-phase), as opções são substituir os dados no contêiner de preparo de ciclos de extração anteriores ou adicionar o diferencial (adicionado/atualizado/excluído) a ele. O Contêiner de preparo não é nada, mas o contêiner de armazenamento de blob associado ao conjunto de migração. Cada conjunto de migração recebe seu próprio container de preparo.

No contexto da [fase de assimilação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/ingesting-content.html?lang=pt-BR), as opções são + para substituir todo o repositório de conteúdo do AEMaaCS ou sincronizar o conteúdo diferencial (adicionado/atualizado/excluído) do container de migração de preparo.

### P: Há vários sites, ativos associados, usuários, grupos no sistema de origem. É possível migrá-los em fases para o AEMaaCS?

Sim, é possível, mas exige um planejamento cuidadoso em relação a:

+ Criando os conjuntos de migração considerando os sites, os ativos ficam em suas respectivas hierarquias
   + Verifique se é aceitável migrar todos os ativos como parte de um conjunto de migração e depois trazer os locais que os estão usando em fases
+ No estado atual, o processo de assimilação do autor torna a instância do autor indisponível para criação de conteúdo, mesmo que o nível de publicação ainda possa servir o conteúdo
   + Isso significa que, até que a assimilação seja concluída no autor, as atividades de criação de conteúdo serão congeladas
+ Os usuários não são mais migrados, mas os grupos são.

Revise o processo de extração e assimilação complementar conforme documentado antes de planejar as migrações.

### P: Meus sites estarão disponíveis para os usuários finais mesmo que a assimilação esteja acontecendo nas instâncias de autor ou publicação do AEMaaCS?

Sim. O tráfego do usuário final não é interrompido pela atividade de migração de conteúdo. No entanto, a assimilação do autor congela a criação de conteúdo até que seja concluída.

### P: O relatório do BPA mostra itens relacionados a representações originais ausentes. Devo limpá-los na origem antes da extração?

Sim. A representação original ausente significa que o binário do ativo não é carregado corretamente em primeiro lugar. Considerá-los como dados ruins; revise, faça backup usando o Gerenciador de pacotes (conforme necessário) e remova-os do AEM de origem antes de executar a extração. Os dados incorretos terão resultados negativos nas etapas de processamento do ativo.

### P: O relatório do BPA tem itens relacionados ao nó `jcr:content` ausente para pastas. O que devo fazer com eles?

Quando `jcr:content` estiver ausente no nível da pasta, qualquer ação para propagar configurações, como perfis de processamento etc. dos pais vai quebrar neste nível. Revise o motivo da ausência de `jcr:content`. Mesmo que seja possível migrar essas pastas, observe que elas prejudicam a experiência do usuário e causam ciclos desnecessários de solução de problemas posteriormente.

### P: Criei um conjunto de migração. é possível verificar o tamanho dele?

Sim, há um recurso [Verificar Tamanho](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=pt-BR#migration-set-size) que faz parte da CTT.

### P: Estou executando a migração (extração, assimilação). É possível validar se todo o meu conteúdo extraído foi assimilado no target?

Sim, há um recurso de [validação](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html?lang=pt-BR) que faz parte da CTT.

### P: Meu cliente tem um requisito para mover o conteúdo entre ambientes do AEMaaCS, como de desenvolvimento do AEMaaCS para o AEMaaCS Stage ou para o AEMaaCS Prod. Posso usar a ferramenta de transferência de conteúdo para esses casos de uso?

Infelizmente, Não. O caso de uso da CTT é migrar o conteúdo da origem do AEM 6.3+ no local/hospedada no AMS para ambientes de nuvem do AEMaaCS. [Leia a documentação da CTT](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html?lang=pt-BR).

### P: Que tipo de problemas são previstos durante a extração?

A Fase de extração é um processo envolvido que requer vários aspectos para funcionar conforme esperado. Estar ciente dos diferentes tipos de problemas que podem ocorrer e como atenuá-los aumenta o sucesso geral da migração de conteúdo.

A documentação pública é continuamente aprimorada com base nos aprendizados, mas aqui estão algumas categorias de problemas de alto nível e possíveis motivos subjacentes.

![Problemas de extração de migração de conteúdo do AEM as a Cloud Service](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### P: Que tipo de problemas são previstos durante a assimilação?

A fase de assimilação ocorre completamente na plataforma de nuvem e requer ajuda dos recursos que têm acesso à infraestrutura do AEMaaCS. Crie um tíquete de suporte para obter mais ajuda.

Estas são as possíveis categorias de problemas (não considere isso como uma lista exclusiva)

![Problemas de assimilação de migração de conteúdo do AEM as a Cloud Service](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### P: Meu servidor de origem precisa ter uma conexão de saída com a Internet para que o CTT funcione?

A resposta curta é &quot;**Sim**&quot;.

O processo da CTT requer conectividade com os recursos abaixo:

+ O ambiente de destino do AEM as a Cloud Service: `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ O serviço de armazenamento de blobs do Azure: `casstorageprod.blob.core.windows.net`

Consulte a documentação para obter mais informações sobre a [conectividade de origem](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html?lang=pt-BR#source-environment-connectivity).

## Perguntas relacionadas ao processamento de ativos do Dynamic Media

### P: Os ativos serão reprocessados automaticamente após a assimilação no AEMaaCS?

Não. Para processar os ativos, a solicitação para reprocessar deve ser iniciada.

### P: Os ativos serão reindexados automaticamente após a assimilação no AEMaaCS?

Sim. Os ativos são reindexados com base nas definições de índice disponíveis no AEMaaCS.

### P: O AEM de origem tem uma integração com o Dynamic Media. Há alguma coisa específica que deve ser considerada antes da migração de conteúdo?

Sim, considere o seguinte quando o AEM de origem tiver a Integração do Dynamic Media.

+ O AEMaaCS é compatível somente com o modo Scene7 do Dynamic Media. Se o sistema de origem estiver no modo híbrido, será necessária a migração do DM para os modos do Scene7.
+ Se a abordagem for migrar das instâncias de clone de origem, é seguro desabilitar a integração DM no clone que seria usado para CTT. Essa etapa serve exclusivamente para evitar gravações no DM ou a carga no tráfego do DM.
+ Observe que a CTT migra nós, metadados de um conjunto de migração do AEM de origem para o AEMaaCS. Ele não executará operações diretamente no DM.

### P: Quais são as diferentes abordagens de migração quando a integração do DM está presente no Source AEM?

Leia a pergunta e a resposta acima antes de

(Estas são duas opções possíveis, mas não estão limitadas a apenas estas duas). Depende de como o cliente deseja abordar o UAT, do teste de desempenho, do ambiente disponível e se um clone está sendo usado para migração ou não. Considere esses dois como ponto de partida para discussão

**Opção 1**

Se o número de ativos/nós no ambiente de origem estiver na extremidade inferior (~100.000), supondo que eles possam ser migrados por um período de 24 + 72 horas, incluindo extração e assimilação, a melhor abordagem é

+ Realizar a migração diretamente da produção
+ Executar uma extração e assimilação iniciais no AEMaaCS com `wipe=true`
   + Esta etapa migra todos os nós e binários
+ Continuar trabalhando no local/autor de produção do AMS
+ A partir de agora, execute todas as outras provas dos ciclos de migração com o `wipe=true`
   + Observe que esta operação migra o armazenamento de nós completo, mas apenas os blobs modificados, em vez dos blobs inteiros. O conjunto anterior de blobs está lá no armazenamento de blob do Azure da instância de destino do AEMaaCS.
   + Use essa prova de migrações para medir a duração da migração, os testes e a validação de todas as outras funcionalidades
+ Por fim, antes da semana de ativação, execute uma migração wipe=true
   + Conectar o Dynamic Media no AEMaaCS
   + Desconectar configuração DM da origem local do AEM

Com essa opção, é possível executar a migração de um para um, ou seja, desenvolvimento no local → desenvolvimento do AEMaaCS e assim por diante. e mover as configurações do DM de seus respectivos ambientes

(Caso a migração esteja sendo planejada para ser executada a partir do clone)

**Opção 2**

+ Criar clone do autor de produção, remover configuração do DM do clone
+ Migrar clone no local → Desenvolvimento/preparo AEMaaCS
   + Conecte a empresa de DM de produção brevemente ao desenvolvimento/preparo do AEMaaCS para fins de validação
   + Durante a conexão do DM estar ativa, evite a assimilação de ativos no AEMaaCS
   + Isso permite validar validações específicas de CTT e DM
+ Quando o teste for concluído no AEMaaCS
   + Executar uma migração de limpeza do Palco local para o Palco do AEMaaCS

Execute uma migração de limpeza de Desenvolvimento local para Desenvolvimento do AEMaaCS.

A abordagem acima pode ser usada apenas para medir a duração da migração, mas requer uma limpeza posterior.

## Recursos adicionais

+ [Dicas e truques para migrar para o Experience Manager na nuvem ( Summit 2022)](https://business.adobe.com/br/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [Vídeo da Série de Especialistas da CTT](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html?lang=pt-BR)

+ [Vídeos da série Expert sobre outros tópicos do AEMaaCS](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/expert-resources/aem-experts-series.html?lang=pt-BR)
