---
title: Criação do seu primeiro serviço OSGi com o AEM Forms
description: Crie seu primeiro serviço OSGi com a AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 2%

---

# Serviço OSGi

Um serviço OSGi é uma classe Java ou interface de serviço, juntamente com várias propriedades de serviço como pares de nome/valor. As propriedades do serviço se diferenciam entre diferentes provedores de serviços que oferecem serviços com a mesma interface de serviço.

Um serviço OSGi é definido semanticamente por sua interface de serviço e implementado como um objeto de serviço. A funcionalidade de um serviço é definida pelas interfaces que ele implementa. Dessa forma, aplicativos diferentes podem implementar o mesmo serviço. As interfaces de serviço permitem que os pacotes interajam por interfaces de vinculação, não por implementações. Uma interface de serviço deve ser especificada com o menor número possível de detalhes de implementação.

## Definir a interface

Uma interface simples com um método para unir dados à <span class="x x-first x-last">XDP</span> modelo .

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
	public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Implementar a interface

Crie um novo pacote chamado `com.mysite.samples.impl` para manter a implementação da interface.

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

A anotação `@Component(...)` na linha 10 marca essa classe Java como um Componente OSGi, bem como a registra como um Serviço OSGi.

O `@Reference` a anotação faz parte dos serviços declarativos OSGi e é usada para inserir uma referência da variável [Serviço de saída](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) na variável `outputService`.


## Criar e implantar o pacote

* Abrir **janela da tela de comandos**
* Vá até `c:\aemformsbundles\mysite\core`
* Execute o comando `mvn clean install -PautoInstallBundle`
* O comando acima criará e implantará automaticamente o pacote em sua instância de AEM em execução no localhost:4502

O pacote também estará disponível no seguinte local `C:\AEMFormsBundles\mysite\core\target`. O pacote também pode ser implantado em AEM usando o [Console da Web Felix.](http://localhost:4502/system/console/bundles)

## Uso do serviço

Agora você pode usar o serviço em sua página JSP. O trecho de código a seguir mostra como obter acesso ao seu serviço e usar os métodos implementados pelo serviço

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

O pacote de amostra que contém a página JSP pode ser [baixado aqui](assets/learning_aem_forms.zip)

[O pacote completo está disponível para download](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## Teste o pacote

Importe e instale o pacote no AEM usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)

Use o postman para fazer uma chamada de POST e fornecer os parâmetros de entrada, conforme mostrado na captura de tela abaixo
![carteiro](assets/test-service-postman.JPG)
