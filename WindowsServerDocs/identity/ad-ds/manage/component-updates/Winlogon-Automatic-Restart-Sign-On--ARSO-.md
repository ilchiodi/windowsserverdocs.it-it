---
ms.assetid: cb834273-828a-4141-9387-37dd8270e932
title: Accesso automatico riavvio Sign-On (Winlogon)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 27d4285d34105908555458a95bd70fc04fd2901a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Accesso automatico riavvio Sign-On (Winlogon)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows

> [!NOTE]
> Questo contenuto è stato redatto da un ingegnere del supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano per una spiegazione tecnica più approfondita delle funzionalità e le soluzioni in Windows Server 2012 R2 che solitamente disponibili su TechNet. Tuttavia, non è stata sottoposta stessi passaggi redazionali, in modo che il linguaggio potrebbe sembrare che meno accurato della documentazione che cosa si trova in genere su TechNet.

## <a name="overview"></a>Panoramica
Windows 8 ha introdotto App schermata di blocco.  Queste sono le applicazioni che eseguono e visualizzare le notifiche durante la sessione dell'utente è bloccata (del calendario appuntamenti, e-mail e messaggi, ecc.).  I dispositivi sono è necessario riavviare il processo di aggiornamento di Windows a causa di esito negativo visualizzare queste notifiche della schermata di blocco al riavvio successivo.  Alcuni utenti dipendono da queste applicazioni schermata di blocco.

## <a name="whats-changed"></a>Novità?
Quando un utente accede su un dispositivo Windows 8.1, LSA verrà salvate le credenziali dell'utente in memoria crittografata accessibile solo da lsass.exe. Quando Windows Update avvia un riavvio automatico senza presenza dell'utente, queste credenziali da utilizzare per configurare l'accesso automatico per l'utente. Windows Update viene eseguito come sistema con privilegi TCB verrà avviata la chiamata RPC per eseguire questa operazione.

In il riavvio, l'utente verrà automaticamente essere effettuato l'accesso tramite il meccanismo di accesso automatico e quindi inoltre bloccato per proteggere la sessione dell'utente. Verrà avviato il blocco tramite Winlogon mentre viene eseguita la gestione delle credenziali tramite LSA.  Effettuando l'accesso automaticamente e il blocco dell'utente nella console, applicazioni schermata di blocco dell'utente è riavviate e disponibili.

> [!NOTE]
> Dopo un aggiornamento di Windows indotta dal riavvio, l'ultimo utente interattivo automaticamente assegnato e la sessione è bloccata così possono eseguire App schermata di blocco dell'utente.

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreenApp.gif)

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_LockScreen.gif)

**Breve panoramica**

-   Windows Update richiede il riavvio

-   È possibile riavviare computer (*alcuna App in esecuzione che non perdita di dati*)?

    -   Riavvio per te

    -   Eseguire nuovamente l'accesso

    -   Macchina di blocco

-   Abilitati o disabilitati da criteri di gruppo

    -   Disattivata per impostazione predefinita in server SKU

-   Perché?

    -   Alcuni aggiornamenti non possono finire fino a quando l'utente accede nuovamente.

    -   Ottenere una migliore esperienza utente: non è necessario attendere 15 minuti per gli aggiornamenti completare l'installazione

-   In che modo? Accesso automatico

    -   Archivia password, Usa la credenziale per effettuare l'accesso

    -   Credenziali Salva come un segreto LSA in memoria di paging

    -   Può essere abilitata solo se BitLocker è abilitato

## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Criteri di gruppo: Accesso l'ultimo utente interattivo automaticamente dopo un riavvio richiesto dal sistema
In Windows 8.1 / Windows Server 2012 R2, accesso automatico dell'utente schermata blocco dopo un riavvio di Windows Update per gli SKU Server consente di partecipare e rifiutare esplicitamente per SKU Client.

**Percorso di criteri:** configurazione Computer > Criteri > modelli amministrativi > componenti di Windows > opzione di accesso di Windows

**Nome criterio:** accesso l'ultimo utente interattivo automaticamente dopo un riavvio richiesto dal sistema

**Supportato in:** almeno Windows Server 2012 R2, Windows 8.1 o Windows RT 8.1

**Descrizione/Guida:**

Questa impostazione determina se un dispositivo verrà automaticamente accesso aggiuntivo l'ultimo utente interattivo dopo il riavvio di Windows Update il sistema.

