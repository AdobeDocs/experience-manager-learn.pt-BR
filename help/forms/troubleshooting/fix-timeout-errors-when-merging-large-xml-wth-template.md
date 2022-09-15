---
title: Correção de erros de tempo limite ao mesclar arquivos de dados xml grandes com o modelo xdp
description: Mesclar arquivos xml grandes com modelo no AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 5%

---

# Como habilitar a criação de arquivos pdf ao mesclar grandes arquivos de dados xml com modelos xdp

Ao mesclar arquivos de dados xml grandes com o modelo xdp, você pode ver o seguinte erro no arquivo de log

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Para corrigir o erro acima, faça o seguinte

## Alterar o tempo limite de bibliotecas

* Parar Servidor AEM
* Crie uma pasta chamada **instalar** na pasta crx-quickstart da instalação do AEM
* Crie um arquivo chamado **org.apache.aries.transaction.config** com o seguinte conteúdo aries.transaction.timeout=&quot;1200&quot; na pasta de instalação. Você pode alterar o valor do tempo limite de acordo com sua necessidade. O valor de tempo limite é em segundos

>[!NOTE]
> Depois de criar a configuração org.apache.aries.transaction , você pode editar os valores de tempo limite da transação da variável [configMgr](http://localhost:4502/system/console/configMgr) em vez de editar o arquivo


## Alterar as configurações do provedor Jacorb ORB

* [Abra o OSGi ConfigMgr](http://localhost:4502/system/console/configMgr)
* Procurar por **Provedor Jacorb ORB**
* Adicione a seguinte entrada jacorb.connection.client.pending_reply_timeout=600000 A configuração acima define o tempo limite de resposta pendente (também conhecido como tempo limite do cliente CORBA) como 600 segundos.
* Salve as alterações
