---
title: Etapas simplificadas para instalar o AEM Forms no Windows
description: Etapas rápidas e fáceis para instalar o AEM Forms no Windows
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
source-git-commit: 5c53919dd038c0992e1fe5dd85053f26c03c5111
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 5%

---

# Etapas simplificadas para instalar o AEM Forms no Windows

>[!NOTE]
>
>Nunca clique duas vezes no jar AEM Início rápido, se você pretende usar o AEM Forms.
>
>Além disso, verifique se não há espaços no caminho da pasta Instalação do AEM Forms.
>
>Por exemplo, não instale o AEM Forms em c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Se você estiver instalando o AEM Forms 6.5, certifique-se de ter instalado as seguintes redistribuíveis Microsoft Visual C++ de 32 bits.
>
>* Microsoft Visual C++ 2008 redistribuível
>* Microsoft Visual C++ 2010 redistribuível
>* Microsoft Visual C++ 2012 redistribuível
>* Microsoft Visual C++ 2013 redistribuível (a partir de 6.5)


Embora recomendemos seguir a [documentação oficial](https://helpx.adobe.com/br/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) para instalar o AEM Forms. As etapas a seguir podem ser seguidas para instalar e configurar o AEM Forms no ambiente Windows:

* Certifique-se de ter o JDK apropriado instalado
   * AEM 6.2 você precisa: Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 e AEM 6.4 você precisa: Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 você precisa do JDK 8 ou do JDK 11
* [Requisitos oficiais do JDK](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=pt-BR) estão listados aqui
* Verifique se JAVA_HOME está definido para apontar para o JDK que você instalou.
   * Para criar a variável JAVA_HOME no Windows, siga as etapas abaixo:
      * Clique com o botão direito do mouse em Meu computador e selecione Propriedades
      * Na guia Advanced , selecione Environment Variables e crie uma nova variável de sistema chamada JAVA_HOME.
      * Defina o valor da variável para apontar para JDK instalado no sistema. Por exemplo c:\program files\java\jdk1.8.0_25

* Crie uma pasta chamada AEMForms na unidade C
* Localize o AEMQuickStart.Jar e mova-o para a pasta AEMForms
* Copie o arquivo license.properties para esta pasta AEMForms
* Crie um arquivo em lote chamado &quot;StartAemForms.bat&quot; com o seguinte conteúdo:
   * java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui. Aqui AEM_6.5_Quickstart.jar é o nome do meu jar AEM quickstart.
   * Você pode renomear seu jar com qualquer nome, mas certifique-se de que o nome seja refletido no arquivo de lote. Salve o arquivo em lote na Pasta AEMForms.

* Abra um novo prompt de comando e navegue até _c:\aemforms_.

* Execute o arquivo StartAemForms.bat a partir do prompt de comando.

* Você deve obter uma pequena caixa de diálogo indicando o progresso da inicialização.

* Quando a inicialização estiver concluída, abra o arquivo sling.properties . Está localizado em c:\AEMForms\crx-quickstart\conf folder.

* Copie as 2 linhas a seguir na parte inferior do arquivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Essas duas propriedades são necessárias para que os serviços de documento funcionem
* Salve o arquivo sling.properties
* [Baixe o pacote de addon de formulários apropriado](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* Instale o pacote de complementos de formulários usando [gerenciador de pacotes.](http://localhost:4502/crx/packmgr/index.jsp)
* Depois de instalar o pacote adicional, siga as seguintes etapas

       * **Certifique-se de que todos os pacotes estejam no estado ativo. (Exceto para o pacote de assinaturas AEMFD).**
       * **Normalmente levaria 5 ou mais minutos para todos os pacotes chegarem ao estado ativo.**
   
   * **Quando todos os pacotes estiverem ativos (exceto o pacote de assinaturas do AEMFD), reinicie o sistema para concluir a instalação do AEM Forms**

## pacote sun.util.calendar para a lista de permissões

1. Abra o console da Web do Felix em seu [janela do navegador](http://localhost:4502/system/console/configMgr)
2. Pesquise e abra a Configuração do firewall de desserialização: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. Adicionar `sun.util.calendar` como nova entrada em `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. Salve as alterações.

Parabéns!!! Agora você instalou e configurou o AEM Forms em seu sistema.
Dependendo das suas necessidades, você pode configurar  [Extensões Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) ou [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=pt-BR) no seu servidor
