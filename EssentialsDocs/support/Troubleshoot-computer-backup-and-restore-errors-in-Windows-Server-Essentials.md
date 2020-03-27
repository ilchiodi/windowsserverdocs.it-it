---
title: Risolvere gli errori di backup e ripristino del computer in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 06/25/2013
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 28e3564c93f192563626bfb44992ef9bc4a49598
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313244"
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Risolvere gli errori di backup e ripristino del computer in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Usare queste procedure per risolvere i problemi di backup del computer in Windows Server Essentials, inclusi i problemi di configurazione del backup, i backup incompleti o non riusciti, gli avvisi sullo stato di integrità del backup e i problemi relativi a file, cartelle o ripristino completo del sistema.  
  
> [!NOTE]
>  Per le informazioni più recenti sulla risoluzione dei problemi della community di Windows Server Essentials, visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums//winserveressentials/threads).  
  
##  <a name="troubleshoot-backup-configuration-issues-for-a-connected-computer"></a><a name="BKMK_TroubleshootBackupConfigurationIssues"></a>Risolvere i problemi di configurazione del backup per un computer connesso  
 Usare queste procedure per risolvere i problemi relativi alle configurazioni di backup per i computer sottoposti a backup nel server Windows Server Essentials.  
  
### <a name="errors"></a>Errori  
  
-   Configurazione di backup non completata correttamente  
  
-   Errore nella raccolta di informazioni per il computer  
  
-   Errore nella rimozione del computer dal backup  
  
### <a name="resolutions"></a>Soluzioni  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Per risolvere i problemi che si verificano quando si configura il backup per un computer connesso  
  
1.  Verificare che il computer sia connesso alla rete tramite un dispositivo di rete.  
  
2.  Verificare che il dispositivo di rete a cui il computer è connesso sia a sua volta connesso alla rete, che sia acceso e che funzioni correttamente.  
  
3.  Verificare che il Servizio Backup computer client Windows Server e il Servizio Provider backup computer client Windows Server siano in esecuzione nel server.  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>Per avviare i servizi di backup del computer nel server  
  
    1.  Nel server fare clic su **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Servizi**.  
  
    2.  Scorrere verso il basso e fare clic su **Servizio Provider backup computer client Windows Server**. Se lo stato del servizio non è **Avviato**, fare clic con il pulsante destro del mouse sul servizio e scegliere **Avvia**.  
  
    3.  Fare clic su **Servizio Backup computer client Windows Server**. Se lo stato del servizio non è **Avviato**, fare clic con il pulsante destro del mouse sul servizio e scegliere **Avvia**.  
  
    4.  Chiudere **Servizi**.  
  
4.  Verificare che il Servizio Provider backup computer client Windows Server sia in esecuzione nel computer client.  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Per avviare il servizio di backup del computer nel computer client  
  
    1.  Nel computer client fare clic su**Start**, digitare **Servizi** nella casella **Cerca programmi e file**, quindi premere INVIO.  
  
    2.  Scorrere verso il basso e fare clic su **Servizio Provider backup computer client Windows Server**. Se lo stato del servizio non è **Avviato**, fare clic con il pulsante destro del mouse sul servizio e scegliere **Avvia**.  
  
    3.  Chiudere **Servizi**.  
  
5.  Controllare gli avvisi per determinare se sono presenti errori nel database di backup. Se sono presenti errori, seguire le istruzioni nell'avviso per ripristinare il database di backup.  
  
