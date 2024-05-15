---
title: Compilação XDP usando o serviço de montagem
description: Uso do Serviço de Assembler no AEM Forms para compilar xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 91
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---

# Costura XDP usando o serviço de montagem

Este artigo fornece os ativos para demonstrar a capacidade de compilar documentos xdp usando o serviço de montagem.
O código jsp a seguir foi gravado para inserir um subformulário chamado **endereço** de um documento xdp chamado address.xdp em um ponto de inserção chamado **endereço** no documento master.xdp. O xdp resultante foi salvo na pasta raiz da instalação do AEM.

O serviço do Assembler depende de documentos DDX válidos para descrever a manipulação de documentos PDF. Você pode consultar a [Documento de referência DDX aqui](assets/ddxRef.pdf).A página 40 tem informações sobre a compilação xdp.

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

O arquivo DDX para inserir fragmentos em outro xdp está listado abaixo. O DDX insere o subformulário  **endereço** de address.xdp no ponto de inserção chamado **endereço** no master.xdp. O documento resultante chamado **stitched.xdp** é salvo no sistema de arquivos.

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

Para que esse recurso funcione no servidor AEM

* Baixar [Pacote de compilação XDP](assets/xdp-stitching.zip) ao seu sistema local.
* Faça upload e instale o pacote usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Extrair o conteúdo deste arquivo zip](assets/xdp-and-ddx.zip) para obter os arquivos xdp e DDX de exemplo

**Depois de instalar o pacote, você terá que incluir na lista de permissões os seguintes URLs no Filtro CSRF do Adobe Granite.**

1. Siga as etapas mencionadas abaixo para incluir na lista de permissões os caminhos mencionados acima.
1. [Fazer logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Pesquisar por filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve `/content/AemFormsSamples/assemblerservice`
1. Procure por &quot;Sling Referrer Filter&quot;
1. Marque a caixa de seleção &quot;Permitir vazio&quot;. (Essa configuração deve ser somente para fins de teste) Há várias maneiras de testar o código de amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite que você faça solicitações do POST ao seu servidor. Instale o aplicativo Postman no sistema.
Inicie o aplicativo e insira o seguinte URL para testar a API de dados de exportação http://localhost:4502/content/AemFormsSamples/assemblerservice.html

Forneça os seguintes parâmetros de entrada, conforme especificado na captura de tela. Você pode usar os documentos de amostra baixados anteriormente,
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Verifique se a instalação do AEM Forms foi concluída. Todos os seus pacotes precisam estar no estado ativo.
>
