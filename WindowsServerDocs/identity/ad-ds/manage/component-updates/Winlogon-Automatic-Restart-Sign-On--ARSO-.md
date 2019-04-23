---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Accesso automatico al riavvio Winlogon
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4024a00c6c186aa929e88cb2aa86b0ec04a731b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883622"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Accesso automatico al riavvio Winlogon

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autore**: Justin. Turner, Senior Support Escalation Engineer presso il gruppo di Windows

> [!NOTE]
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.

## <a name="overview"></a>Panoramica
Windows 8 è stato introdotto App schermata di blocco.  Si tratta di applicazioni che eseguono e visualizzare le notifiche quando la sessione dell'utente è bloccata (calendario degli appuntamenti, messaggio di posta elettronica e i messaggi e così via).  Per visualizzare queste notifiche schermata di blocco al riavvio dei dispositivi che vengono riavviati a causa del processo di Windows Update non riesce.  Alcuni utenti dipendono da queste applicazioni schermata di blocco.

## <a name="whats-changed"></a>Modifiche apportate
Quando un utente accede in un dispositivo Windows 8.1, LSA salverà le credenziali dell'utente in memoria crittografata accessibile solo da lsass.exe. Quando Windows Update attiva un riavvio automatico senza presenza utente, queste credenziali verranno utilizzate per configurare l'accesso automatico per l'utente. Aggiornamento di Windows in esecuzione come sistema con privilegi TCB avvierà la chiamata RPC per eseguire questa operazione.

Dopo il riavvio, l'utente verrà automaticamente eseguito l'accesso tramite il meccanismo di accesso automatico e inoltre è bloccata per proteggere la sessione dell'utente. Verrà avviato il blocco tramite Winlogon mentre la gestione delle credenziali avviene tramite LSA.  Effettua l'accesso automaticamente e bloccare l'utente della console, alle applicazioni schermata di blocco dell'utente sarà riavviate e disponibili.

> [!NOTE]
> Dopo un aggiornamento di Windows indotta riavvio, l'ultimo utente interattivo viene eseguito automaticamente l'accesso e la sessione è bloccata in modo possono eseguire le app schermata di blocco dell'utente.

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**Panoramica rapida**

-   Aggiornamento di Windows richiede riavvio

-   È possibile riavviare computer (*Nessuna App in esecuzione che comporterebbe la perdita di dati*)?

    -   Riavvia automaticamente

    -   Eseguire nuovamente l'accesso

    -   Computer di blocco

-   Attivata o disattivata da criteri di gruppo

    -   Disabilitato per impostazione predefinita nel server di SKU

-   Perché?

    -   Alcuni aggiornamenti non può essere completata fino a quando l'utente accede nuovamente.

    -   Una migliore esperienza utente: non è necessario attendere 15 minuti completare l'installazione di aggiornamenti

-   In che modo? AutoLogon

    -   Consente di archiviare password e Usa le credenziali per l'accesso

    -   Salva credenziali come un segreto LSA in memoria di paging

    -   Può essere abilitata solo se è attivato BitLocker

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Criteri di gruppo: Accesso ultimo utente interattivo automaticamente dopo un riavvio avviate dal sistema
In Windows 8.1 o Windows Server 2012 R2, l'accesso automatico dell'utente blocco schermo dopo un riavvio di Windows Update è acconsentire esplicitamente agli SKU di Server e rifiuto per gli SKU di Client.

**Percorso di criteri:** Configurazione computer > Criteri > modelli amministrativi > componenti di Windows > opzione di accesso di Windows

**Nome del criterio:** Accesso ultimo utente interattivo automaticamente dopo un riavvio avviate dal sistema

**Supportato in:** Almeno Windows Server 2012 R2, Windows 8.1 o Windows RT 8.1

**Guida/descrizione:**

Questa impostazione criterio controlla se un dispositivo verrà automaticamente Accedi dell'ultimo utente interattivo dopo l'aggiornamento di Windows viene riavviato il sistema.

