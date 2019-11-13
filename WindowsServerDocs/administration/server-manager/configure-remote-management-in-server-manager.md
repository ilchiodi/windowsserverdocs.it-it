---
title: Configurare la gestione remota in Server Manager
description: Server Manager
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e1058a5679f73fcd2ceb8586da687158762d10f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383207"
---
# <a name="configure-remote-management-in-server-manager"></a>Configurare la gestione remota in Server Manager

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server, è possibile utilizzare Server Manager per eseguire attività di gestione su server remoti. la gestione remota è abilitata per impostazione predefinita nei server che eseguono Windows Server 2016. Per gestire un server in modalità remota tramite Server Manager, si aggiunge il server al pool di server di gestione Server.

È possibile utilizzare Server Manager per gestire i server remoti che eseguono versioni precedenti di Windows Server, ma i seguenti aggiornamenti sono necessari per gestire completamente questi sistemi operativi meno recenti.

Per gestire i server che eseguono versioni di Windows Server precedente a Windows Server 2016, installare i seguenti aggiornamenti software e per semplificare le versioni precedenti di Windows Server tramite Server Manager in Windows Server 2016.

|Sistema operativo|Software necessario|Gestibilità|
|----------|-----------|---------|
| Windows Server 2012 R2 o Windows Server 2012 |-   [.NET Framework 4,6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5,0](https://go.microsoft.com/fwlink/?LinkID=395058). Il pacchetto di download di Windows Management Framework 5.0 Aggiorna provider Strumentazione gestione Windows (WMI) su Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con stato gestibilità **non accessibile**.<br />-L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) non è più necessario nei server che eseguono Windows Server 2012 R2 o Windows Server 2012.||
| Windows Server 2008 R2 |-   [.NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881). Il pacchetto di download di Windows Management Framework 4.0 Aggiorna provider Strumentazione gestione Windows (WMI) in Windows Server 2008 R2. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2008 R2 con stato gestibilità **non accessibile**.<br />-L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) consente di raccogliere dati sulle prestazioni da Windows Server 2008 R2 Server Manager.||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows management Framework 3,0 scaricare i provider degli aggiornamenti del pacchetto Strumentazione gestione Windows (WMI) in windows Server 2008. Il provider WMI aggiornati consentono a Server Manager di raccogliere informazioni sui ruoli e funzionalità installati sui server gestiti. Fino a quando non viene applicato l'aggiornamento, i server che eseguono Windows Server 2008 hanno uno stato di gestibilità **non accessibile: verificare che le versioni precedenti eseguano Windows Management Framework 3,0**.<br />-L'aggiornamento delle prestazioni associati [articolo della Knowledge Base 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) consente di raccogliere dati sulle prestazioni da Windows Server 2008 Server Manager.||

per informazioni dettagliate su come aggiungere server in gruppi di lavoro da gestire o gestire server remoti da un computer del gruppo di lavoro che esegue Server Manager, vedere [aggiungere server a Server Manager](add-servers-to-server-manager.md).

## <a name="enabling-or-disabling-remote-management"></a>Abilitare o disabilitare la gestione remota
In Windows Server 2016, gestione remota è abilitata per impostazione predefinita. Prima di connettersi a un computer che esegue Windows Server 2016 in modalità remota tramite Server Manager, gestione remota di Server Manager deve abilitata nel computer di destinazione se è stata disabilitata. Le procedure illustrate in questa sezione descrivono come disabilitare la gestione remota e come riabilitarla qualora sia stata disabilitata. Nella console di Server Manager, lo stato di gestione remota del server locale viene visualizzato nel **proprietà** area di **Server locale** pagina.

Gli account degli amministratori locali diversi dall'account Administrator integrato potrebbero non disporre dei diritti necessari alla gestione di un server da una postazione remota, anche se la gestione remota è abilitata. L'impostazione del registro di sistema **LocalAccountTokenFilterPolicy** per il controllo dell'account utente remoto deve essere configurata in modo da consentire agli account locali del gruppo Administrators diversi dall'account predefinito Administrator di gestire il server in modalità remota.

