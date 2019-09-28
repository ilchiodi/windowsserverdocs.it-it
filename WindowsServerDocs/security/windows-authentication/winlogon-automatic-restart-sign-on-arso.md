---
title: Accesso automatico al riavvio Winlogon
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: na
ms.suite: na
ms.technology: security-auditing
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15cddcfa-8a8e-45e4-bb76-b8e1a14ceac0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f085cf78a01148f97a450577131213ce977a432a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402321"
---
# <a name="winlogon-automatic-restart-sign-on-arso"></a>Accesso automatico al riavvio Winlogon

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Autore**: Justin Turner, Senior Support Escalation Engineer con il gruppo di Windows  
  
> [!NOTE]  
> Questo contenuto è stato redatto da un ingegnere del Supporto tecnico Microsoft ed è destinato ad amministratori esperti e architetti di sistemi che desiderano una spiegazione tecnica delle funzionalità e delle soluzioni relative a Windows Server 2012 R2 più approfondita rispetto agli argomenti solitamente disponibili su TechNet. Non è stato tuttavia sottoposto agli stessi passaggi redazionali e, di conseguenza, per alcune lingue potrebbe essere meno accurato della documentazione che si trova in genere su TechNet.  
  
## <a name="overview"></a>Panoramica  
In Windows 8 sono state introdotte app della schermata di blocco.  Si tratta delle applicazioni che eseguono e visualizzano le notifiche quando la sessione dell'utente è bloccata (appuntamenti del calendario, messaggi di posta elettronica e così via).  I dispositivi che vengono riavviati a causa del processo di Windows Update non riescono a visualizzare le notifiche della schermata di blocco al riavvio.  Alcuni utenti dipendono da queste applicazioni della schermata di blocco.  
  
## <a name="whats-changed"></a>Modifiche apportate  
Quando un utente accede a un dispositivo Windows 8.1, LSA salverà le credenziali utente nella memoria crittografata accessibile solo da Lsass. exe. Quando Windows Update avvia un riavvio automatico senza presenza dell'utente, queste credenziali verranno usate per configurare l'accesso automatico per l'utente. Windows Update eseguito come System con privilegi TCB avvierà la chiamata RPC a tale scopo.  
  
Al riavvio, l'utente verrà automaticamente connesso tramite il meccanismo autologo e quindi bloccato per proteggere la sessione dell'utente. Il blocco verrà avviato tramite Winlogon, mentre la gestione delle credenziali viene eseguita da LSA.  Eseguendo automaticamente l'accesso e il blocco dell'utente nella console, le applicazioni della schermata di blocco dell'utente verranno riavviate e disponibili.  
  
> [!NOTE]  
> Dopo il riavvio di un Windows Update indotto, l'ultimo utente interattivo viene automaticamente connesso e la sessione è bloccata in modo da consentire l'esecuzione delle app della schermata di blocco dell'utente.  
  
![Screenshot che mostra la schermata di blocco](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreenApp.gif)  
  
![Screenshot che mostra le app della schermata di blocco](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_LockScreen.gif)  
  
**Panoramica rapida**  
  
-   Windows Update richiede il riavvio  
  
-   Il computer può essere riavviato (*nessuna app in esecuzione che potrebbe perdere dati*)?  
  
    -   Riavvia per te  
  
    -   Accedi di nuovo  
  
    -   Blocca computer  
  
-   Abilitato o disabilitato da Criteri di gruppo  
  
    -   Disabilitato per impostazione predefinita negli SKU del server  
  
-   Perché?  
  
    -   Alcuni aggiornamenti non possono terminare fino a quando l'utente non esegue nuovamente l'accesso.  
  
    -   Migliore esperienza utente: non è necessario attendere 15 minuti per completare l'installazione degli aggiornamenti  
  
-   Come? AutoLogon  
  
    -   Archivia la password, USA le credenziali per l'accesso  
  
    -   Salva le credenziali come segreto LSA nella memoria di paging  
  
    -   Può essere abilitato solo se BitLocker è abilitato  
  
## <a name="group-policy-sign-in-last-interactive-user-automatically-after-a-system-initiated-restart"></a>Criteri di gruppo: Ultimo utente interattivo di accesso automatico dopo un riavvio avviato dal sistema  
In Windows 8.1/Windows Server 2012 R2, l'accesso automatico dell'utente della schermata di blocco dopo un riavvio del Windows Update è acconsentito esplicitamente per gli SKU del server ed escludere gli SKU dei client.  
  
**Percorso criterio:** Configurazione computer > criteri > Modelli amministrativi > componenti di Windows > opzione di accesso a Windows  
  
**Nome criterio:** Ultimo utente interattivo di accesso automatico dopo un riavvio avviato dal sistema  
  
**Supportato in:** Almeno Windows Server 2012 R2, Windows 8.1 o Windows RT 8,1  
  
**Descrizione/guida:**  
  
