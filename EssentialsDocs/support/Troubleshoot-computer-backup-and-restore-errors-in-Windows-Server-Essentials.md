---
title: Risolvere i problemi di backup del computer e ripristinare gli errori in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 06/25/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37e79661442ba9f66a564b6c6c8fb57db1978454
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Risolvere i problemi di backup del computer e ripristinare gli errori in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Utilizzare queste procedure per risolvere i problemi di backup del computer in Windows Server Essentials, inclusi i problemi di configurazione del backup, backup incompleti o non riusciti, gli avvisi di backup dello stato e problemi relativi a file, cartelle o ripristino completo del sistema.  
  
> [!NOTE]
>  Per informazioni sulla risoluzione dei problemi più recenti dalla community Windows Server Essentials, visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums//winserveressentials/threads).  
  
##  <a name="BKMK_TroubleshootBackupConfigurationIssues"></a>Risolvere i problemi di configurazione del backup per un computer connesso  
 Utilizzare queste procedure per risolvere i problemi con le configurazioni di backup per i computer che vengono eseguito il backup del server Windows Server Essentials.  
  
### <a name="errors"></a>Errori  
  
-   Configurazione di backup non completato correttamente  
  
-   Raccolta di informazioni errore per computer  
  
-   Errore rimuovere computer dal backup  
  
### <a name="resolutions"></a>Risoluzioni  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Per risolvere i problemi che si verificano quando si configura il backup per un computer connesso  
  
1.  Assicurarsi che il computer è connesso alla rete tramite un dispositivo di rete.  
  
2.  Verificare che il dispositivo di rete che il computer è connesso a anche sia connesso alla rete, accesa e funzioni correttamente.  
  
3.  Assicurarsi che il servizio di Backup Client di Windows Server e servizio Provider Backup Computer Windows Server Client sono in esecuzione sul server.  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>Per avviare i servizi di backup computer nel server di  
  
    1.  Nel server, fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **servizi**.  
  
    2.  Scorrere verso il basso e fare clic su **servizio Provider Backup Computer Client Server Windows**. Se lo stato del servizio non è **avviato**, fare doppio clic su servizio e quindi fare clic su **Start**.  
  
    3.  Fare clic su **servizio Backup Computer Client Server Windows**. Se lo stato del servizio non è **avviato**, fare doppio clic su servizio e quindi fare clic su **Start**.  
  
    4.  Chiudi **servizi**.  
  
4.  Assicurarsi che il servizio Provider Backup Computer di Windows Server Client sia in esecuzione nel computer client.  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Per avviare il servizio di backup del computer nel computer client  
  
    1.  Nel computer client, fare clic su **Start**, tipo **servizi** nel **Cerca programmi e file** casella di testo e quindi premere INVIO.  
  
    2.  Scorrere verso il basso e fare clic su **servizio Provider Backup Computer Client Server Windows**. Se lo stato del servizio non è **avviato**, fare doppio clic su servizio e quindi fare clic su **Start**.  
  
    3.  Chiudi **servizi**.  
  
5.  Controllare gli avvisi per determinare se sono presenti errori nel database di backup. Se sono presenti errori, seguire le istruzioni nell'avviso per ripristinare il database di backup.  
  
