---
title: Criação do seu primeiro serviço OSGi com o AEM Forms
description: Crie seu primeiro serviço OSGi com o AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
duration: 103
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---

# Serviço OSGi

Um serviço OSGi é uma classe Java ou interface de serviço, juntamente com várias propriedades de serviço como pares de nome/valor. As propriedades do serviço diferenciam entre diferentes provedores de serviços que fornecem serviços com a mesma interface de serviço.

Um serviço OSGi é definido semanticamente por sua interface de serviço e implementado como um objeto de serviço. A funcionalidade de um serviço é definida pelas interfaces que ele implementa. Dessa forma, aplicativos diferentes podem implementar o mesmo serviço. As interfaces de serviço permitem que os pacotes interajam por interfaces de vinculação, não por implementações. Uma interface de serviço deve ser especificada com o menor número de detalhes de implementação possível.

## Definir a interface

Uma interface simples com um método para mesclar dados com <span class="x x-first x-last">XDP</span> modelo.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Implementar a interface

Crie um novo pacote chamado `com.mysite.samples.impl` para controlar a implementação da interface.

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

A anotação `@Component(...)` Na linha 10, o marca essa classe Java como um componente OSGi, além de registrá-la como um serviço OSGi.

A variável `@Reference` A anotação faz parte dos serviços declarativos OSGi e é usada para inserir uma referência do [Serviço de saída](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) na variável `outputService`.


## Criar e implantar o pacote

* Abertura **janela da tela de comandos**
* Navegue até `c:\aemformsbundles\mysite\core`
* Executar o comando `mvn clean install -PautoInstallBundle`
* O comando acima criará e implantará automaticamente o pacote na instância do AEM em execução em localhost:4502

O pacote também estará disponível no seguinte local `C:\AEMFormsBundles\mysite\core\target`. O pacote também pode ser implantado no AEM usando o [Felix web console.](http://localhost:4502/system/console/bundles)

## Uso do serviço

Agora você pode usar o serviço em sua página JSP. O trecho de código a seguir mostra como obter acesso ao serviço e usar os métodos implementados pelo serviço

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

O pacote de exemplo que contém a página JSP pode ser [baixado aqui](assets/learning_aem_forms.zip)

[O pacote completo está disponível para download](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## Testar o pacote

Importe e instale o pacote no AEM usando o [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)

Use o carteiro para fazer uma chamada de POST e fornecer os parâmetros de entrada, conforme mostrado na captura de tela abaixo
![carteiro](assets/test-service-postman.JPG)

## Próximas etapas

[Criar Sling Servlet](./create-servlet.md)

