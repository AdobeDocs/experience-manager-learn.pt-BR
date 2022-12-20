---
title: Compilação XDP usando serviço montador
description: Uso do Assembler Service no AEM Forms para unir xdp
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# Compilação XDP usando serviço de montagem

Este artigo fornece os recursos para demonstrar a capacidade de compilar documentos xdp usando o serviço de montagem.
O código jsp a seguir foi gravado para inserir um subformulário chamado **endereço** do documento xdp chamado address.xdp em um ponto de inserção chamado **endereço** no documento principal.xdp. O xdp resultante foi salvo na pasta raiz da instalação AEM.

O serviço Assembler depende de documentos DDX válidos para descrever a manipulação de documentos PDF. Você pode fazer referência à variável [Documento de referência DDX aqui](assets/ddxRef.pdf).Page 40 tem informações sobre a compilação de xdp.

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

O arquivo DDX para inserir fragmentos em outro xdp está listado abaixo. O DDX insere o subformulário  **endereço** de address.xdp no ponto de inserção chamado **endereço** no principal.xdp. O documento resultante chamado **stitched.xdp** é salvo no sistema de arquivos.

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

* Baixar [Pacote de configuração XDP](assets/xdp-stitching.zip) ao seu sistema local.
* Faça upload e instale o pacote usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Extrair o conteúdo deste arquivo zip](assets/xdp-and-ddx.zip) para obter o arquivo de amostra xdp e DDX

**Depois de instalar o pacote, você terá que lista de permissões os seguintes URLs no Adobe Granite CSRF Filter.**

1. Siga os passos mencionados abaixo para lista de permissões os caminhos mencionados acima.
1. [Logon no configMgr](http://localhost:4502/system/console/configMgr)
1. Procure por Filtro CSRF do Adobe Granite
1. Adicione o seguinte caminho nas seções excluídas e salve `/content/AemFormsSamples/assemblerservice`
1. Pesquise por &quot;Filtro do referenciador do Sling&quot;
1. Marque a caixa de seleção &quot;Permitir vazio&quot;. (Essa configuração deve ser somente para fins de teste) Há várias maneiras de testar o código da amostra. O mais rápido e fácil é usar o aplicativo Postman. O Postman permite fazer solicitações do POST para o seu servidor. Instale o aplicativo Postman em seu sistema.
Inicie o aplicativo e insira o seguinte URL para testar a API de exportação de dados http://localhost:4502/content/AemFormsSamples/assemblerservice.html

Forneça os seguintes parâmetros de entrada, conforme especificado na captura de tela. Você pode usar a amostra de documentos baixados anteriormente,
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>Certifique-se de que a instalação do AEM Forms foi concluída. Todos os seus pacotes precisam estar no estado ativo.