Se abiliti o non si configura questa impostazione di criteri, il dispositivo salva in modo sicuro le credenziali dell'utente (inclusi il nome utente, dominio e password crittografata) per configurare accesso aggiuntivo automatica dopo il riavvio di un aggiornamento di Windows. Dopo il riavvio di Windows Update, l'utente viene automaticamente eseguito l'accesso in e la sessione è bloccata automaticamente con tutte le app schermata di blocco configurate per tale utente.

Se si disabilita questa impostazione di criteri, il dispositivo non archivia le credenziali dell'utente per l'accesso automatico dopo il riavvio di un aggiornamento di Windows. App schermata di blocco degli utenti non vengono riavviate dopo il riavvio del sistema.

**Editor del Registro di sistema**

|Nome del valore|Tipo|Dati|
|--------------|--------|--------|
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Esempio:**<br /><br />0 (abilitato)<br /><br />1 (disabilitata)|

**Percorso del Registro di sistema di criteri:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System

**Tipo:** DWORD

**Nome del Registro di sistema:** DisableAutomaticRestartSignOn

Valore: 0 o 1

0 = abilitata

1 = disattivata

![Winlogon](media/Winlogon-Automatic-Restart-Sign-On--ARSO-/GTR_ADDS_SignInPolicy.gif)

## <a name="troubleshooting"></a>Risoluzione dei problemi
Quando si blocca automaticamente WinLogon, traccia dello stato del WinLogon verrà archiviato nel registro eventi di WinLogon.

Lo stato di un tentativo di configurazione di accesso automatico viene registrato

-   Se ha esito positivo

    -   Di conseguenza registra

-   Se verifica un errore:

    -   Registra l'errore era

-   Quando viene modificato lo stato di BitLocker:

    -   Verrà registrato l'eliminazione delle credenziali

        -   Questi verranno archiviati nel registro operativo LSA.

### <a name="reasons-why-autologon-might-fail"></a>Motivi per cui accesso automatico potrebbe non riuscire
Esistono diversi casi in cui non è possibile ottenere un accesso automatico dell'utente.  Questa sezione è destinata per acquisire gli scenari noti in cui ciò può verificarsi.

### <a name="user-must-change-password-at-next-login"></a>Cambiamento obbligatorio Password all'accesso successivo
L'accesso dell'utente può immettere uno stato bloccato quando è necessario modificare la password all'accesso successivo.  Questo può essere rilevato prima del riavvio nella maggior parte dei casi, ma non tutti (ad esempio, può essere raggiunto scadenza password tra l'arresto e l'accesso successivo.

### <a name="user-account-disabled"></a>Account utente disattivato
Anche se disabilitata, è possibile mantenere una sessione utente esistente.  Riavvio di un account che è disabilitato può essere rilevato in locale nella maggior parte dei casi in anticipo, a seconda dei criteri di gruppo che potrebbe non essere per gli account di dominio (alcuni dominio memorizzate nella cache lavoro scenari di accesso anche se l'account è disabilitato nei controller di dominio).

### <a name="logon-hours-and-parental-controls"></a>Orario di accesso e controllo genitori
L'orario di accesso e i genitori possono impedire la viene creata una nuova sessione utente.  Se si verificano durante questa finestra di un riavvio, all'utente non essere consentito per l'accesso.  Esiste criteri aggiuntivi che causa la disconnessione come un'azione di conformità o blocco.  Questo potrebbe essere problematico per molti casi figlio in cui si verifichi blocco account tra l'ora letto e riattivazione, in particolare se la finestra di manutenzione è comunemente durante questo periodo.

## <a name="additional-resources"></a>Risorse aggiuntive
**Tabella SEQ tabella \ \ \ * ARABIC 3: glossario di Winlogon**

|Termine|Definizione|
|--------|--------------|
|Accesso automatico|Accesso automatico è una funzionalità che non è stata presente in Windows per le versioni diverse.  È una funzionalità di Windows che include anche gli strumenti, ad esempio l'accesso automatico per Windows v 3.01 documentata *[http:/technet.microsoft.com/sysinternals/bb963905.aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Consente a un singolo utente del dispositivo di accedere automaticamente senza immettere le credenziali. Le credenziali configurate e archiviate nel Registro di sistema come un segreto LSA crittografato.|


