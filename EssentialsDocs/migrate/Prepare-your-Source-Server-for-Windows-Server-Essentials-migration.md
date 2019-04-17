---
title: Preparare il Server di origine per Windows Server Essentials migration1
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 929c7506c78667646e429c4f28df7e5642c575ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Preparare il Server di origine per Windows Server Essentials migration1

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Completare i passaggi preliminari seguenti per assicurarsi che le impostazioni e i dati nel Server di origine eseguire la migrazione correttamente al Server di destinazione.  
  
#### <a name="to-prepare-for-migration"></a>Per preparare la migrazione  
  

1.  [Eseguire il backup del Server di origine](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installare il service pack più recenti](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Valutare l'integrità del Server di origine](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Eseguire lo strumento di preparazione alla migrazione nel Server di origine](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Creare un piano per la migrazione delle applicazioni line-of-business](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [Eseguire il backup del Server di origine](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installare il service pack più recenti](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Valutare l'integrità del Server di origine](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Eseguire lo strumento di preparazione alla migrazione nel Server di origine](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Creare un piano per la migrazione delle applicazioni line-of-business](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Eseguire il backup del Server di origine  
 Eseguire il backup del Server di origine prima di iniziare il processo di migrazione. Effettuare un backup consente di proteggere i dati da perdita accidentale in caso di errore irreversibile durante la migrazione.  
  
##### <a name="to-back-up-the-source-server"></a>Per eseguire il backup del Server di origine  
  
1.  Eseguire un backup completo del Server di origine. Per ulteriori informazioni sul backup di Windows Small Business Server 2011 Essentials, vedere [ulteriori informazioni sulla configurazione del backup del server](https://technet.microsoft.com/library/server-backup-support-1.aspx).  
  
2.  Verificare che il backup è stato eseguito correttamente. Per verificare l'integrità del backup, selezionare file casuali dal backup, ripristinarli in un percorso alternativo e quindi verificare che i file ripristinati siano uguali ai file originali.  
  
###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Installare il service pack più recenti  
 Nel Server di origine prima della migrazione, è necessario installare gli aggiornamenti più recenti e i service pack.  
  
###  <a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Valutare l'integrità del Server di origine  
 È importante valutare l'integrità del Server di origine prima di iniziare la migrazione. Utilizzare le procedure seguenti per verificare che gli aggiornamenti siano correnti, per generare un rapporto di integrità di sistema e per l'esecuzione di Windows Server soluzioni Best Practice Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Download e installazione critici e sicurezza degli aggiornamenti  
 L'installazione critici e aggiornamenti protezione nel Server di origine contribuisce a garantire che la migrazione sia stata completata e consente di proteggere la rete durante il processo di migrazione.  
  
###### <a name="to-check-for-the-latest-updates"></a>Per cercare gli aggiornamenti più recenti  
  
1.  Dal Server di origine, fare clic su **Start**, fare clic su **tutti i programmi**, quindi fare clic su **Windows Update**.  
  
2.  Fare clic su **verifica disponibilità aggiornamenti**.  
  
3.  Se vengono trovati aggiornamenti, fare clic su **installare gli aggiornamenti**.  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>Cercare nel Visualizzatore avvisi per gli errori critici  
 È possibile controllare il Visualizzatore avvisi nel Dashboard per individuare eventuali errori critici.  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Eseguire Windows Server Solutions Best Practices Analyzer  
 È possibile eseguire il Windows Server Solutions Best Practices Analyzer (BPA) per verificare che vi siano problemi sul server, rete o dominio prima di iniziare il processo di migrazione. BPA raccoglie informazioni di configurazione dalle origini seguenti:  
  
-   Attiva Directory® Strumentazione gestione (Windows WMI)  
  
-   Il Registro di sistema  
  
-   La metabase di Internet Information Services (IIS)  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Per usare Windows Server Solutions BPA per l'analisi del Server di origine  
  
1.  Scaricare e installare il [Windows Server Solutions Best Practices Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=15556) in Microsoft Download Center.  
  
2.  Al termine del download, fare clic su **Start**, scegliere **tutti i programmi**, quindi fare clic su **SBS Best Practices Analyzer Tool**.  
  
    > [!NOTE]
    >  Controllare gli aggiornamenti prima di acquisire il server.  
  
3.  Nel riquadro di spostamento, fare clic su **avviare un'analisi**.  
  
4.  Nel riquadro dei dettagli, digitare l'etichetta dell'analisi, quindi fare clic su **avvia l'analisi**. L'etichetta dell'analisi è il nome del rapporto di analisi, ad esempio, **SBS BPA Scan 1jul2012**.  
  
5.  Al termine dell'analisi, fare clic su **visualizzare un report di questa analisi Best Practices**.  
  
 Dopo aver raccolto informazioni sulla configurazione del server, Windows Server Solutions BPA verifica che le informazioni siano corrette e quindi presenta agli amministratori un elenco di informazioni e problemi ordinati per gravità. L'elenco descrive ogni problema e fornisce una suggerimento o una possibile soluzione. Sono disponibili tre tipi di report:  
  
|Tipo di report|Descrizione|  
|-----------------|-----------------|  
|Rapporti elenco|Visualizza i rapporti in un elenco unidimensionale.|  
|Rapporti albero|Visualizza i rapporti in un elenco gerarchico.|  
|Altri rapporti|Visualizza i rapporti, ad esempio un registro di Run-Time.|  
  
 Per visualizzare la descrizione e le soluzioni per un problema, fare clic il problema nel report. Non tutti i problemi segnalati da Windows SBS 2011 Essentials BPA influiscono sulla migrazione, ma è consigliabile risolvere il maggior numero di problemi affinché la migrazione abbia esito positiva.  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Sincronizzare l'ora del Server di origine con un'origine ora esterna  
 L'ora nel Server di origine deve essere impostato su entro cinque minuti l'ora nel Server di destinazione e la data e il fuso orario deve essere lo stesso in entrambi i server. Se il Server di origine è in esecuzione in una macchina virtuale, la data, ora e fuso orario nel server host deve corrispondere a quello del Server di origine e il Server di destinazione. Per garantire che Windows Server Essentials è stato installato correttamente, è necessario sincronizzare l'ora del Server di origine al server di protocollo NTP (Network Time) su Internet.  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Per sincronizzare l'ora del Server di origine con il server NTP  
  
1.  Accedere al Server di origine con un account amministratore di dominio e una password.  
  
2.  Fare clic su **Start**, fare clic su **eseguire**, tipo **cmd** nella casella di testo e quindi premere INVIO.  
  
3.  Al prompt dei comandi, digitare w32tm /config /syncfromflags: DOMHIER /Reliable: no /update e quindi premere INVIO.  
  
4.  Al prompt dei comandi, digitare net stop w32time e quindi premere INVIO.  
  
5.  Al prompt dei comandi, digitare net start w32time e quindi premere INVIO.  
  
> [!IMPORTANT]
>  Durante l'installazione di Windows Server Essentials, hai la possibilità di verificare l'ora nel Server di destinazione e cambiarla, se necessario. Assicurarsi che il tempo sia entro cinque minuti di tempo impostato nel Server di origine. Al termine dell'installazione, il Server di destinazione si sincronizza con il protocollo NTP. Tutti i computer appartenenti a un dominio, incluso il Server di origine, vengono sincronizzati con il Server di destinazione che assume il ruolo di dominio primario master dell'emulatore del controller (PDC).  
  
###  <a name="BKMK_MPT"></a>Eseguire lo strumento di preparazione alla migrazione nel Server di origine  
 È possibile eseguire un'installazione in modalità migrazione senza aver prima eseguito lo strumento di preparazione alla migrazione nel Server di origine. Questo strumento è progettato per preparare il Server di origine e il dominio per eseguire la migrazione a Windows Server Essentials.  
  
> [!IMPORTANT]
>  Eseguire il backup del Server di origine prima di eseguire lo strumento di preparazione alla migrazione. Tutte le modifiche che lo strumento di preparazione alla migrazione apporta allo schema sono irreversibili. Se si verificano problemi durante la migrazione, l'unico modo per riportare il Server di origine allo stato che precedente è stato eseguito lo strumento è ripristinare il server da un backup del sistema.  
  
 Per eseguire lo strumento di preparazione alla migrazione, è necessario essere un membro del gruppo Enterprise Admins, gruppo Schema Admins e del gruppo Domain Admins.  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Per verificare di disporre delle autorizzazioni appropriate per eseguire lo strumento sul Server di origine  
  
1.  Nel Server di origine, fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**.  
  
2.  Nell'albero della console, fare clic per espandere il dominio e quindi fare clic su **utenti**.  
  
3.  Fare doppio clic sull'account amministratore che utilizza per la migrazione, quindi fare clic su **proprietà**.  
  
4.  Fare clic su di **membro di** scheda e quindi verificare che Enterprise Admins, Schema Admins e Domain Admins siano elencati nel **appartenente** casella di testo.  
  
5.  Se non sono elencati i gruppi, fare clic su **Aggiungi**, e quindi aggiungere ciascun gruppo che non è elencato.  
  
    > [!NOTE]
    >  -   Se il servizio Netlogon non è stato avviato, potresti ricevere un errore di autorizzazione.  
    > -   È necessario disconnettersi e riaccedere al server rendere effettive le modifiche.  
  
     È possibile utilizzare la versione più recente dell'agente di Windows Update per verificare che il processo di aggiornamento del server funziona correttamente.  
  
 È possibile utilizzare la versione più recente dell'agente di Windows Update per verificare che il processo di aggiornamento del server funziona correttamente.  
  
 Prima di poter installare agente di Windows Update nel Server di origine, è innanzitutto necessario installare Windows PowerShell 2.0 e Microsoft Baseline Configuration Analyzer 2.0.  
  
-   Per scaricare e installare Windows PowerShell 2.0, vedere [articolo 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) della Microsoft Knowledge Base.  
  
-   Per scaricare e installare Microsoft Baseline Configuration Analyzer 2.0, vedere il [Microsoft Baseline Configuration Analyzer 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484) in Microsoft Download Center.  
  
-   Per scaricare e installare la versione più recente dell'agente Windows Update, vedere [articolo 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) della Microsoft Knowledge Base.  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Per installare ed eseguire lo strumento di preparazione alla migrazione nel Server di origine  
  
1.  Inserisci DVD1 di Windows Server Essentials nell'unità DVD nel Server di origine.  
  
2.  Aprire Esplora risorse, passare al **\support\tools** cartella del DVD e quindi fare doppio clic il **sourcetool.msi** file.  
  
    > [!NOTE]
    >  -   Se lo strumento di preparazione alla migrazione è già installato nel server, eseguire lo strumento di **Start** menu.  
    > -   Per assicurarsi che siano preparati per la migliore esperienza possibile migrazione, è consigliabile che sempre scegliere di installare l'aggiornamento più recente.  
  
     La procedura guidata installa lo strumento di preparazione alla migrazione nel Server di origine. Una volta completato l'installazione, lo strumento di preparazione alla migrazione viene eseguito automaticamente e installa gli aggiornamenti più recenti.  
  
3.  Nello strumento di preparazione alla migrazione, selezionare **ho una copia di backup e sono pronto a procedere**, quindi fare clic su **Avanti**.  
  
    > [!WARNING]
    >  Se si riceve un messaggio di errore relativo all'installazione di aggiornamenti rapidi, vedere il metodo 2: rinominare la cartella Catroot2 in [articolo 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) della Microsoft Knowledge Base.  
  
     Lo strumento di preparazione alla migrazione Prepara il dominio di origine per la migrazione estendendo lo schema di Active Directory. Dopo il completamento dell'attività, fare clic su **Avanti** per continuare.  
  
4.  Dopo aver preparato il dominio di origine, lo strumento di preparazione alla migrazione analizza il Server di origine per identificare due tipi di potenziali problemi.  
  
    -   **Errori** rilevati problemi nel Server di origine che può causare l'errore della migrazione o bloccare la migrazione. Seguire le istruzioni nel messaggio di errore, risolvere i problemi, quindi fare clic su **nuova ricerca**.  
  
    -   **Avvisi** rilevati problemi nel Server di origine che possono provocare problemi funzionali durante la migrazione. È consigliabile seguire le istruzioni nel messaggio di errore per risolvere i problemi prima di procedere con la migrazione.  
  
     Dopo aver risolto o valutato tutti i problemi, fare clic su **Avanti**.  
  
5.  Nello strumento di preparazione alla migrazione, fare clic su **fine**.  
  
6.  Al termine, lo strumento di preparazione alla migrazione potrebbe essere necessario riavviare il Server di origine prima di iniziare la migrazione a Windows Server Essentials.  
  
> [!NOTE]
>  È necessario completare l'esecuzione dello strumento di preparazione alla migrazione nel Server di origine entro due settimane dall'installazione di Windows Server Essentials nel Server di destinazione. In caso contrario, l'installazione di Windows Server Essentials nel Server di destinazione verrà bloccata. In questo caso, è necessario eseguire nuovamente lo strumento di preparazione alla migrazione nel Server di origine.  
  
###  <a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Creare un piano per la migrazione delle applicazioni line-of-business  
 Un'applicazione di line-of-business (LOB) è un'applicazione critica del computer che è essenziale per la conduzione di un'azienda. Le applicazioni LOB includono gestione contabilità, della supply chain e pianificazione delle risorse.  
  
 Quando si prevede di eseguire la migrazione delle applicazioni LOB, consultare i fornitori di applicazioni LOB per determinare il metodo appropriato per la migrazione di ogni applicazione. È inoltre necessario individuare il supporto che viene utilizzato per installare le applicazioni LOB nel Server di destinazione.  
  
> [!NOTE]
>  Se hai usato Windows Small Business Server 2011 Essentials SDK per sviluppare un integrità di sistema personalizzato o avviso aggiuntivo e si desidera continuare a usare il componente aggiuntivo con Windows Server Essentials, è anche necessario aggiornare il componente aggiuntivo e distribuirlo nel Server di destinazione.  
  
