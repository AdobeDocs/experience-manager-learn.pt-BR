---
title: Etapas simplificadas para instalar o AEM Forms no Windows
seo-title: Etapas simplificadas para instalar o AEM Forms no Windows
description: Etapas rápidas e fáceis de instalar o AEM Forms no Windows
seo-description: Etapas rápidas e fáceis de instalar o AEM Forms no Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: Formulários adaptáveis
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 4%

---


# Etapas simplificadas para instalar o AEM Forms no Windows

>[!NOTE]
>
>Nunca clique duas vezes no jar AEM Quick Start , se você pretende usar o AEM Forms.
>
>Além disso, verifique se não há espaços no caminho da pasta Instalação do AEM Forms.
>
>Por exemplo, não instale o AEM Forms em c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Se você estiver instalando o AEM Forms 6.5, certifique-se de ter instalado os seguintes redistribuíveis de 32 bits do Microsoft Visual C++.
>
>* Microsoft Visual C++ 2008 redistribuível
>* Microsoft Visual C++ 2010 redistribuível
>* Microsoft Visual C++ 2012 redistribuível
>* Microsoft Visual C++ 2013 redistribuível (a partir de 6.5)


Embora seja recomendável seguir a [documentação oficial](https://helpx.adobe.com/br/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) para instalar o AEM Forms. As etapas a seguir podem ser seguidas para instalar e configurar o AEM Forms no ambiente Windows:

* Certifique-se de ter o JDK apropriado instalado
   * AEM 6.2 você precisa: Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 e AEM 6.4 você precisa: Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 você precisa do JDK 8 ou do JDK 11
* [Os ](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) requisitos oficiais do JDK estão listados aqui
* Verifique se JAVA_HOME está definido para apontar para o JDK que você instalou.
   * Para criar a variável JAVA_HOME no Windows, siga as etapas abaixo:
      * Clique com o botão direito do mouse em Meu computador e selecione Propriedades
      * Na guia Advanced , selecione Environment Variables e crie uma nova variável de sistema chamada JAVA_HOME.
      * Defina o valor da variável para apontar para JDK instalado no sistema. Por exemplo c:\program files\java\jdk1.8.0_25

* Crie uma pasta chamada AEMForms na unidade C
* Localize o AEMQuickStart.Jar e mova-o para a pasta AEMForms
* Copie o arquivo license.properties para esta pasta AEMForms
* Crie um arquivo em lote chamado &quot;StartAemForms.bat&quot; com o seguinte conteúdo:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Aqui AEM_6.3_Quickstart.jar é o nome do meu jar de início rápido do AEM.
   * Você pode renomear seu jar para qualquer nome, mas certifique-se de que o nome seja refletido no arquivo em lote.Salve o arquivo em lote na Pasta AEMForms.

* Abra um novo prompt de comando e navegue até c:\aemforms.

* Execute o arquivo StartAemForms.bat a partir do prompt de comando.

* Você deve obter uma pequena caixa de diálogo indicando o progresso da inicialização.

* Quando a inicialização estiver concluída, abra o arquivo sling.properties . Está localizado em c:\AEMForms\crx-quickstart\conf folder.

* Copie as 2 linhas a seguir na parte inferior do arquivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* Essas duas propriedades são necessárias para que os serviços de documento funcionem
* Salve o arquivo sling.properties

* [Fazer login no Package Share](http://localhost:4502/crx/packageshare/login.html)

   * Você precisará do Adobe Id para fazer logon no compartilhamento de pacotes
   * Procure o pacote AEM Forms Add on apropriado para sua versão do AEM Forms e sistema operacional
   * Ou [você pode baixar o pacote de adjuntos de formulários apropriado](https://helpx.adobe.com/br/aem-forms/kb/aem-forms-releases.html)
   * Após instalar o pacote de complementos, siga as etapas a seguir

      * **Verifique se todos os pacotes estão no estado ativo. (Exceto para o pacote de assinaturas AEMFD).**
      * **Normalmente, levaria 5 ou mais minutos para todos os pacotes chegarem ao estado ativo.**
   * **Quando todos os pacotes estiverem ativos (exceto o pacote de assinaturas do AEMFD), reinicie o sistema para concluir a instalação do AEM Forms**


* Adicione o pacote `sun.util.calendar` à lista permitida:

   1. Abra o console da Web Felix em sua [janela do navegador](http://localhost:4502/system/console/configMgr)
   2. Pesquise e abra a Configuração do firewall de desserialização: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Adicionar `sun.util.calendar` como uma nova entrada em `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Salve as alterações.

Parabéns!!! Agora você instalou e configurou o AEM Forms em seu sistema.
Dependendo das suas necessidades, você pode configurar [Reader Extensions](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) ou [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) no seu servidor
