---
title: Use o painel Visão geral do sistema no AEM
description: Em versões anteriores dos administradores do AEM, era necessário consultar vários locais para obter uma imagem completa da instância do AEM. A Visão geral do sistema tem como objetivo resolver isso fornecendo uma visualização de alto nível da configuração, do hardware e da integridade da instância do AEM, tudo isso a partir de um único painel.
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Administração
role: Administrador
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 1%

---


# Usar o painel Visão geral do sistema

A [!UICONTROL Visão geral do sistema do Adobe Experience Manager] fornece uma visualização de alto nível da configuração, do hardware e da integridade da instância do AEM, tudo a partir de um único painel.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. A Visão geral do sistema pode ser acessada em: **Início do AEM** > **[!UICONTROL Ferramentas]** > **[!UICONTROL Operações]** > **[!UICONTROL Visão Geral do Sistema]**

   Diretamente em **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. As informações de [!UICONTROL Visão geral do sistema] podem ser exportadas clicando no botão [!UICONTROL Download]. As informações também são expostas por meio do seguinte terminal [!DNL REST]:
1. Abaixo está uma amostra de saída do JSON que é exportada do [!UICONTROL Visão geral do sistema]:

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