6.  Disinstallare il software connettore Windows Server Essentials dal computer e quindi reinstallarlo. Per ulteriori informazioni, vedere gli argomenti [disinstallare il software connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [installare il software connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>Risolvere i problemi relativi a un backup non completato correttamente  
 Quando un backup ha lo stato non riuscito, nessuna parte del backup ha avuto esito positivo e dati non sono disponibili per il ripristino. Tuttavia, un backup con stato incompleto, non tutti gli elementi specificati nel backup configurazione sono stati sottoposti a backup, ma alcuni dati potrebbero essere disponibili per il ripristino.  
  
### <a name="errors"></a>Errori  
  
-   Backup è incompleto  
  
-   Backup non riuscito  
  
### <a name="resolutions"></a>Risoluzioni  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Per identificare i volumi di backup non riuscita  
  
1.  Aprire il Dashboard di Windows Server Essentials, quindi fare clic su **computer e Backup**.  
  
2.  Fare clic sul nome del computer in cui backup non completato correttamente e quindi fare clic su **Visualizza proprietà del computer** nel **attività** riquadro.  
  
3.  Fare clic sul backup non completato correttamente e quindi fare clic su **visualizzare i dettagli**.  
  
4.  Nel **dettagli Backup** viene visualizzata un segno di spunta verde nella finestra di dialogo di stato di ogni volume sottoposto correttamente a backup.  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Risolvere i problemi relativi a un backup non riuscito di un volume  
  
1.  Verificare che il disco rigido sia connesso al computer, acceso e che funzioni correttamente.  
  
2.  Eseguire **chkdsk /f /r** per correggere gli errori sul disco rigido (**/f**) e recuperare informazioni leggibili da eventuali settori danneggiati (**/r**). Per ulteriori informazioni sull'esecuzione **chkdsk**, vedere [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).  
  
3.  Assicurarsi che il computer non è stato arrestato o disconnesso dalla rete durante l'esecuzione di backup.  
  
4.  Assicurarsi che vi sia spazio libero sufficiente in ogni volume per eseguire il backup. Backup richiede spazio su disco aggiuntivo nel computer client per creare uno snapshot VSS. In qualsiasi volume non è riservato per il sistema, deve essere libero almeno il 10% di spazio disponibile su disco. In un volume riservato per il sistema, se le dimensioni del volume sono inferiori a 500 MB, VSS richiede 32 MB di spazio libero per creare uno snapshot. Se il volume è 500 MB o più, VSS richiede 320 MB di spazio libero.  
  
     Se è sufficiente spazio libero disponibile su un volume, prova una di queste risoluzioni:  
  
    -   Estendere il volume. È possibile estendere qualsiasi volume di base o dinamico tranne il volume riservato per il sistema.  
  
        ###### <a name="to-extend-a-volume"></a>Per estendere un volume  
  
        1.  Nel Pannello di controllo, fare clic su **sistema e sicurezza**.  
  
        2.  In **strumenti di amministrazione**, fare clic su **creare e formattare le partizioni del disco rigido**.  
  
        3.  Fare clic sul volume da estendere. Se **Estendi Volume** è abilitata, selezionare l'opzione corrispondente. Se l'opzione non è abilitato, è possibile estendere il volume.  
  
        4.  Seguire i passaggi in estensione guidata Volume per estendere il volume.  
  
    -   Eliminare il contenuto dal volume per aumentare lo spazio disponibile.  
  
        > [!NOTE]
        >  Se è necessario liberare spazio sul volume riservato per il sistema, è possibile spostare l'immagine di ripristino di sistema in un volume diverso. Per istruzioni, vedere [distribuire un'immagine di ripristino di sistema](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx).  
  
    -   Escludere il volume dal backup del client. Eseguire questa operazione solo se non è importante conservare una copia di backup dei dati nel volume.  
  
        > [!WARNING]
        >  Se si esclude il volume riservato per il sistema da un backup del client, il sistema client non verrà eseguito e non sarà in grado di eseguire un ripristino completo del sistema del computer.  
  
5.  Cercare altri avvisi nel server che potrebbero indicare non c'è spazio su disco insufficiente nel server per il backup completato correttamente. Seguire le istruzioni nell'avviso per risolvere il problema.  
  
6.  Eseguire **vssadmin** in un prompt dei comandi del servizio Copia Shadow del Volume (VSS) di risoluzione dei problemi. Per informazioni su **vssadmin**, vedere [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332).  
  
##  <a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>Risolvere i problemi degli avvisi di backup dello stato  
  
### <a name="errors"></a>Errori  
  
-   Il servizio Provider Backup Computer di Windows Server Solutions ha smesso di funzionare  
  
-   Il servizio di Provider di Windows Server Solutions Client Computer Backup ha smesso di funzionare  
  
### <a name="resolutions"></a>Risoluzioni  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>Per risolvere un avviso di stato backup  
  
1.  Se un avviso indica che il database di backup presenta problemi, seguire le istruzioni nell'avviso per risolvere il problema.  
  
2.  Se un avviso indica che un servizio di backup non è in esecuzione, provare ad avviare il servizio nel server o il computer client in cui viene visualizzato il messaggio di errore.  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>Per avviare i servizi di backup nel server di  
  
    1.  Nel server, fare clic su **Start**e fare clic su **strumenti di amministrazione**, quindi fare clic su **servizi**.  
  
        > [!NOTE]
        >  Se si sta amministrando il server in modalità remota, è necessario usare connessione Desktop remoto per accedere al desktop del server. Per informazioni sull'uso di connessione Desktop remoto, vedere [connettersi a un altro computer mediante connessione Desktop remoto](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection).  
  
    2.  Scorrere verso il basso e fare clic su **servizio Provider Backup Computer Client Server Windows**. Se lo stato del servizio non è **avviato**, fare doppio clic su servizio e quindi fare clic su **Start**.  
  
    3.  Fare clic su **servizio Backup Computer Client Server Windows**. Se lo stato del servizio non è **avviato**, fare doppio clic su servizio e quindi fare clic su **Start**.  
  
    4.  Chiudi **servizi**.  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>Per avviare i servizi di backup in un computer client  
  
    1.  Nel computer client, fare clic su **Start**, tipo **servizi** nel **Cerca programmi e file** casella di testo, quindi premere INVIO.  
  
    2.  Fare doppio clic su **servizio Provider Backup Computer Client Server Windows**, quindi fare clic su **Start**.  
  
    3.  Chiudi il **servizi**.  
  
3.  Controllare i registri eventi nel computer client o server per i problemi relativi a servizi di backup o driver.  
  
4.  Riavviare il computer server o client in cui viene visualizzato il messaggio di errore.  
  
5.  Controllare gli avvisi di integrità per altri problemi che possono avere un effetto sul backup del client.  
  
##  <a name="BKMK_FileAndFolder"></a>Risolvere i problemi relativi a un ripristino file o una cartella  
  
### <a name="errors"></a>Errori  
  
-   Ripristino file o una cartella non completato correttamente.  
  
### <a name="resolutions"></a>Risoluzioni  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Per risolvere un ripristino di file o una cartella non riuscito  
  
1.  Assicurarsi che il computer è connesso alla rete tramite un dispositivo di rete.  
  
2.  Verificare che il dispositivo di rete che il computer è connesso a anche sia connesso alla rete, accesa e funzioni correttamente.  
  
3.  Controllare gli avvisi per determinare se sono presenti errori nel database di backup. Se sono presenti errori, seguire le istruzioni nell'avviso per ripristinare il database di backup.  
  
4.  Provare a ripristinare i file o cartelle da un altro backup.  
  
5.  Assicurarsi che il **Windows Server soluzione Driver per ripristino Computer** sia installato e funzioni correttamente.  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>Per controllare lo stato di Windows Server soluzione Computer ripristinare il Driver  
  
    1.  Fare clic su **Start**, tipo **Gestione dispositivi** nel **Cerca programmi e file** casella di testo, quindi premere INVIO.  
  
    2.  In Gestione dispositivi, fare clic su **dispositivi di sistema**, scorrere fino a **Windows Server Solutions Driver per ripristino Computer**.  
  
    3.  Se il driver non è visualizzato:  
  
        1.  Aprire un prompt dei comandi con privilegi di amministratore ed eseguire il comando seguente:  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe?  posso**  
  
        2.  Aggiornare Gestione dispositivi. Il driver dovrebbe essere visualizzato.  
  
    4.  Se l'icona visualizzata è un monitor del computer, il driver è correttamente installato e in esecuzione. Chiudere Gestione dispositivi.  
  
    5.  Se l'icona visualizzata non è un monitor del computer  
  
        1.  Fare doppio clic su **Windows Server Solutions Driver per ripristino Computer**, quindi fare clic su **proprietà**.  
  
        2.  Fare clic su di **Driver** scheda e quindi fare clic su **Aggiorna Driver**.  
  
        3.  Fare clic su **cerca automaticamente un driver aggiornato**e quindi segui le istruzioni per aggiornare il driver.  
  
    6.  Chiudere Gestione dispositivi.  
  
6.  Disinstallare il software connettore Windows Server Essentials dal computer e quindi reinstallarlo. Per ulteriori informazioni, vedere gli argomenti [disinstallare il software connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [installare il software connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_Troubleshootfullsystemrestoreissues"></a>Risolvere i problemi relativi a un ripristino completo del sistema  
  
### <a name="errors"></a>Errori  
  
-   Impossibile accedere al computer client dopo un ripristino completo del sistema.  
  
### <a name="resolutions"></a>Risoluzioni  
 Se si modifica il nome di un computer, e successivamente si desidera ripristinare un backup salvato prima della modifica del nome, dopo il ripristino, quando si tenta di accedere con l'account di dominio, viene visualizzato questo errore: "il database di protezione sul server non ha un account computer per la relazione di trust tra questa workstation". Per ottenere nuovamente accesso di rete al computer, rimuovere il software connettore, rimuovere il computer dal dominio di Windows e quindi connettersi al server.  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Per ottenere nuovamente accesso di rete a un computer ripristinato dopo una modifica del nome computer  
  
1.  Accedere al computer come amministratore locale.  
  
2.  Disinstallare il software connettore. Per ulteriori informazioni, vedere [disinstallare il software connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
3.  Rimuovere il computer dal dominio. Per ulteriori informazioni, vedere [rimuovere un computer da un dominio Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  
  
4.  Connettere nuovamente il computer al server. Per ulteriori informazioni, vedere [connettere i computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Supporto per Windows Server Essentials](Support-Windows-Server-Essentials.md)