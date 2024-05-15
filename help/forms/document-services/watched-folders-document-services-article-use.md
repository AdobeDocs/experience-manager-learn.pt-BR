---
title: Uso de pastas monitoradas no AEM Forms
description: Configurar e usar pastas monitoradas no AEM Forms
feature: Output Service
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: abb74d44-d1b9-44d6-a49f-36c01acfecb4
last-substantial-update: 2020-07-07T00:00:00Z
duration: 86
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Uso de pastas monitoradas no AEM Forms{#using-watched-folders-in-aem-forms}

Um administrador pode configurar uma pasta de rede, conhecida como Pasta monitorada, para que, quando um usuário colocar um arquivo (como um arquivo PDF) na Pasta monitorada, um fluxo de trabalho, serviço ou operação de script pré-configurado seja iniciado para processar o arquivo adicionado. Depois que o serviço executa a operação especificada, ele salva o arquivo de resultado em uma pasta de saída especificada. Para obter mais informações sobre fluxo de trabalho, serviço e script.

Para saber mais sobre como criar uma pasta monitorada, [clique aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/Creating-Configure-watched-folder.html)

As Pastas monitoradas são usadas para gerar documentos em modo de lote. Usando o mecanismo de pasta monitorada, você pode gerar Comunicações interativas para o canal de impressão ou usar o serviço de saída para mesclar dados com o modelo.

Este artigo abordará o caso de uso de mesclagem de dados com um modelo usando o serviço de saída por meio do mecanismo de pastas monitoradas.

O serviço de saída é um serviço OSGi que faz parte dos Serviços de documento AEM. O serviço de saída é compatível com vários formatos de saída e recursos de design de saída do AEM Forms Designer. O serviço de saída pode converter modelos XFA e dados XML para gerar documentos de impressão em vários formatos.

Para saber mais sobre o serviço de saída, [clique aqui](https://helpx.adobe.com/aem-forms/6/output-service.html).
Para configurar a pasta monitorada no seu sistema, siga as etapas abaixo:
* [Baixe e extraia o conteúdo do arquivo zip](assets/outputservicewatchedfolderkt.zip)Este arquivo zip contém um pacote para a criação de pastas monitoradas e arquivos de exemplo para testar o serviço de saída usando o mecanismo de pastas monitoradas
   * Sistema Windows

      * Importe o outputservicewatchedfolder.zip para o AEM usando o gerenciador de pacotes
      * Essa ação cria uma pasta monitorada chamada outputservicewatchedfolder na unidade C.
   * Sistema não Windows
      * [Abrir a definição de configuração da pasta monitorada](http://localhost:4502/crx/de/index.jsp#/etc/fd/watchfolder/config/outputservice)
      * Defina a propriedade de caminho da pasta do nó outservice para apontar para um local adequado
      * Salve as alterações
      * O local mencionado acima é sua pasta monitorada.

Solte as pastas SamplePdfFile e SampleXdpFile na pasta de entrada da pasta monitorada. Ao processar os arquivos com êxito, os resultados são colocados na pasta de resultados da pasta monitorada.


>[!NOTE]
>
>Se o script associado à pasta monitorada precisar de mais de um arquivo, será necessário criar uma pasta e colocar todos os arquivos necessários na pasta e soltar a pasta na pasta de entrada da pasta monitorada.
>
>Se o script associado à pasta monitorada precisar de apenas um arquivo de entrada, você poderá soltar o arquivo diretamente na pasta de entrada da pasta monitorada
