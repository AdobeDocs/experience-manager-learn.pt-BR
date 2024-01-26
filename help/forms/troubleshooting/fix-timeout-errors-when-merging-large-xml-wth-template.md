---
title: Corrigir erros de tempo limite ao mesclar arquivos de dados xml grandes com o modelo xdp
description: Mesclar arquivos xml grandes com o modelo no AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
jira: KT-11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
duration: 45
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '178'
ht-degree: 0%

---

# Como habilitar a criação de arquivos pdf mesclando arquivos de dados xml grandes com modelos xdp

Ao mesclar arquivos de dados xml grandes com o modelo xdp, você pode ver o seguinte erro no arquivo de log

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Para corrigir o erro acima, faça o seguinte

## Alterar o tempo limite de variáveis

* Parar servidor AEM
* Crie uma pasta chamada **instalar** na pasta crx-quickstart da instalação do AEM
* Crie um arquivo chamado **org.apache.aries.transaction.config** com o seguinte conteúdo aries.transaction.timeout=&quot;1200&quot; na pasta de instalação. Você pode alterar o valor do tempo limite de acordo com sua necessidade. O valor de tempo limite está em segundos

>[!NOTE]
> Depois de criar a configuração org.apache.aries.transaction, é possível editar os valores de timeout de transação na [configMgr](http://localhost:4502/system/console/configMgr) em vez de editar o arquivo


## Alterar as configurações do provedor Jacorb ORB

* [Abra o OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* Pesquisar por **Provedor ORB Jacorb**
* Adicione a seguinte entrada jacorb.connection.client.pending_reply_timeout=600000 A configuração acima define o tempo limite de resposta pendente (também conhecido como, tempo limite do cliente CORBA) como 600 segundos.
* Salve as alterações
