---
title: Usar pastas assistidas no AEM Forms
description: Configurar e usar pastas vigiadas no AEM Forms
feature: Serviço de saída
version: 6.4,6.5
topic: Desenvolvimento
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# Usar pastas assistidas no AEM Forms{#using-watched-folders-in-aem-forms}

Um administrador pode configurar uma pasta de rede, conhecida como Pasta assistida, de modo que, quando um usuário colocar um arquivo (como um arquivo PDF) na Pasta assistida, um fluxo de trabalho, serviço ou operação de script pré-configurado seja iniciado para processar o arquivo adicionado. Depois que o serviço executa a operação especificada, ele salva o arquivo de resultado em uma pasta de saída especificada. Para obter mais informações sobre workflow, serviço e script.

Para saber mais sobre como criar uma pasta assistida, [clique aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

Pastas vigiadas são usadas para gerar documentos no modo de lote. Usando o mecanismo de pasta monitorada, você pode gerar Comunicações interativas para o canal de impressão ou usar o serviço de saída para mesclar dados com o modelo.

Este artigo abordará o caso de uso da mesclagem de dados com um modelo usando o serviço de saída via mecanismo de pasta monitorada.

O serviço de saída é um serviço OSGi que faz parte AEM Document Services. O serviço de saída oferece suporte a vários formatos de saída e recursos de design de saída do AEM Forms Designer. O serviço de saída pode converter modelos XFA e dados XML para gerar documentos de impressão em vários formatos.

Para saber mais sobre o serviço de saída, [clique aqui](https://helpx.adobe.com/aem-forms/6/output-service.html).
Para configurar uma pasta monitorada em seu sistema, siga as etapas abaixo:
* [Baixe e extraia o conteúdo do arquivo zip](assets/outputservicewatchedfolderkt.zip). Esse arquivo zip contém o pacote para criar pastas monitoradas e arquivos de amostra para testar o serviço de saída usando o mecanismo de pasta monitorada
   * Sistema Windows

      * Importe o outputservicewatchedfolder.zip no AEM usando o gerenciador de pacotes
      * Isso criará uma pasta assistida chamada outputservicewatchedfolder na unidade C.
   * Sistema não Windows
      * [Abra a configuração da pasta monitorada](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Defina a propriedade de caminho da pasta do nó outservice para apontar para um local adequado
      * Salve as alterações
      * O local mencionado acima será a sua pasta monitorada.

Solte as pastas SamplePdfFile e SampleXdpFile na pasta de entrada da pasta assistida. Ao processar os arquivos com êxito, os resultados são colocados na pasta de resultados da pasta monitorada.


>[!NOTE]
>
>Se o script associado à pasta assistida precisar de mais de um arquivo, será necessário criar uma pasta, colocar todos os arquivos necessários na pasta e soltar a pasta na pasta de entrada da pasta assistida.
>
>Se o script associado à pasta monitorada precisar de apenas um arquivo de entrada, você poderá soltar o arquivo diretamente na pasta de entrada da pasta assistida

