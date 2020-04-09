---
title: Impostazioni dei criteri di gruppo usate in Autenticazione di Windows
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7acd7439ec2e0382f1e2c725d66363e6b3a50b40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857574"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Impostazioni dei criteri di gruppo usate in Autenticazione di Windows

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive l'uso e l'effetto delle impostazioni di Criteri di gruppo nel processo di autenticazione.

Per gestire l'autenticazione nei sistemi operativi Windows, è possibile aggiungere utenti, computer e account del servizio ai gruppi, quindi applicare i criteri di autenticazione a tali gruppi. Questi criteri sono definiti come criteri di sicurezza locali e come modelli amministrativi, noti anche come impostazioni di Criteri di gruppo. È possibile configurare e distribuire entrambi i set in tutta l'organizzazione utilizzando Criteri di gruppo.

> [!NOTE]
> Le funzionalità introdotte in Windows Server 2012 R2 consentono di configurare i criteri di autenticazione per le applicazioni o i servizi di destinazione, comunemente denominati silo di autenticazione, usando gli account protetti. Per informazioni su come eseguire questa operazione in Active Directory, vedere [How to configure Protected accounts](how-to-configure-protected-accounts.md).

È ad esempio possibile applicare i criteri seguenti ai gruppi in base alla relativa funzione nell'organizzazione:

-   Accedere localmente o a un dominio

-   Accesso tramite rete

-   Reimposta account

-   Crea account

Nella tabella seguente sono elencati i gruppi di criteri rilevanti per l'autenticazione di e vengono forniti i collegamenti alla documentazione che consentono di configurare tali criteri.

