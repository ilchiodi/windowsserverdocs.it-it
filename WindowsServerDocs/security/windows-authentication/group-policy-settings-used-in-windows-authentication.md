---
title: Impostazioni dei criteri di gruppo usate in Autenticazione di Windows
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 38df2033b57c0394b96f539f54efe6a3579500f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847932"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Impostazioni dei criteri di gruppo usate in Autenticazione di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive l'uso e l'impatto delle impostazioni di criteri di gruppo nel processo di autenticazione.

È possibile gestire l'autenticazione nei sistemi operativi Windows tramite l'aggiunta di utenti, computer e account del servizio ai gruppi, quindi applicando i criteri di autenticazione a tali gruppi. Questi criteri sono definiti come criteri di sicurezza locali e i modelli amministrativi, noto anche come impostazioni di criteri di gruppo. Entrambi i set possono essere configurati e distribuiti in tutta l'organizzazione usando criteri di gruppo.

> [!NOTE]
> Le funzionalità introdotte in Windows Server 2012 R2, consentono di configurare i criteri di autenticazione per i servizi di destinazione o le applicazioni, comunemente denominato silo di autenticazione, usando gli account protetti. Per informazioni su come eseguire questa operazione in Active Directory, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

Ad esempio, è possibile applicare i criteri seguenti ai gruppi, in base alle funzione all'interno dell'organizzazione:

-   Accesso localmente o a un dominio

-   Accedere al sistema tramite una rete

-   Reimpostare gli account

-   Creare gli account

Nella tabella seguente sono elencati i gruppi di criteri pertinenti per l'autenticazione e fornisce collegamenti alla documentazione che consentono di configurare i criteri.

