---
title: Impostazioni di criteri di gruppo usate nell'autenticazione di Windows
description: Protezione di Windows Server
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Impostazioni di criteri di gruppo usate nell'autenticazione di Windows

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive l'uso e l'impatto delle impostazioni di criteri di gruppo nel processo di autenticazione.

È possibile gestire l'autenticazione nei sistemi operativi Windows tramite l'aggiunta di utenti, computer e gli account del servizio ai gruppi, quindi applicando criteri di autenticazione a tali gruppi. Questi criteri sono definiti come criteri di sicurezza locali e come modelli amministrativi, noto anche come impostazioni di criteri di gruppo. Entrambi i gruppi possono essere configurati e distribuiti in tutta l'organizzazione tramite criteri di gruppo.

> [!NOTE]
> Le funzionalità introdotte in Windows Server 2012 R2, consentono di configurare criteri di autenticazione per servizi di destinazione o applicazioni, comunemente denominato silo di autenticazione, con gli account protetti. Per informazioni su come eseguire questa operazione in Active Directory, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

Ad esempio, è possibile applicare i seguenti criteri ai gruppi, in base alla loro funzione nell'organizzazione:

-   Accesso localmente o a un dominio

-   Accedere in rete

-   Reimpostare gli account

-   Creare account

Nella tabella seguente elenca i gruppi di criteri rilevanti per l'autenticazione e fornisce collegamenti alla documentazione che consentono di configurare tali criteri.

