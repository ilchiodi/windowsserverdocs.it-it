---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Accesso automatico a Winlogon (al riavvio Winlogon)
description: Il modo in cui il riavvio automatico di Windows è in grado di migliorare la produttività degli utenti.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.reviewer: cahick
ms.date: 08/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56f485491340b3974d8bf5ba697c6cf01f3e56ac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868211"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Accesso automatico a Winlogon (al riavvio Winlogon)

Durante una Windows Update, è necessario che siano presenti processi specifici dell'utente per completare l'aggiornamento. Questi processi richiedono che l'utente sia connesso al dispositivo. Al primo accesso dopo l'avvio di un aggiornamento, gli utenti devono attendere il completamento di questi processi specifici dell'utente prima di poter iniziare a usare il dispositivo.

## <a name="how-does-it-work"></a>Come funziona?

Quando Windows Update avvia un riavvio automatico, al riavvio Winlogon estrae le credenziali derivate dell'utente attualmente connesso, le salva in modo permanente su disco e configura il logo per l'utente. Windows Update eseguito come System con privilegi TCB avvierà la chiamata RPC a tale scopo.

Dopo il riavvio finale del Windows Update, l'utente verrà automaticamente connesso tramite il meccanismo autologo e la sessione dell'utente viene riattivata con i segreti salvati in modo permanente. Inoltre, il dispositivo è bloccato per proteggere la sessione dell'utente. Il blocco verrà avviato tramite Winlogon, mentre la gestione delle credenziali viene eseguita dall'autorità di protezione locale (LSA). Dopo aver eseguito correttamente la configurazione e l'accesso a al riavvio Winlogon, le credenziali salvate vengono eliminate immediatamente dal disco.

Eseguendo automaticamente l'accesso e il blocco dell'utente nella console, Windows Update possibile completare i processi specifici dell'utente prima che l'utente torni al dispositivo. In questo modo, l'utente può iniziare immediatamente a usare il dispositivo.

AL riavvio Winlogon considera i dispositivi gestiti e non gestiti in modo diverso. Per i dispositivi non gestiti, viene usata la crittografia del dispositivo, ma non è necessario che l'utente ottenga al riavvio Winlogon. Per i dispositivi gestiti, TPM 2,0, SecureBoot e BitLocker sono necessari per la configurazione di al riavvio Winlogon. Gli amministratori IT possono eseguire l'override di questo requisito tramite Criteri di gruppo. AL riavvio Winlogon per i dispositivi gestiti è attualmente disponibile solo per i dispositivi aggiunti a Azure Active Directory.

|   | Windows Update| arresto-g-t 0  | Riavvii avviati dall'utente | API con flag SHUTDOWN_ARSO/EWX_ARSO |
| --- | :---: | :---: | :---: | :---: |
| Dispositivi gestiti | :heavy_check_mark:  | :heavy_check_mark: |   | :heavy_check_mark: |
| Dispositivi non gestiti | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

> [!NOTE]
> Dopo il riavvio di un Windows Update indotto, l'ultimo utente interattivo viene automaticamente connesso e la sessione è bloccata. In questo modo, le app per la schermata di blocco di un utente possono continuare a funzionare nonostante il riavvio del Windows Update.

![Pagina delle impostazioni](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-lockscreenapp.png)

## <a name="policy-1"></a>#1 di criteri

### <a name="sign-in-and-lock-last-interactive-user-automatically-after-a-restart"></a>Accesso e blocco dell'ultimo utente interattivo automaticamente dopo un riavvio

In Windows 10, al riavvio Winlogon è disabilitato per gli SKU del server ed escludere gli SKU dei client.

**Percorso criteri di gruppo:** Configurazione computer > Modelli amministrativi > componenti di Windows > Opzioni di accesso a Windows

**Criteri di Intune:**

- Piattaforma: Windows 10 e versioni successive
- Tipo di profilo: Modelli amministrativi
- Percorso: \Windows opzioni di accesso Windows\windows

**Supportato in:** Almeno Windows 10 versione 1903

**Descrizione:**