In Windows Server 2016, Server Manager si basa su gestione remota Windows (WinRM) e DCOM (Distributed Component Object Model) per le comunicazioni remote. Le impostazioni controllate dalla finestra di dialogo **Configura gestione remota** influiscono solo sulle parti di Server Manager e Windows PowerShell che utilizzano WinRM per le comunicazioni remote. Tali estensioni non influiscono sulle parti di Server Manager che utilizzano DCOM per le comunicazioni remote. Ad esempio, Server Manager utilizza WinRM per comunicare con i server remoti che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, ma utilizza DCOM per comunicare con server che eseguono Windows Server 2008 e Windows Server 2008 R2, ma non si dispone di [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881) o [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) aggiornamenti applicati. Microsoft Management Console (MMC) e altri strumenti di gestione legacy utilizzano DCOM. Per ulteriori informazioni su come modificare queste impostazioni, vedere [per configurare MMC o un altro strumento di gestione remota su DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom) in questo argomento.

> [!NOTE]
> Le procedure descritte in questa sezione possono essere completate solo in computer che eseguono Windows Server. Impossibile abilitare o disabilitare la gestione remota in un computer che esegue Windows 10 tramite queste procedure, perché il sistema operativo client non possono essere gestito tramite Server Manager.

-   Per abilitare la gestione remota WinRM, scegliere una delle procedure seguenti.

    -   [Per abilitare la gestione remota di Server Manager tramite l'interfaccia di Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [Per abilitare la gestione remota Server Manager tramite Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [Per abilitare la gestione remota Server Manager tramite la riga di comando](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [Per abilitare Server Manager e la gestione remota di Windows PowerShell nelle versioni precedenti di Windows Server](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   Per disabilitare la gestione remota WinRM e Server Manager, selezionare una delle seguenti procedure.

    -   [Per disabilitare la gestione remota utilizzando Criteri di gruppo](#to-disable-remote-management-by-using-group-policy)

    -   [Per disabilitare la gestione remota utilizzando un file di risposte durante l'installazione automatica](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   Per configurare la gestione remota DCOM, vedere [To configure DCOM remote management](#to-configure-mmc-or-other-tool-remote-management-over-dcom).

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>Per abilitare la gestione remota di Server Manager usando l'interfaccia di Windows

1.  > [!NOTE]
    > Le impostazioni controllate dalla finestra di dialogo **Configura gestione remota** non interessano parti di Server Manager che utilizzano DCOM per le comunicazioni remote.

    Nel computer che si desidera gestire in remoto, aprire Server Manager, se non è già aperta. Sulla barra delle applicazioni di Windows fare clic su **Server Manager**. Nella schermata **Start** fare clic sul riquadro **Server Manager** .

2.  Nell'area **Proprietà** della pagina **server locali** fare clic sul valore con collegamento ipertestuale per la proprietà **gestione remota** .

3.  Eseguire una delle operazioni seguenti, quindi fare clic su **OK**.

    -   Per impedire che questo computer venga gestito in modalità remota tramite Server Manager (o Windows PowerShell se è installato), deselezionare la casella di controllo **Abilita gestione remota del server da altri computer** .

    -   Per consentire a questo computer di gestione remota tramite Server Manager o Windows PowerShell, selezionare **Consenti gestione remota del server da altri computer**.

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>Per abilitare la gestione remota di Server Manager tramite Windows PowerShell

1.  Nel computer che si desidera gestire in remoto, eseguire una delle seguenti operazioni per aprire una sessione di Windows PowerShell con diritti utente elevati.

    -   Nel desktop di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell** nella barra delle applicazioni e scegliere **Esegui come amministratore**.

    -   Nella schermata **Start** di Windows fare clic con il pulsante destro del mouse su **Windows PowerShell**e quindi, nella barra dell'app, fare clic su **Esegui come amministratore**.

2.  digitare quanto segue, quindi premere **invio** per abilitare tutte le eccezioni alle regole firewall necessarie.

    **Configure-SMRemoting. exe-Abilita**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>Per abilitare la gestione remota di Server Manager tramite la riga di comando

1.  Nel computer che si desidera gestire da postazione remota aprire una sessione di prompt dei comandi con diritti utente elevati. A tale scopo, nella schermata **Start** Digitare **cmd**, fare clic con il pulsante destro del mouse sul riquadro **prompt dei comandi** quando viene visualizzato nei risultati di **app** e quindi, nella barra dell'app, fare clic su **Esegui come amministratore**.

2.  Eseguire il seguente file eseguibile.

    **%windir%\system32\Configure-SMremoting.exe**

3.  Effettua una delle seguenti operazioni:

    -   Per disabilitare la gestione remota, digitare **Configure-SMRemoting. exe-disable**, quindi premere **invio**.

    -   Per abilitare la gestione remota, digitare **Configure-SMRemoting. exe-enable**, quindi premere **invio**.

    -   Per visualizzare l'impostazione di gestione remota corrente, digitare **Configure-SMRemoting. exe-get**, quindi premere INVIO.

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>Per abilitare la gestione remota di Server Manager e Windows PowerShell in versioni precedenti di Windows Server

-   Effettua una delle seguenti operazioni:

    -   Per abilitare la gestione remota sui server che eseguono Windows Server 2012, vedere [per abilitare la gestione remota di Server Manager tramite l'interfaccia di Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) in questo argomento.

    -   Per abilitare la gestione remota nei server che eseguono Windows Server 2008 R2, vedere [gestione remota con Server Manager](https://go.microsoft.com/fwlink/?LinkID=137378) nella Guida di windows Server 2008 R2.

    -   Per abilitare la gestione remota nei server che eseguono Windows Server 2008, vedere [abilitare e usare i comandi remoti in Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>Per configurare MMC o un altro strumento di gestione remota su DCOM

1.  Eseguire una delle operazioni seguenti per aprire lo snap-in Windows Firewall con sicurezza avanzata.

    -   Nel **proprietà** area di **Server locale** in Server Manager fare clic sul valore del collegamento ipertestuale per la **Windows Firewall** proprietà e quindi fare clic su **Impostazioni avanzate**.

    -   Nella schermata **Start** digitare **WF. msc**, quindi fare clic sul riquadro dello snap-in quando viene visualizzato nei risultati di **app** .

2.  Nel riquadro dell'albero selezionare **Regole in entrata**.

3.  Verificare che le eccezioni alle regole del firewall seguenti siano abilitate e che non siano state disabilitate da Criteri di gruppo impostazioni. Se qualcuna non è abilitata, andare al passaggio successivo.

    -   Accesso alla rete COM+ (DCOM-In)

    -   Gestione remota registro eventi (NP-in)

    -   Gestione remota registro eventi (RPC)

    -   Gestione remota registro eventi (RPC-EPMAP)

4.  Fare clic con il pulsante destro del mouse sulle regole che non sono state abilitate, quindi scegliere **Abilita regola** dal menu di scelta rapida.

5.  Chiudere lo snap-in Windows Firewall con sicurezza avanzata.

### <a name="to-disable-remote-management-by-using-group-policy"></a>Per disabilitare la gestione remota tramite Criteri di gruppo

1.  Per aprire l'editor di Criteri di gruppo locale, eseguire una delle operazioni seguenti.

    -   In un server che esegue Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, nella schermata **Start** Digitare **gpedit. msc**, quindi fare clic sul riquadro **gpedit** quando viene visualizzato.

    -   In un server che esegue Windows Server 2008 R2 o Windows Server 2008, nel **eseguire** la finestra di dialogo, digitare **gpedit. msc**, quindi premere **INVIO**.

2.  Aprire **Configurazione computer\Modelli amministrativi\Componenti di Windows\Windows Remote Management (WinRM) \Servizio Service**.

3.  Nel riquadro del contenuto, fare doppio clic su **Consenti gestione del server remota tramite Gestione remota Windows**.

4.  Nella finestra di dialogo per l'impostazione criteri **Consenti gestione del server remota tramite Gestione remota Windows** selezionare **Disabilitato** per disabilitare la gestione remota. Fare clic su **OK** per salvare le modifiche e chiudere la finestra di dialogo delle impostazioni criteri.

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>Per disabilitare la gestione remota tramite un file di risposte durante l'installazione automatica

1.  creare un file di risposte per l'installazione automatica per le installazioni di Windows Server 2016 usando Windows System Image Manager (Windows SIM). Per altre informazioni sulla creazione di un file di risposte e sull'uso di Windows SIM, vedere [Che cos'è Windows System Image Manager?](https://technet.microsoft.com/library/cc766347.aspx) e [Guida dettagliata - Distribuzione Windows di base per i professionisti IT](https://technet.microsoft.com/library/dd349348.aspx).

2.  Nel file di risposte individuare l'impostazione **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**.

3.  Per disabilitare Server Manager gestione remota per impostazione predefinita in tutti i server a cui si desidera applicare il file di risposte, impostare **Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement** su **false**.

    > [!NOTE]
    > Questa impostazione consente di disabilitare la gestione remota nel corso del processo di installazione del sistema operativo. Configurazione di questa impostazione non impedisce un amministratore di abilitare la gestione remota di Server Manager in un server al termine dell'installazione del sistema operativo. Gli amministratori possono abilitare gestione remota eseguendo i passaggi in Server Manager [per configurare Gestione remota di Server Manager tramite l'interfaccia di Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) o [per abilitare la gestione remota di Server Manager tramite Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell) in questo argomento.
    > 
    > Se si disabilita la gestione remota per impostazione predefinita come parte di un'installazione automatica e non abilitare la gestione remota nel server dopo l'installazione, i server in cui viene applicato questo file di risposta non possono essere completamente gestiti tramite Server Manager. Server che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (gestione remota è disattivata per impostazione predefinita) genera errori di stato di gestibilità nella console di Server Manager dopo l'aggiunta al pool di server di gestione Server.

## <a name="windows-remote-management-winrm-listener-settings"></a>Impostazioni del listener di gestione remota Windows (WinRM)
Server Manager si basa su Impostazioni listener WinRM sui server remoti che si desidera gestire. Se il meccanismo di autenticazione predefinito o il numero di porta del listener di gestione remota Windows su un server remoto è stato modificato rispetto alle impostazioni predefinite, Server Manager non può comunicare con il server remoto.

Nell'elenco seguente mostra le impostazioni listener WinRM predefinite per la gestione tramite Server Manager.

-   Il servizio WinRM è in esecuzione.

-   È stato creato un listener WinRM per accettare le richieste HTTP attarverso la porta numero 5985.

-   La porta numero 5985 è stata abilitata nelle impostazioni di Windows Firewall per consentire le richieste tramite WinRM.

-   Sono abilitati entrambi i tipi di autenticazione: **Kerberos** e **autenticazione con negoziazione**.

Il numero di porta 5985 è quello predefinito per consentire comunicazione di WinRM con un computer remoto.

Per ulteriori informazioni su come configurare le impostazioni del listener WinRM, al prompt dei comandi digitare **winrm help config**e quindi premere INVIO.

## <a name="see-also"></a>Vedi anche
[Aggiungere server a Server Manager](add-servers-to-server-manager.md)
[windows PowerShell: about_remote_Troubleshooting nel TechCenter di Windows Server](https://technet.microsoft.com/library/dd347642.aspx)
[Descrizione del controllo dell'account utente](https://support.microsoft.com/kb/951016)