|Gruppo di criteri|Location|Descrizione|
|--------|------|--------|
|**Criteri password**|Configurazione computer\Impostazioni di Windows\Impostazioni Locale\configurazione protezione\Criteri criteri del computer locale|I criteri password influiscono sulle caratteristiche e sul comportamento delle password. I criteri password vengono utilizzati per gli account di dominio o gli account utente locali. Determinano le impostazioni per le password, ad esempio l'imposizione e la durata.<p>Per informazioni sulle impostazioni specifiche, vedere [criteri password](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Criteri di blocco degli account**|Configurazione computer\Impostazioni di Windows\Impostazioni Locale\configurazione protezione\Criteri criteri del computer locale|Le opzioni dei criteri di blocco degli account disabilitano gli account dopo un numero impostato di tentativi di accesso non riusciti. L'utilizzo di queste opzioni consente di rilevare e bloccare i tentativi di interrompere le password.<p>Per informazioni sulle opzioni dei criteri di blocco degli account, vedere [criteri di blocco degli account](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Criteri di Kerberos**|Configurazione computer\Impostazioni di Windows\Impostazioni Locale\configurazione protezione\Criteri criteri del computer locale|Le impostazioni relative a Kerberos includono le regole per la durata e l'imposizione di ticket. Il criterio Kerberos non si applica ai database degli account locali poiché il protocollo di autenticazione Kerberos non viene utilizzato per autenticare gli account locali. Pertanto, le impostazioni dei criteri Kerberos possono essere configurate solo tramite l'oggetto Criteri di gruppo dominio predefinito (GPO), in cui influiscono sugli accessi al dominio.<p>Per informazioni sulle opzioni dei criteri Kerberos per il controller di dominio, vedere [criteri Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Criteri di controllo**|Computer locale Locale\configurazione computer\Impostazioni di Windows\Impostazioni protezione\Criteri locali\Criteri controllo criterio|I criteri di controllo consentono di controllare e comprendere l'accesso agli oggetti, ad esempio file e cartelle, nonché di gestire gli account utente e di gruppo e gli accessi e le disconnessioni degli utenti. I criteri di controllo possono specificare le categorie di eventi che si desidera controllare, impostare le dimensioni e il comportamento del log di sicurezza e determinare gli oggetti per i quali si desidera monitorare l'accesso e il tipo di accesso che si desidera monitorare.<p>|
|**Assegnazione diritti utente**|Configurazione computer\Impostazioni Locale\configurazione Windows\Impostazioni protezione\Criteri locali\Assegnazione diritti di Windows\Impostazioni|I diritti utente vengono in genere assegnati in base ai gruppi di sicurezza a cui appartiene un utente, ad esempio amministratori, utenti avanzati o utenti. Le impostazioni dei criteri in questa categoria vengono in genere utilizzate per concedere o negare l'autorizzazione per accedere a un computer in base al metodo di accesso e alle appartenenze ai gruppi di sicurezza.|
|**Opzioni di sicurezza**|Computer locale Locale\configurazione computer\Impostazioni di Windows\Impostazioni protezione\Criteri locali\Opzioni opzioni|I criteri relativi all'autenticazione includono:<p>-Dispositivi<br />-Controller di dominio<br />-Membro del dominio<br />-Accesso interattivo<br />-Server di rete Microsoft<br />-Accesso alla rete<br />-Sicurezza di rete<br />-Console di ripristino<br />-Arresta<p>|
|**Delega delle credenziali**|Configurazione computer\Modelli Templates\System\Credentials|La delega delle credenziali è un meccanismo che consente di usare le credenziali locali in altri sistemi, in particolare i server membri e i controller di dominio all'interno di un dominio. Queste impostazioni si applicano alle applicazioni usando Credential Security Support Provider (credito SSP). Connessione Desktop remoto è un esempio.|
|**KDC**|Configurazione computer\Modelli Templates\System\KDC|Queste impostazioni dei criteri influiscono sul modo in cui il Centro distribuzione chiavi (KDC), che è un servizio sul controller di dominio, gestisce le richieste di autenticazione Kerberos.|
|**Kerberos**|Configurazione computer\Modelli Templates\System\Kerberos|Queste impostazioni di criteri influiscono sul modo in cui Kerberos è configurato per gestire il supporto per attestazioni, blindatura Kerberos, autenticazione composta, server proxy di identificazione e altre configurazioni.|
|**Accesso**|Configurazione computer\Modelli amministrativi\Sistema\Accesso|Queste impostazioni dei criteri controllano il modo in cui il sistema presenta l'esperienza di accesso per gli utenti.|
|**Accesso rete**|Configurazione computer\Modelli Templates\System\Net Logon|Queste impostazioni dei criteri controllano il modo in cui il sistema gestisce le richieste di accesso alla rete, incluso il comportamento del localizzatore del controller di dominio.<p>Per ulteriori informazioni sul modo in cui il localizzatore del controller di dominio si inserisce nei processi di replica, vedere [informazioni sulla replica tra siti](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biometria**|Configurazione computer\Modelli Amministrativi\componenti di Components\Biometrics|Queste impostazioni dei criteri consentono in genere o negano l'uso di biometria come metodo di autenticazione.<p>Per informazioni sull'implementazione di Windows della biometria, vedere Windows Biometric Framework Overview.|
|**Interfaccia utente delle credenziali**|Configurazione computer\Modelli Amministrativi\componenti di Components\Credential interfaccia utente|Queste impostazioni dei criteri controllano la modalità di gestione delle credenziali in corrispondenza del punto di ingresso.|
|**Sincronizzazione password**|Configurazione computer\Modelli Amministrativi\componenti di Components\Password|Queste impostazioni dei criteri determinano il modo in cui il sistema gestisce la sincronizzazione delle password tra i sistemi operativi Windows e UNIX.<p>Per altre informazioni, vedere [sincronizzazione delle password](https://technet.microsoft.com/library/cc732609.aspx).|
|**Smart Card**|Configurazione computer\Modelli Amministrativi\componenti di Components\Smart|Queste impostazioni dei criteri controllano il modo in cui il sistema gestisce gli accessi tramite smart card.<p>|
|**Opzioni di accesso di Windows**|Configurazione computer\Modelli Amministrativi\componenti di Windows\windows opzioni di accesso|Queste impostazioni dei criteri controllano quando e come sono disponibili le opportunità di accesso.|
|**Opzioni CTRL + ALT + CANC**|Configurazione computer\Modelli Amministrativi\componenti di Components\Ctrl + ALT + CANC|Queste impostazioni dei criteri influiscono sull'aspetto e sull'accessibilità delle funzionalità nell'interfaccia utente di accesso (desktop protetto), ad esempio Gestione attività e il blocco da tastiera del computer.|
|**Accesso**|Configurazione computer\Modelli Amministrativi\componenti di Components\Logon|Queste impostazioni dei criteri determinano se o quali processi possono essere eseguiti quando l'utente esegue l'accesso.|

## <a name="see-also"></a>Vedere anche
[Panoramica tecnica di Autenticazione di Windows](windows-authentication-technical-overview.md)