Questa impostazione dei criteri controlla se un dispositivo effettuerà automaticamente l'accesso e bloccherà l'ultimo utente interattivo dopo il riavvio del sistema o dopo un arresto e l'avvio a freddo.

Questa situazione si verifica solo se l'ultimo utente interattivo non è stato dismesso prima del riavvio o dell'arresto.

Se il dispositivo è stato aggiunto a Active Directory o Azure Active Directory, questo criterio si applica solo ai riavvii Windows Update. In caso contrario, verrà applicato sia ai riavvii Windows Update sia ai riavvii e agli arresti avviati dall'utente.

Se questa impostazione di criteri non è configurata, è abilitata per impostazione predefinita. Quando il criterio è abilitato, l'utente viene automaticamente connesso e la sessione viene bloccata automaticamente con tutte le app della schermata di blocco configurate per tale utente dopo l'avvio del dispositivo.

Dopo aver abilitato questo criterio, è possibile configurarne le impostazioni tramite il criterio ConfigAutomaticRestartSignOn, che configura la modalità di accesso automatico e blocco dell'ultimo utente interattivo dopo un riavvio o un avvio a freddo.

Se si disabilita questa impostazione di criteri, il dispositivo non configura l'accesso automatico. Le app della schermata di blocco dell'utente non vengono riavviate dopo il riavvio del sistema.

**Editor del registro di sistema:**

| Nome valore | Type | Data |
| --- | --- | --- |
| DisableAutomaticRestartSignOn | DWORD | 0 (Abilita al riavvio Winlogon) |
|   |   | 1 (Disabilita al riavvio Winlogon) |

**Percorso del registro di sistema dei criteri:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/gtr-adds-signinpolicy.png)

## <a name="policy-2"></a>#2 di criteri

### <a name="configure-the-mode-of-automatically-signing-in-and-locking-last-interactive-user-after-a-restart-or-cold-boot"></a>Configurare la modalità di accesso e blocco automatico dell'ultimo utente interattivo dopo un riavvio o un avvio a freddo

**Percorso criteri di gruppo:** Configurazione computer > Modelli amministrativi > componenti di Windows > Opzioni di accesso a Windows

**Criteri di Intune:**

- Piattaforma: Windows 10 e versioni successive
- Tipo di profilo: Modelli amministrativi
- Percorso: \Windows opzioni di accesso Windows\windows

**Supportato in:** Almeno Windows 10 versione 1903

**Descrizione:**

Questa impostazione dei criteri controlla la configurazione con cui si verifica un riavvio e un blocco automatici dopo un riavvio o un avvio a freddo. Se è stato scelto "disabilitato" nei criteri "Accedi e blocca ultimo utente interattivo automaticamente dopo un riavvio", l'accesso automatico non verrà eseguito e non sarà necessario configurare questo criterio.

Se si abilita questa impostazione di criteri, è possibile scegliere una delle due opzioni seguenti:

1. "Abilitato se BitLocker è attivato e non sospeso" indica che l'accesso automatico e il blocco si verificano solo se BitLocker è attivo e non è sospeso durante il riavvio o l'arresto. È possibile accedere ai dati personali sul disco rigido del dispositivo in questo momento se BitLocker non è acceso o sospeso durante un aggiornamento. La sospensione di BitLocker rimuove temporaneamente la protezione per i componenti di sistema e i dati, ma potrebbe essere necessaria in determinate circostanze per aggiornare correttamente i componenti critici per l'avvio.
   - BitLocker viene sospeso durante gli aggiornamenti se:
      - Il dispositivo non ha TPM 2,0 e PCR7 oppure
      - Il dispositivo non usa una protezione con solo TPM
2. "Always Enabled" indica che l'accesso automatico si verificherà anche se BitLocker è spento o sospeso durante il riavvio o l'arresto. Quando BitLocker non è abilitato, i dati personali sono accessibili sul disco rigido. Il riavvio automatico e l'accesso devono essere eseguiti solo in questa condizione se si è certi che il dispositivo configurato si trovi in una posizione fisica sicura.

Se si disattiva o non si configura questa impostazione, l'accesso automatico verrà impostato automaticamente sul comportamento "abilitato se BitLocker è acceso e non sospeso".

