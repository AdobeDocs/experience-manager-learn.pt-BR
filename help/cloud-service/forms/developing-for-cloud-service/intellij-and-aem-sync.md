---
title: Configuração do IntelliJ com a ferramenta Repo
description: Preparar seu IntelliJ para sincronização com a instância pronta para nuvem AEM
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
kt: 8844
exl-id: 9a7ed792-ca0d-458f-b8dd-9129aba37df6
source-git-commit: 10ff0d87991d7766d5ca9563062a2f7be6035e43
workflow-type: tm+mt
source-wordcount: '515'
ht-degree: 3%

---

# Instalação do Cygwin


Cygwin é um ambiente de programação e tempo de execução compatível com POSIX que é executado nativamente no Microsoft Windows.
Instalar [Cygwin](https://www.cygwin.com/). Instalei o na pasta C:\cygwin64
>[!NOTE]
> Certifique-se de instalar pacotes zip, unzip, curl e rsync com sua instalação do cygwin

Crie uma pasta chamada adoberepo em c:\cloudmanager.

[Instalar a ferramenta repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo) A instalação da ferramenta de repositório nada mais é do que copiar o arquivo do repositório e colocá-lo na pasta c:\cloudmanger\adoberepo.

Adicione o seguinte à variável de ambiente Caminho C:\cygwin64\bin;C:\CloudManager\adoberepo;

## Configurar ferramentas externas

* Iniciar IntelliJ
* Pressione Ctrl+Alt+S para iniciar a janela de configurações.
* Selecione Ferramentas->Ferramentas externas, clique no sinal + e insira o seguinte, conforme mostrado na captura de tela.
  ![rep](assets/repo.png)
* Certifique-se de criar um grupo chamado repo digitando &quot;repo&quot; no campo suspenso Grupo e todos os comandos criados pertencem a **repo** grupo


**Comando Put**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo put -f \$FilePath\$
**Diretório de trabalho**: \$ProjectFileDir\$
![put-command](assets/put-command.png)

**Obter Comando**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo get -f \$FilePath\$
**Diretório de trabalho**: \$ProjectFileDir\$
![get-command](assets/get-command.png)

**Comando de Status**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo st -f \$FilePath\$
**Diretório de trabalho**: \$ProjectFileDir\$
![status-command](assets/status-command.png)

**Comando &#39;Diff&#39;**
**Programa**: C:\cygwin64\bin\bash
**Argumentos**: -l C:\CloudManager\adoberepo\repo diff -f $FilePath$
**Diretório de trabalho**: \$ProjectFileDir\$
![diff-command](assets/diff-command.png)

Extraia o arquivo .repo de [repo.zip](assets/repo.zip) e coloque-o na pasta raiz dos projetos AEM. C:\CloudManager\aem-banking-application). Abra o arquivo .repo e verifique se o servidor e as configurações de credenciais correspondem ao seu ambiente.
Abra o arquivo .gitignore, adicione o seguinte na parte inferior do arquivo e salve as alterações \# repo .repo

Selecione qualquer projeto no projeto aem-banking-application, como ui.content, e clique com o botão direito do mouse. Você verá a opção repo e, sob a opção repo, você verá os 4 comandos que adicionamos anteriormente.

## Configurar instância do autor do AEM

As etapas a seguir podem ser seguidas para configurar rapidamente a instância pronta para nuvem no sistema local.
* [Baixe o SDK do AEM mais recente](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* [Baixar o complemento mais recente do AEM Forms](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)

* Crie a seguinte estrutura de pastas c:\aemformscs\aem-sdk\author

* Extraia o arquivo aem-sdk-quickstart-xxxxxxx.jar do arquivo zip do SDK do AEM e coloque-o na pasta c:\aemformscs\aem-sdk\author. Renomeie o arquivo jar para aem-author-p4502.jar

* Abra o prompt de comando e navegue até c:\aemformscs\aem-sdk\author insira o seguinte comando java -jar aem-author-p4502.jar -gui. Isso iniciará a instalação do AEM.
* Fazer logon usando credenciais de administrador/administrador
* Parar a instância do AEM
* Crie a seguinte estrutura de pastas.C:\aemformscs\aem-sdk\author\crx-quickstart\install
* Copie o aem-forms-addon-xxxxxx.far para a pasta de instalação
* Abra o prompt de comando e navegue até c:\aemformscs\aem-sdk\author insira o seguinte comando java -jar aem-author-p4502.jar -gui. Isso implantará o pacote complementar de formulários na instância do AEM.

## Próximas etapas

[Sincronizar formulários e modelos do AEM com o projeto AEM](./deploy-your-first-form.md)
