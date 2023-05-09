---
title: Gerando documentos de canal de impressão usando a pasta assistida
seo-title: Generating Print Channel Documents Using Watched Folder
description: Esta é a parte 10 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas para o canal de impressão. Nesta parte, geraremos documentos de canal de impressão usando o mecanismo de pasta monitorada.
seo-description: This is part 10 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will generate print channel documents using the watched folder mechanism.
uuid: 9e39f4e3-1053-4839-9338-09961ac54f81
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
contentOwner: gbedekar
discoiquuid: 23fbada3-d776-4b77-b381-22d3ec716ae9
topic: Development
role: Developer
level: Beginner
exl-id: 9bb05c94-2a7b-4149-b567-186eb08b1c66
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# Gerando documentos de canal de impressão usando a pasta assistida

Nesta parte, geraremos documentos de canal de impressão usando o mecanismo de pasta monitorada.

Depois de criar e testar o documento do canal de impressão, precisamos de um mecanismo para gerar esse documento em modo de lote ou sob demanda. Normalmente, esses tipos de documentos são gerados no modo de lote e o mecanismo mais comum está usando a pasta assistida.

Ao configurar uma pasta assistida no AEM, você associa um script ECMA ou código java que é executado quando um arquivo é solto na pasta assistida. Neste artigo, o foco será no script ECMA que gerará documentos de canal de impressão e os salvará no sistema de arquivos.

A configuração de pasta assistida e o script ECMA são parte dos ativos importados na [início deste tutorial](introduction.md)

O arquivo de entrada que é solto na pasta assistida tem a seguinte estrutura. O script ECMA lê os números de conta e gera um documento de canal de impressão para cada uma dessas contas.

Para obter mais detalhes sobre o script ECMA para gerar documentos, [consulte este artigo](/help/forms/interactive-communications/generating-interactive-communications-print-document-using-api-tutorial-use.md)

```xml
<accountnumbers>
 <accountnumber>509840</accountnumber>
 <accountnumber>948576</accountnumber>
 <accountnumber>398762</accountnumber>
 <accountnumber>291723</accountnumber>
 <accountnumber>291724</accountnumber>
 <accountnumber>291725</accountnumber>
 <accountnumber>291726</accountnumber>
 <accountnumber>291727</accountnumber>
</accountnumbers>
```

Para gerar o documento do canal de impressão usando o mecanismo de pasta assistida, siga as etapas abaixo:

* [Siga as etapas mencionadas neste documento](/help/forms/adaptive-forms/service-user-tutorial-develop.md)

* Faça logon no crx e acesse /etc/fd/watchfolder/scripts/PrintPDF.ecma

* Certifique-se de que o caminho para interativeCommunicationsDocument está apontando para o documento correto que você deseja imprimir.( Linha 1)
* Anote o saveLocation(Line 2). Você pode alterá-lo de acordo com suas necessidades.
* Verifique se o parâmetro de entrada do Modelo de dados de formulário está vinculado ao Atributo de solicitação e se seu valor de vínculo está definido como &quot;accountnumber&quot;. Consulte a captura de tela abaixo.
   ![solicitação](assets/requestattributeprintchannel.gif)

* Crie o arquivo accountnumbers.xml com o seguinte conteúdo

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```

* Solte o arquivo xml em C:\RenderPrintChannel\input

* Verifique os arquivos pdf no local de salvamento, conforme especificado no script ECMA.

## Próximas etapas

[Abrir a interface do agente no envio do formulário](./opening-agent-ui-on-form-submission.md)