6.  Disinstallare il software Connettore di Windows Server Essentials dal computer e quindi reinstallarlo. Per altre informazioni, vedere gli argomenti [Disinstallare il software Connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [Installare il software Connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="troubleshoot-a-backup-that-did-not-complete-properly"></a><a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>Risolvere i problemi relativi a un backup non completato correttamente  
 Quando un backup ha lo stato Non riuscito, nessuna parte del backup è stata completata e dati non sono disponibili per il ripristino. Tuttavia, quando lo stato di un backup è Incompleto, non tutti gli elementi specificati nella configurazione di backup sono stati salvati, ma alcuni dati potrebbero essere disponibili per il ripristino.  
  
### <a name="errors"></a>Errori  
  
-   Backup incompleto  
  
-   Backup non riuscito  
  
### <a name="resolutions"></a>Soluzioni  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Per identificare i volumi il cui backup non è riuscito  
  
1.  Aprire il dashboard di Windows Server Essentials e quindi fare clic su **Computer e backup**.  
  
2.  Fare clic sul nome del computer il cui backup non è stato completato correttamente e quindi su **Visualizza proprietà del computer** nel riquadro **Attività**.  
  
3.  Fare clic sul backup non completato correttamente e quindi su **Visualizza dettagli**.  
  
4.  Nella finestra di dialogo **Dettagli backup**, nello stato di ogni volume sottoposto correttamente a backup è visualizzato un segno di spunta verde.  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Per risolvere i problemi relativi al backup di un volume non riuscito  
  
1.  Verificare che il disco rigido sia connesso al computer, sia acceso e funzioni correttamente.  
  
2.  Eseguire **chkdsk /f /r** per risolvere eventuali errori nel disco rigido ( **/f**) e recuperare informazioni leggibili da eventuali settori danneggiati ( **/r**). Per altre informazioni sull'esecuzione di **chkdsk**, vedere [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).  
  
3.  Verificare che il computer non sia stato arrestato o disconnesso dalla rete durante l'esecuzione del backup.  
  
4.  Verificare che vi sia spazio libero sufficiente in ogni volume per l'esecuzione del backup. Per il backup è necessario spazio su disco aggiuntivo nel computer client per la creazione di uno snapshot del Servizio Copia Shadow del volume. In qualsiasi volume non riservato per il sistema, almeno il 10% dello spazio disponibile su disco deve essere libero. In un volume riservato per il sistema, se le dimensioni del volume sono inferiori a 500 MB, il Servizio Copia Shadow del volume richiede 32 MB di spazio libero per creare uno snapshot. Se le dimensioni del volume sono di 500 MB o più, il Servizio Copia Shadow del volume richiede 320 MB di spazio libero  
  
     Se in un volume non è disponibile spazio sufficiente, provare una di queste risoluzioni:  
  
    -   Estendere il volume. È possibile estendere qualsiasi volume di base o dinamico, ad eccezione del volume riservato per il sistema.  
  
        ###### <a name="to-extend-a-volume"></a>Per estendere un volume  
  
        1.  Nel Pannello di controllo fare clic su **Sistema e sicurezza**.  
  
        2.  In **Strumenti di amministrazione** fare clic su **Crea e formatta le partizioni del disco rigido**.  
  
        3.  Fare clic con il pulsante destro del mouse sul volume da estendere. Se l'opzione **Estendi volume** è abilitata, selezionarla. Se l'opzione non è abilitata, non è possibile estendere il volume.  
  
        4.  Seguire i passaggi in Estensione guidata volume per estendere il volume.  
  
    -   Eliminare contenuto dal volume per aumentare lo spazio disponibile.  
  
        > [!NOTE]
        >  Se è necessario liberare spazio sul volume riservato per il sistema, è possibile spostare l'immagine di ripristino del sistema in un altro volume. Per istruzioni, vedere [Distribuire un'immagine di ripristino del sistema](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx).  
  
    -   Escludere il volume dal backup del client. Eseguire questa operazione solo se non è importante conservare una copia di backup dei dati nel volume.  
  
        > [!WARNING]
        >  Se si esclude il volume riservato per il sistema da un backup del client, il sistema client non verrà sottoposto a backup e non sarà possibile eseguire un ripristino completo del sistema nel computer.  
  
5.  Verificare l'eventuale presenza di altri avvisi nel server che possono indicare che nel server non è disponibile spazio su disco sufficiente per il corretto completamento del backup. Seguire le istruzioni nell'avviso per risolvere il problema.  
  
6.  Eseguire **vssadmin** in un prompt dei comandi per la risoluzione dei problemi del Servizio Copia Shadow del volume. Per informazioni su **vssadmin**, vedere [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332).  
  
##  <a name="troubleshoot-backup-health-alert-issues"></a><a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>Risolvere i problemi relativi agli avvisi di integrità del backup  
  
### <a name="errors"></a>Errori  
  
-   Arresto del Servizio Provider backup computer delle soluzioni Windows Server  
  
-   Arresto del Servizio Provider backup computer client delle soluzioni Windows Server  
  
### <a name="resolutions"></a>Soluzioni  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>Per risolvere un problema relativo a un avviso sullo stato di integrità del backup  
  
1.  Se un avviso indica che il database di backup presenta problemi, seguire le istruzioni nell'avviso per risolvere il problema.  
  
2.  Se un avviso indica un servizio di backup non è in esecuzione, provare ad avviare il servizio nel server o nel computer client in cui viene visualizzato il messaggio di errore.  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>Per avviare i servizi di backup nel server  
  
    1.  Nel server fare clic su **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Servizi**.  
  
        > [!NOTE]
        >  Se si sta amministrando il server in remoto, è necessario usare connessione Desktop remoto per accedere al desktop del server. Per informazioni sull'uso di Connessione Desktop remoto, vedere [Connettersi a un altro computer mediante Connessione desktop remoto](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection).  
  
    2.  Scorrere verso il basso e fare clic su **Servizio Provider backup computer client Windows Server**. Se lo stato del servizio non è **Avviato**, fare clic con il pulsante destro del mouse sul servizio e scegliere **Avvia**.  
  
    3.  Fare clic su **Servizio Backup computer client Windows Server**. Se lo stato del servizio non è **Avviato**, fare clic con il pulsante destro del mouse sul servizio e scegliere **Avvia**.  
  
    4.  Chiudere **Servizi**.  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>Per avviare i servizi di backup in un computer client  
  
    1.  Nel computer client fare clic su**Start**, digitare **Servizi** nella casella **Cerca programmi e file**, quindi premere INVIO.  
  
    2.  Fare clic con il pulsante destro del mouse su **Servizio Provider backup computer client Windows Server** e scegliere **Avvia**.  
  
    3.  Chiudere **Servizi**.  
  
3.  Controllare nei registri eventi del computer client o server l'eventuale presenza di problemi relativi ai servizi di backup o ai driver.  
  
4.  Riavviare il computer client o server in cui viene visualizzato il messaggio di errore.  
  
5.  Controllare negli avvisi di integrità l'eventuale presenza di altri problemi che possono influire sul backup del client.  
  
##  <a name="troubleshoot-a-file-or-folder-restore"></a><a name="BKMK_FileAndFolder"></a>Risolvere i problemi relativi al ripristino di un file o una cartella  
  
### <a name="errors"></a>Errori  
  
-   Ripristino di un file o una cartella non completato correttamente.  
  
### <a name="resolutions"></a>Soluzioni  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Per risolvere i problemi relativi al ripristino non riuscito di un file o una cartella  
  
1.  Verificare che il computer sia connesso alla rete tramite un dispositivo di rete.  
  
2.  Verificare che il dispositivo di rete a cui il computer è connesso sia a sua volta connesso alla rete, che sia acceso e che funzioni correttamente.  
  
3.  Controllare gli avvisi per determinare se sono presenti errori nel database di backup. Se sono presenti errori, seguire le istruzioni nell'avviso per ripristinare il database di backup.  
  
4.  Provare a ripristinare i file o le cartelle da un altro backup.  
  
5.  Verificare che **Soluzioni Windows Server - Driver per ripristino computer** sia installato e funzioni correttamente.  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>Per verificare lo stato di Soluzioni Windows Server - Driver per ripristino computer  
  
    1.  Fare clic su **Start**, digitare **Gestione dispositivi** nella casella **Cerca programmi e file**, quindi premere INVIO.  
  
    2.  In Gestione dispositivi fare clic su **Dispositivi di sistema** e scorrere fino a **Soluzioni Windows Server - Driver per ripristino computer**.  
  
    3.  Se il driver non è visualizzato:  
  
        1.  Aprire un prompt dei comandi con privilegi di amministratore ed eseguire il comando seguente:  
  
             **%Programmi%\Windows Server\Bin\BackupDriverInstaller.exe? -i**  
  
        2.  Aggiornare Gestione dispositivi. Il driver dovrebbe essere visualizzato.  
  
    4.  Se l'icona visualizzata è un monitor del computer, il driver è correttamente installato e in esecuzione. Chiudere Gestione dispositivi.  
  
    5.  Se l'icona visualizzata non è un monitor del computer  
  
        1.  Fare clic con il pulsante destro del mouse su **Soluzioni Windows Server - Driver per ripristino computer** e quindi scegliere **Proprietà**.  
  
        2.  Fare clic sulla scheda **Driver** e quindi su **Aggiorna driver**.  
  
        3.  Fare clic su **Cerca automaticamente un driver aggiornato** e seguire le istruzioni per aggiornare il driver.  
  
    6.  Chiudere Gestione dispositivi.  
  
6.  Disinstallare il software Connettore di Windows Server Essentials dal computer e quindi reinstallarlo. Per altre informazioni, vedere gli argomenti [Disinstallare il software Connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [Installare il software Connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="troubleshoot-a-full-system-restore"></a><a name="BKMK_Troubleshootfullsystemrestoreissues"></a>Risolvere i problemi relativi a un ripristino completo del sistema  
  
### <a name="errors"></a>Errori  
  
-   Non è possibile accedere al computer client dopo un ripristino completo del sistema.  
  
### <a name="resolutions"></a>Soluzioni  
 Se si modifica il nome di un computer e successivamente si desidera ripristinare un backup salvato prima della modifica del nome, dopo il ripristino, quando si tenta di accedere con l'account di dominio, viene visualizzato questo messaggio di errore: "Il database di sicurezza sul server non ha un account computer per la relazione di trust di questa workstation". Per ottenere nuovamente accesso di rete al computer rimuovere il software Connettore, rimuovere il computer dal dominio Windows e quindi riconnettersi al server.  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Per ottenere nuovamente accesso di rete a un computer ripristinato dopo una modifica del nome del computer  
  
1.  Accedere al computer come amministratore locale.  
  
2.  Disinstallare il software Connettore. Per altre informazioni, vedere [Disinstallare il software Connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
3.  Rimuovere il computer dal dominio. Per altre informazioni, vedere [Rimuovere un computer da un dominio Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  
  
4.  Connettere nuovamente il computer al server. Per ulteriori informazioni, vedere [Come connettere i computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Supportare Windows Server Essentials](Support-Windows-Server-Essentials.md)