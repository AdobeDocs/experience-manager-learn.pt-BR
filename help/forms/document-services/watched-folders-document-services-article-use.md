---
title: Uso de pastas vigiadas no AEM Forms
seo-title: Uso de pastas vigiadas no AEM Forms
description: Configurar e usar pastas monitoradas no AEM Forms
seo-description: Configurar e usar pastas monitoradas no AEM Forms
uuid: 32c4bda2-363d-4294-925e-405a176f7f8d
feature: output-service
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: a40e2381-0dc8-4784-9b80-15e27b244035
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---


# Uso de pastas vigiadas no AEM Forms{#using-watched-folders-in-aem-forms}

Um administrador pode configurar uma pasta de rede, conhecida como Pasta assistida, para que quando um usuário coloca um arquivo (como um arquivo PDF) na Pasta assistida, uma operação de fluxo de trabalho, serviço ou script pré-configurado seja iniciada para processar o arquivo adicionado. Depois que o serviço executa a operação especificada, ele salva o arquivo de resultado em uma pasta de saída especificada. Para obter mais informações sobre fluxo de trabalho, serviço e script.

Para saber mais sobre como criar uma pasta assistida, [clique aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Pastas monitoradas são usadas para gerar documentos no modo de lote. Usando o mecanismo de pasta monitorada, você pode gerar o Interative Communications for the print canal, ou usar o serviço de saída para unir dados ao modelo.

Este artigo abordará o caso de uso da união de dados com um modelo usando o serviço de saída via mecanismo de pasta monitorada.

O serviço de saída é um serviço OSGi que faz parte AEM Documento Services. O serviço de saída suporta vários formatos de saída e recursos de design de saída do AEM Forms Designer. O serviço de saída pode converter modelos XFA e dados XML para gerar documentos de impressão em vários formatos.

Para saber mais sobre o serviço de saída, [clique aqui](https://helpx.adobe.com/aem-forms/6/output-service.html).
Para configurar a pasta monitorada no sistema, siga as etapas abaixo:
* [Baixe e extraia o conteúdo do arquivo](assets/outputservicewatchedfolderkt.zip)zip. Este arquivo zip contém o pacote para criação de pastas monitoradas e arquivos de amostra para testar o serviço de saída usando o mecanismo de pasta monitorada
   * Sistema Windows

      * Importe o outputservicewatchedfolder.zip para AEM usando o gerenciador de pacote
      * Isso criará uma pasta assistida chamada outputservicewatchedfolder na unidade C.
   * Sistema Windows
      * [Abra a configuração da pasta assistida](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Defina a propriedade de caminho de pasta do nó de serviço externo para apontar para um local adequado
      * Salvar suas alterações
      * O local mencionado acima será sua pasta monitorada.

Solte as pastas SamplePdfFile e SampleXdpFile na pasta de entrada da pasta assistida. Após o processamento bem-sucedido dos arquivos, os resultados são colocados sob a pasta de resultados da sua pasta monitorada.


>[!NOTE]
>
>Se o script associado à pasta assistida precisar de mais de um arquivo, será necessário criar uma pasta e colocar todos os arquivos necessários na pasta e soltar a pasta na pasta de entrada da pasta assistida.
>
>Se o script associado à pasta monitorada precisar de apenas um arquivo de entrada, você poderá soltar o arquivo diretamente na pasta de entrada da pasta monitorada

