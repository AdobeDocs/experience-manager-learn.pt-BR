---
title: Usar pastas assistidas no AEM Forms
description: Configurar e usar pastas vigiadas no AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '423'
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
* [Baixe e extraia o conteúdo do arquivo zip](assets/outputservicewatchedfolderkt.zip).Este arquivo zip contém pacote para criar arquivos de pasta monitorada e de amostra para testar o serviço de saída usando o mecanismo de pasta monitorada
   * Sistema Windows

      * Importe o outputservicewatchedfolder.zip no AEM usando o gerenciador de pacotes
      * Isso cria uma pasta monitorada chamada outputservicewatchedfolder na unidade C.
   * Sistema não Windows
      * [Abra a configuração da pasta monitorada](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Defina a propriedade de caminho da pasta do nó outservice para apontar para um local adequado
      * Salve as alterações
      * O local mencionado acima é a sua pasta monitorada.

Solte as pastas SamplePdfFile e SampleXdpFile na pasta de entrada da pasta assistida. Ao processar os arquivos com êxito, os resultados são colocados na pasta de resultados da pasta monitorada.


>[!NOTE]
>
>Se o script associado à pasta assistida precisar de mais de um arquivo, será necessário criar uma pasta, colocar todos os arquivos necessários na pasta e soltar a pasta na pasta de entrada da pasta assistida.
>
>Se o script associado à pasta monitorada precisar de apenas um arquivo de entrada, você poderá soltar o arquivo diretamente na pasta de entrada da pasta assistida
