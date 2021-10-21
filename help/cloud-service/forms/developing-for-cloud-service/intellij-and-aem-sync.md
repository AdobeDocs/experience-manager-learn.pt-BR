---
title: Configuração do IntelliJ com a ferramenta Repo
description: Prepare seu IntelliJ para sincronizar com AEM instância pronta para nuvem
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8844
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '504'
ht-degree: 2%

---

# Instalação do Cygwin


O Cygwin é um ambiente de programação e tempo de execução compatível com POSIX, executado nativamente no Microsoft Windows.
Instalar [Cygwin](https://www.cygwin.com/). Eu instalei em C:\cygwin64 folder
>[!NOTE]
> Certifique-se de instalar pacotes zip, unzip, curl e rsync com sua instalação do cygwin

Crie uma pasta chamada adoberepo no diretório c:\cloudmanager.

[Instale a ferramenta repo].(https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo).Installing a ferramenta repo não é nada além de copiar o arquivo repo e colocá-lo em seu c:\cloudmanger\adoberepo folder.

Adicione o seguinte à variável de ambiente Caminho C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Configurar ferramentas externas

* Iniciar o IntelliJ
* Pressione as teclas Ctrl+Alt+S para iniciar a janela de configurações.
* Selecione Ferramentas->Ferramentas externas e clique no sinal + e insira o seguinte, como mostrado na captura de tela.
   ![rep](assets/repo.png)
* Crie um grupo chamado repo digitando &quot;repo&quot; no campo suspenso Grupo e todos os comandos criados pertencem ao grupo **repo** grupo


**Comando Put**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Diretório de trabalho**: \$ProjectFileDir\$
![comando put](assets/put-command.png)

**Comando Get**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Diretório de trabalho**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**Comando Status**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Diretório de trabalho**: \$ProjectFileDir\$
![status-command](assets/status-command.png)

**Comando Diff**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**Diretório de trabalho**: \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

Extraia o arquivo .repo de [repo.zip](assets/repo.zip) e coloque-o na pasta raiz AEM projetos . (C:\CloudManager\aem-banking-application). Abra o arquivo .repo e verifique se o servidor e as configurações de credenciais correspondem ao seu ambiente.
Abra o arquivo .gitignore e adicione o seguinte na parte inferior do arquivo e salve as alterações \# repo .repo

Selecione qualquer projeto em seu projeto do aem-banking-application, como ui.content e right click, e você deverá ver a opção repo e, na opção repo , verá os 4 comandos que adicionamos anteriormente.

## Configurar a instância do autor do AEM

As etapas a seguir podem ser seguidas para configurar rapidamente a instância pronta para nuvem em seu sistema local.
* [Baixe o SDK do AEM mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [Baixe o complemento AEM Forms mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* Crie a seguinte estrutura de pastas c:\aemformscs\aem-sdk\author

* Extraia o arquivo aem-sdk-quickstart-xxxxxx.jar do arquivo zip do SDK AEM e coloque-o no arquivo jar c:\aemformscs\aem-sdk\author folder.Rename para aem-author-p4502.jar

* Abra o prompt de comando e navegue até c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. Isso iniciará a instalação do AEM.
* Faça logon usando credenciais de administrador/administrador
* Pare a instância de AEM
* Crie a seguinte estrutura de pastas.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Copie o aem-forms-addon-xxxxx.far na pasta de instalação
* Abra o prompt de comando e navegue até c:\aemformscs\aem-sdk\author enter the following command java -jar aem-author-p4502.jar -gui. Isso implantará o pacote de complementos de formulários na sua instância de AEM.



