Questa impostazione dei criteri controlla se un dispositivo effettuerà automaticamente l'accesso all'ultimo utente interattivo dopo che Windows Update il riavvio del sistema.  
  
Se si Abilita o non si configura questa impostazione di criteri, il dispositivo salva in modo sicuro le credenziali dell'utente, inclusi il nome utente, il dominio e la password crittografata, per configurare l'accesso automatico dopo un riavvio del Windows Update. Dopo il riavvio del Windows Update, l'utente viene automaticamente connesso e la sessione viene bloccata automaticamente con tutte le app della schermata di blocco configurate per tale utente.  
  
Se si disabilita questa impostazione di criteri, il dispositivo non archivia le credenziali dell'utente per l'accesso automatico dopo un riavvio del Windows Update. Le app della schermata di blocco degli utenti non vengono riavviate dopo il riavvio del sistema.  
  
**Editor del registro di sistema**  
  
|Nome valore|Type|Data|  
|-------|----|----|  
|DisableAutomaticRestartSignOn|DWORD|0<br /><br />**Esempio:**<br /><br />0 (abilitato)<br /><br />1 (disabilitato)|  
  
**Percorso del registro di sistema dei criteri:** HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System  
  
**Tipo:** DWORD  
  
**Nome del registro di sistema:** DisableAutomaticRestartSignOn  
  
Valore: 0 o 1  
  
0 = abilitato  
  
1 = disabilitato  
  
![Screenshot che illustra l'interfaccia utente dei controlli di impostazione dei criteri, in cui è possibile specificare se un dispositivo effettuerà automaticamente l'accesso all'ultimo utente interattivo dopo che Windows Update riavviato il sistema](../media/winlogon-automatic-restart-sign-on-arso/GTR_ADDS_SignInPolicy.gif)  
  
## <a name="troubleshooting"></a>Risoluzione dei problemi  
Quando WinLogon si blocca automaticamente, la traccia dello stato di WinLogon verrà archiviata nel registro eventi di WinLogon.  
  
Lo stato di un tentativo di configurazione con logo automatico viene registrato  
  
-   In caso di esito positivo  
  
    -   registra il nome  
  
-   Se si verifica un errore:  
  
    -   registra l'errore  
  
-   Quando lo stato di BitLocker cambia:  
  
    -   la rimozione delle credenziali verrà registrata  
  
        -   Questi verranno archiviati nel log operativo di LSA.  
  
### <a name="reasons-why-autologon-might-fail"></a>Motivi per cui l'accesso automatico potrebbe non riuscire  
Esistono diversi casi in cui non è possibile ottenere un account di accesso automatico dell'utente.  Questa sezione ha lo scopo di acquisire gli scenari noti in cui questo può verificarsi.  
  
### <a name="user-must-change-password-at-next-login"></a>L'utente deve modificare la password all'accesso successivo  
L'accesso utente può entrare in uno stato bloccato quando è necessario modificare la password all'accesso successivo.  Questa operazione può essere rilevata prima del riavvio nella maggior parte dei casi, ma non tutti (ad esempio, la scadenza della password può essere raggiunta tra l'arresto e l'accesso successivo.  
  
### <a name="user-account-disabled"></a>Account utente disabilitato  
Una sessione utente esistente può essere mantenuta anche se disabilitata.  Il riavvio di un account disabilitato può essere rilevato localmente nella maggior parte dei casi, a seconda del GP, potrebbe non essere per gli account di dominio (alcuni scenari di accesso memorizzati nella cache del dominio funzionano anche se l'account è disabilitato nel controller di dominio).  
  
### <a name="logon-hours-and-parental-controls"></a>Orari di accesso e controlli padre  
Le ore di accesso e i controlli padre possono impedire la creazione di una nuova sessione utente.  Se si verifica un riavvio durante questa finestra, l'utente non sarà autorizzato a eseguire l'accesso.  Sono presenti criteri aggiuntivi che causano il blocco o la disconnessione come azione di conformità.  Questo può essere problematico per molti casi figlio in cui il blocco dell'account può verificarsi tra il tempo di sospensione e il riattivazione, in particolare se la finestra di manutenzione si trova in genere durante questo periodo di tempo.  
  
## <a name="additional-resources"></a>Risorse aggiuntive  
**Table SEQ Table \\ @ no__t-2 ARABIC 3: Glossario al riavvio Winlogon @ no__t-0  
  
|Nome|Definizione|  
|----|-------|  
|Autologon|L'accesso automatico è una funzionalità presente in Windows per diverse versioni.  Si tratta di una funzionalità documentata di Windows che include anche strumenti come l'accesso automatico per Windows v 3.01  *[http:/technet. Microsoft. com/sysinternals/bb963905. aspx](https://technet.microsoft.com/sysinternals/bb963905.aspx)*<br /><br />Consente a un singolo utente del dispositivo di accedere automaticamente senza immettere le credenziali. Le credenziali vengono configurate e archiviate nel registro di sistema come segreto LSA crittografato.|  
  

