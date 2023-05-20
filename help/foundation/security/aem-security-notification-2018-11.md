---
title: Notificação de segurança do AEM (novembro de 2018)
seo-title: AEM Security Notification (November 2018)
description: Dispatcher de notificação de segurança do AEM Experience Manager
seo-description: AEM Experience Manager Security Notification Dispatcher
version: 6.4
feature: Dispatcher
topics: security
activity: understand
audience: all
doc-type: article
uuid: 3ccf7323-4061-49d7-ae95-eb003099fd77
discoiquuid: 9d181b3e-fbd5-476d-9e97-4452176e495c
topic: Security
role: Architect
level: Beginner
exl-id: 1ee11a01-9e49-42f4-aec4-09e51f769f69
source-git-commit: 4b47daf82e27f6bea4be30e3cdd132f497f4c609
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 14%

---

# Notificação de segurança do AEM (novembro de 2018)

## Resumo

Este artigo aborda algumas vulnerabilidades recentes e antigas que foram relatadas recentemente no AEM. Observe que a maioria das vulnerabilidades identificadas eram problemas conhecidos do produto AEM e que a mitigação foi identificada anteriormente. Uma nova versão do dispatcher está disponível para as novas vulnerabilidades. A Adobe também solicita que os clientes concluam o [Lista de verificação de segurança do AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/security-checklist.html) e siga as orientações pertinentes.

## Ação obrigatória

* As implantações de AEM devem começar a usar a versão mais recente do Dispatcher.
* As regras de segurança do dispatcher devem ser aplicadas de acordo com a configuração recomendada.
* A variável [Lista de verificação de segurança do AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/security-checklist.html) deve ser concluída para implantações de AEM.

## Vulnerabilidades e resoluções

| Problema | Resolução | Links |
|-------|------------|-------|
| Ignorar regras do Dispatcher do AEM | Instale a versão mais recente do Dispatcher(4.3.1) e siga a configuração recomendada do Dispatcher. | Consulte [Notas de versão do Dispatcher do AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configuração do Dispatcher](https://helpx.adobe.com/pt/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| O filtro de URL ignora a vulnerabilidade que poderia ser usada para contornar as regras do dispatcher - CVE-2016-0957 | Isso foi corrigido em uma versão mais antiga do Dispatcher, mas agora é recomendável instalar a versão mais recente do Dispatcher (4.3.1) e seguir a configuração recomendada do Dispatcher. | Consulte [Notas de versão do Dispatcher do AEM](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html) e [Configuração do Dispatcher](https://helpx.adobe.com/pt/experience-manager/dispatcher/using/dispatcher-configuration.html). |
| Vulnerabilidade XSS relacionada a arquivos SWF armazenados | Isso foi resolvido com correções de segurança lançadas anteriormente. | Consulte [Boletim de segurança do AEM APSB18-10](https://helpx.adobe.com/security/products/experience-manager/apsb18-10.html). |
| Exploits relacionados a senhas | Siga a recomendação na Lista de verificação de segurança para senhas mais seguras. | Consulte [Lista de verificação de segurança do AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/administering/using/security-checklist.html) |
| Exposição de uso do disco para usuários anônimos | Esse problema foi resolvido para o AEM 6.1 e posteriores, para o AEM 6.0, as permissões predefinidas podem ser modificadas para serem mais restritivas. | Consulte [notas de versão](https://helpx.adobe.com/experience-manager/aem-previous-versions.html)para AEM 6.1 e mais antigo. |
| Exposição do Proxy Social Aberto para usuários anônimos | Isso foi resolvido nas versões a partir da 6.0 SP2. | Consulte [notas de versão](https://helpx.adobe.com/experience-manager/aem-previous-versions.html) para AEM 6.1 e mais antigo. |
| Acesso ao CRX Explorer em instâncias de produção | O gerenciamento do acesso ao CRX Explorer já é abordado na Lista de verificação de segurança. O CRX Explorer deve ser removido do autor de produção e da publicação, e a verificação de integridade da segurança o relata, se não for removido. | Consulte [Lista de verificação de segurança do AEM](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security-checklist.html?lang=pt-BR). |
| BGServlets está exposto | Isso foi resolvido desde o AEM 6.2. | Consulte [Notas de versão do AEM 6.2](https://helpx.adobe.com/br/experience-manager/6-2/release-notes.html) |

>[!MORELIKETHIS]
>
>* [Guia do usuário do AEM Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/user-guide.html)
>* [Notas de lançamento do AEM Dispatcher Release ](https://helpx.adobe.com/experience-manager/dispatcher/release-notes.html)
>* [Boletins de segurança do AEM](https://helpx.adobe.com/security.html#experience-manager)