Se si abilita o non si configura questa impostazione dei criteri, il dispositivo salva in modo sicuro le credenziali dell'utente (inclusi il nome utente, dominio e password crittografata) per configurare Accedi automaticamente dopo il riavvio di un aggiornamento di Windows. Dopo il riavvio di Windows Update, l'utente viene automaticamente eseguito l'accesso aggiuntivo e la sessione venga bloccata automaticamente con tutte le app schermata di blocco configurate per l'utente.

Se si disabilita questa impostazione dei criteri, il dispositivo non archivia le credenziali dell'utente per l'accesso automatico dopo il riavvio di un aggiornamento di Windows. App schermata di blocco degli utenti non vengono riavviate dopo il riavvio del sistema.

**Editor del Registro di sistema**

|Nome valore|Tipo|Dati|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Esempio:**<br /><br />0 (attivato)<br /><br />1 (disabilitato)|

**Percorso del Registro di sistema di criteri:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

**Nome del Registro di sistema:** DisableAutomaticRestartSignOn

Valore: 0 o 1

0 = Enabled

1 = disabilitata

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Risoluzione dei problemi
Quando si blocchi automaticamente WinLogon, traccia dello stato di WinLogon verrà archiviato nel registro eventi di WinLogon.

Viene registrato lo stato di un tentativo di configurazione dell'accesso automatico

-   Se ha esito positivo

    -   Registra come tale

-   Se si tratta di un errore:

    -   Registra l'errore era

-   Quando viene modificato lo stato di BitLocker:

    -   la rimozione di credenziali verrà registrata

        -   Vengono memorizzati nel registro operativo LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivi per cui non riesca l'accesso automatico
Esistono diversi casi in cui non è possibile ottenere un account di accesso utente automatico.  In questa sezione consente di acquisire i noti scenari in cui ciò può verificarsi.

### <a name="user-must-change-password-at-next-login"></a>Cambiamento obbligatorio Password all'accesso successivo
Account di accesso utente può immettere uno stato bloccato quando è necessaria la modifica della password all'accesso successivo.  Ciò può essere rilevata prima del riavvio nella maggior parte dei casi, ma non tutte (ad esempio, la scadenza password può essere raggiunto tra l'arresto e l'accesso successivo.

### <a name="user-account-disabled"></a>Account utente disattivato
Anche se disabilitata, è possibile mantenere una sessione utente esistente.  Riavvio di un account che viene disabilitato può essere rilevato in locale nella maggior parte dei casi in anticipo, a seconda dei criteri di gruppo che potrebbe non essere per gli account di dominio (alcuni domini memorizzata nella cache login scenari lavoro anche se l'account è disabilitato nei controller di dominio).

### <a name="logon-hours-and-parental-controls"></a>Orario di accesso e controllo genitori
L'orario di accesso e controllo genitori possono impedire a viene creata una nuova sessione utente.  Se si verificano durante questo periodo di riavvio, l'utente potrebbe non autorizzato per eseguire l'accesso.  È criteri aggiuntivi che causa blocco o logout come azione di conformità.  Ciò potrebbe essere problematica per molti casi figlio in cui il blocco account può verificarsi tra l'ora letto e di riattivazione, in particolare se la finestra di manutenzione è comunemente durante questo periodo.

## <a name="additional-resources"></a>Risorse aggiuntive
**Tabella SEQ tabella \\ \* ARABIC 3: Glossario ARSO**

|Nome|Definizione|
|--------|--------------|
|Autologon|L'accesso automatico è una funzionalità che è stata presente in Windows per diverse versioni.  È una funzionalità di Windows che include anche strumenti, ad esempio l'accesso automatico per Windows v 3.01 documentata  *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Consente a un singolo utente del dispositivo per l'accesso automatico senza dover immettere le credenziali. Le credenziali configurate e archiviate nel Registro di sistema come un segreto LSA crittografato.|