|Criteri di gruppo|Posizione|Descrizione|
|--------|------|--------|
|**Criteri password**|Criteri del Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri|Criteri password riguardano le caratteristiche e il comportamento delle password. Criteri password sono utilizzati per gli account di dominio o account utente locali. E determinano le impostazioni per le password, ad esempio sulla durata e l'imposizione.<br /><br />Per informazioni sulle impostazioni specifiche, vedere [criteri Password](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Criterio di blocco account**|Criteri del Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri|Opzioni dei criteri di blocco account disattivano account dopo un determinato numero di tentativi di accesso non riuscito. Utilizzo di queste opzioni consentono di rilevare e bloccare i tentativi di violazione delle password.<br /><br />Per informazioni sulle opzioni dei criteri di blocco account, vedere [criterio di blocco Account](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Criteri Kerberos**|Criteri del Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri|Le impostazioni relative a Kerberos includono le regole di durata e l'imposizione di ticket. Poiché il protocollo di autenticazione Kerberos non viene usato per autenticare gli account locali, criteri Kerberos non si applicano ai database di account locale. Di conseguenza, le impostazioni dei criteri Kerberos è configurabile solo tramite il dominio predefinito oggetto (criteri di gruppo), in cui influisce sugli accessi al dominio.<br /><br />Per informazioni sulle opzioni di criteri Kerberos per il controller di dominio, vedere [criteri Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Criteri di controllo**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri Locali\criteri|Criteri di controllo consentono di controllare e comprendere l'accesso agli oggetti, ad esempio file e cartelle e per gestire gli account utente e gruppo e gli accessi degli utenti e disconnessione. Criteri di controllo è possono specificare le categorie di eventi che si desidera controllare, impostare le dimensioni e il comportamento del Registro di protezione e determinare quali oggetti si desidera monitorare l'accesso e il tipo di accesso a cui si desidera monitorare.<br /><br />|
|**Assegnazione diritti utente**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni sicurezza\Criteri Locali\assegnazione diritti|Diritti utente vengono in genere assegnati in base a gruppi di sicurezza a cui appartiene un utente, ad esempio gli utenti, Power Users o Administrators. Le impostazioni dei criteri di questa categoria sono in genere utilizzate per concedere o negare l'autorizzazione per accedere a un computer in base al metodo di appartenenza al gruppo di sicurezza e l'accesso.|
|**Opzioni di sicurezza**|Computer locale locale\Configurazione computer\Impostazioni Windows\Impostazioni protezione\Criteri Locali\opzioni|Criteri relativi all'autenticazione includono:<br /><br />-Dispositivi<br />-Controller di dominio<br />-Controller di dominio<br />-Accesso interattivo<br />-Server di rete Microsoft<br />-Accesso alla rete<br />-Sicurezza di rete<br />-Console di ripristino<br />-Arresto del sistema<br /><br />|
|**Delega delle credenziali**|Computer Configurazione computer\Modelli amministrativi\sistema\delega delega|La delega delle credenziali è un meccanismo che consente di utilizzare in altri sistemi, in particolare i server membri e i controller di dominio all'interno di un dominio credenziali locali. Queste impostazioni si applicano alle applicazioni tramite Credential Security Support Provider (SSP credenziali). Connessione Desktop remoto è un esempio.|
|**KDC**|Computer Configurazione computer\Modelli Templates\System\KDC|Queste impostazioni dei criteri influiscono su come il centro distribuzione chiavi (KDC), che è un servizio nel controller di dominio, gestisce le richieste di autenticazione Kerberos.|
|**Kerberos**|Computer Configurazione computer\Modelli Templates\System\Kerberos|Queste impostazioni dei criteri influiscono su come Kerberos è configurata per gestire il supporto di attestazioni, la blindatura Kerberos, autenticazione composta, che identifica i server proxy e altre configurazioni.|
|**Accesso**|Configurazione computer\Modelli amministrativi\Sistema\Accesso|Queste impostazioni dei criteri di controllo come il sistema visualizza l'esperienza di accesso degli utenti.|
|**Accesso rete**|Configurazione computer\Modelli Templates\System\Net accesso al computer|Queste impostazioni dei criteri di controllo come il sistema gestisce le richieste di accesso di rete tra cui come si comporta il localizzatore di Controller di dominio.<br /><br />Per ulteriori informazioni sul modo in cui si inserisce il localizzatore di Controller di dominio nei processi di replica, vedere [informazioni sulla replica tra siti](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biometria**|Computer Configurazione computer\Modelli amministrativi\Componenti Components\Biometrics|Queste impostazioni dei criteri, in genere, consentono o negare l'uso della biometria come metodo di autenticazione.<br /><br />Per informazioni sull'implementazione della biometria Windows, vedere Panoramica di Windows Biometric Framework.|
|**Interfaccia utente delle credenziali**|Computer Configurazione computer\Modelli amministrativi\Componenti di Windows\interfaccia utente|Queste impostazioni dei criteri controllano Gestione credenziali nel punto di ingresso.|
|**Sincronizzazione password**|Sincronizzazione di computer Configurazione computer\Modelli amministrativi\Componenti Components\Password|Queste impostazioni dei criteri di determinare come il sistema gestisce la sincronizzazione delle password tra Windows e sistemi operativi basati su UNIX.<br /><br />Per ulteriori informazioni, vedere [la sincronizzazione delle Password](https://technet.microsoft.com/library/cc732609.aspx).|
|**Smart Card**|Scheda Configurazione computer\Modelli amministrativi\Componenti Components\Smart|Queste impostazioni dei criteri di controllo come il sistema gestisce accessi con smart card.<br /><br />|
|**Opzioni di accesso di Windows**|Opzioni di accesso a computer Configurazione computer\Modelli amministrativi\Componenti Windows\Windows|Queste impostazioni dei criteri di controllo quando e come opportunità di accesso sono disponibili.|
|**Opzioni di CTRL + Alt + Canc**|Opzioni di computer Configurazione computer\Modelli amministrativi\Componenti Components\Ctrl + Alt + Canc|Queste impostazioni dei criteri modificare l'aspetto di e l'accesso facilitato alle funzionalità di accesso dell'interfaccia utente (Desktop sicuro), ad esempio Gestione attività e il blocco della tastiera del computer.|
|**Accesso**|Computer Configurazione computer\Modelli amministrativi\Componenti Components\Logon|Queste impostazioni dei criteri determinano se è possono eseguire i processi quando l'utente accede.|

## <a name="see-also"></a>Vedere anche
[Panoramica tecnica di autenticazione di Windows](windows-authentication-technical-overview.md)