|Criteri di gruppo|Location|Descrizione|
|--------|------|--------|
|**Criteri password**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri criteri|I criteri password interessano le caratteristiche e il comportamento delle password. I criteri password vengono usati per gli account di dominio o account utente locali. E determinano le impostazioni per le password, ad esempio imposizione e durata.<br /><br />Per informazioni sulle impostazioni specifiche, vedere [criteri Password](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Criterio di blocco account**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri criteri|Le opzioni dei criteri di blocco degli account disabilitano degli account dopo un determinato numero di tentativi di accesso non riuscito. Usare queste opzioni consente di rilevare e bloccare i tentativi di password.<br /><br />Per informazioni sulle opzioni dei criteri di blocco degli account, vedere [criterio di blocco Account](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Criteri Kerberos**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri criteri|Le impostazioni relative a Kerberos includono regole di durata e l'imposizione di ticket. Criteri Kerberos non sono applicabile ai database di account locale perché il protocollo di autenticazione Kerberos non viene utilizzato per autenticare gli account locali. Di conseguenza, le impostazioni dei criteri Kerberos possono essere configurate solo tramite il dominio predefinito oggetto (criteri di gruppo), in cui influisce su accessi al dominio.<br /><br />Per informazioni sulle opzioni di criteri Kerberos per il controller di dominio, vedere [criteri Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Criteri di controllo**|Locale\Configurazione Computer locale Configurazione computer\Impostazioni di Windows\Impostazioni protezione\Criteri locali\Criteri controllo criteri|I criteri di controllo consentono di controllare e comprendere l'accesso agli oggetti, quali file e cartelle, nonché per gestire gli account utente e gruppo e gli accessi degli utenti e disconnessioni. I criteri di controllo è possono specificare le categorie di eventi che si desidera controllare, impostare le dimensioni e il comportamento del log di sicurezza e determinare di quali oggetti si desidera monitorare l'accesso e il tipo di accesso da monitorare.<br /><br />|
|**Assegnazione diritti utente**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni sicurezza\Criteri Locali\assegnazione diritti|Diritti utente vengono in genere assegnati in base a cui appartiene un utente, ad esempio amministratori, agli utenti di alimentazione o agli utenti i gruppi di sicurezza. Le impostazioni dei criteri in questa categoria sono in genere usate per concedere o negare l'autorizzazione per accedere a un computer in base al metodo dell'appartenenza a gruppi di accesso e sicurezza.|
|**Opzioni di sicurezza**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni sicurezza\Criteri Locali\opzioni di protezione|I criteri pertinenti per l'autenticazione includono:<br /><br />-I dispositivi<br />: Controller di dominio<br />: Membro del dominio<br />-Accesso interattivo<br />-Server di rete Microsoft<br />: Accesso alla rete<br />: Sicurezza di rete<br />-Console di ripristino<br />-Shutdown<br /><br />|
|**Delega delle credenziali**|Computer Configurazione computer\Modelli amministrativi\sistema\delega delega|La delega delle credenziali è un meccanismo che consente le credenziali locali di essere usato in altri sistemi, in particolare i server membri e i controller di dominio all'interno di un dominio. Queste impostazioni si applicano alle applicazioni tramite Credential Security Support Provider (SSP Cred). Connessione Desktop remoto è un esempio.|
|**KDC**|Computer Configurazione computer\Modelli Templates\System\KDC|Queste impostazioni dei criteri influiscono sul modo in cui il centro distribuzione chiavi (KDC), che è un servizio nel controller di dominio, gestisce le richieste di autenticazione Kerberos.|
|**Kerberos**|Computer Configurazione computer\Modelli Templates\System\Kerberos|Queste impostazioni dei criteri influiscono sul modo in cui Kerberos è configurato per gestire il supporto per le attestazioni, la blindatura Kerberos, autenticazione composta, identificazione i server proxy e altre configurazioni.|
|**Accesso**|Configurazione computer\Modelli amministrativi\Sistema\Accesso|Queste impostazioni dei criteri di controllo come il sistema presenta l'esperienza di accesso per gli utenti.|
|**Accesso rete**|Accesso al computer Configurazione computer\Modelli Templates\System\Net|Queste impostazioni dei criteri di controllo come il sistema gestisce le richieste di accesso di rete tra cui come si comporta il localizzatore di Controller di dominio.<br /><br />Per altre informazioni sul modo in cui il localizzatore di Controller di dominio si integra processi di replica, vedere [informazioni sulla replica tra siti](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biometria**|Computer Configurazione computer\Modelli amministrativi\Componenti Components\Biometrics|Queste impostazioni dei criteri, in genere, consentono o negare l'uso di dati biometrici come un metodo di autenticazione.<br /><br />Per informazioni sull'implementazione di biometria Windows, vedere Panoramica di Windows Biometric Framework.|
|**Interfaccia utente delle credenziali**|Computer Configurazione computer\Modelli amministrativi\Componenti di Windows\interfaccia utente|Queste impostazioni dei criteri di controllo come le credenziali vengono gestite nel punto di ingresso.|
|**Sincronizzazione delle password**|Computer Configurazione computer\Modelli amministrativi\Componenti Components\Password sincronizzazione|Queste impostazioni determinano come il sistema gestisce la sincronizzazione delle password tra Windows e sistemi operativi basati su UNIX.<br /><br />Per altre informazioni, vedere [sincronizzazione delle Password](https://technet.microsoft.com/library/cc732609.aspx).|
|**Smart Card**|Computer Configurazione computer\Modelli amministrativi\Componenti Components\Smart carta|Queste impostazioni controllano il modo in cui il sistema gestisce accessi con smart card.<br /><br />|
|**Opzioni di accesso di Windows**|Opzioni di accesso a computer Configurazione computer\Modelli amministrativi\Componenti Windows\Segnalazione|Queste impostazioni dei criteri controllano come e quando sono disponibili le opportunità di accesso.|
|**Opzioni di CTRL + Alt + Canc**|Opzioni computer Configurazione computer\Modelli amministrativi\Componenti Components\Ctrl + Alt + Canc|Queste impostazioni dei criteri interessano l'aspetto di e l'accessibilità per le funzionalità nell'account di accesso dell'interfaccia utente (Desktop sicuro), ad esempio Gestione attività e il blocco della tastiera del computer.|
|**Accesso**|Computer Configurazione computer\Modelli amministrativi\Componenti Components\Logon|Queste impostazioni determinano se o i processi che possono essere eseguite quando l'utente accede.|

## <a name="see-also"></a>Vedere anche
[Panoramica tecnica di autenticazione di Windows](windows-authentication-technical-overview.md)