**Editor del registro di sistema**

| Nome valore | Type | Data |
| --- | --- | --- |
| AutomaticRestartSignOnConfig | DWORD | 0 (Abilita al riavvio Winlogon se protetto) |
|   |   | 1 (Abilita sempre al riavvio Winlogon) |

**Percorso del registro di sistema dei criteri:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/arso-policy-setting.png)

## <a name="troubleshooting"></a>Risoluzione dei problemi

Quando WinLogon si blocca automaticamente, la traccia dello stato di WinLogon verrà archiviata nel registro eventi di WinLogon.

Lo stato di un tentativo di configurazione con logo automatico viene registrato

- In caso di esito positivo
   - registra il nome
- Se si verifica un errore:
   - registra l'errore
- Quando lo stato di BitLocker cambia:
   - la rimozione delle credenziali verrà registrata
   - Questi verranno archiviati nel log operativo di LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivi per cui l'accesso automatico potrebbe non riuscire

Esistono diversi casi in cui non è possibile ottenere un account di accesso automatico dell'utente.  Questa sezione ha lo scopo di acquisire gli scenari noti in cui questo può verificarsi.

### <a name="user-must-change-password-at-next-login"></a>L'utente deve modificare la password all'accesso successivo

L'accesso utente può entrare in uno stato bloccato quando è necessario modificare la password all'accesso successivo.  Questa operazione può essere rilevata prima del riavvio nella maggior parte dei casi, ma non tutti (ad esempio, la scadenza della password può essere raggiunta tra l'arresto e l'accesso successivo.

### <a name="user-account-disabled"></a>Account utente disabilitato

Una sessione utente esistente può essere mantenuta anche se disabilitata.  Il riavvio di un account disabilitato può essere rilevato localmente nella maggior parte dei casi, a seconda del GP, potrebbe non essere per gli account di dominio (alcuni scenari di accesso memorizzati nella cache del dominio funzionano anche se l'account è disabilitato nel controller di dominio).

### <a name="logon-hours-and-parental-controls"></a>Orari di accesso e controlli padre

Le ore di accesso e i controlli padre possono impedire la creazione di una nuova sessione utente.  Se si verifica un riavvio durante questa finestra, l'utente non sarà autorizzato a eseguire l'accesso.  Sono presenti criteri aggiuntivi che causano il blocco o la disconnessione come azione di conformità. Lo stato di un tentativo di configurazione dell'accesso automatico viene registrato.

## <a name="security-details"></a>Dettagli sulla sicurezza

### <a name="credentials-stored"></a>Credenziali archiviate

|   | Hash password | Chiave credenziali | Ticket di concessione ticket | Token di aggiornamento primario |
| --- | :---: | :---: | :---: | :---: |
| Account locale | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Account MSA | :heavy_check_mark: | :heavy_check_mark: |   |   |
| Azure AD account aggiunto | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark: (se ibrido) | :heavy_check_mark: |
| Account aggiunto a un dominio | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | : heavy_check_mark: (se ibrido) |

### <a name="credential-guard-interaction"></a>Interazione di Credential Guard

Se per un dispositivo è abilitata la funzionalità Credential Guard, i segreti derivati di un utente vengono crittografati con una chiave specifica per la sessione di avvio corrente. Di conseguenza, al riavvio Winlogon non è attualmente supportato nei dispositivi con Credential Guard abilitato.

## <a name="additional-resources"></a>Risorse aggiuntive

L'accesso automatico è una funzionalità presente in Windows per diverse versioni. Si tratta di una funzionalità documentata di Windows che include anche strumenti come l'accesso automatico per Windows [http:/technet. Microsoft. com/sysinternals/bb963905. aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx). Consente a un singolo utente del dispositivo di accedere automaticamente senza immettere le credenziali. Le credenziali vengono configurate e archiviate nel registro di sistema come segreto LSA crittografato. Questo può essere problematico per molti casi figlio in cui il blocco dell'account può verificarsi tra il tempo di sospensione e il riattivazione, in particolare se la finestra di manutenzione si trova in genere durante questo periodo di tempo.
