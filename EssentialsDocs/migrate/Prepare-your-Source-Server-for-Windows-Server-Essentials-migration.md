---
title: Preparare il server di origine per Windows Server Essentials migration1
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 503b8edc645b43da1dc5c5fb37547e8e0245d4a2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318812"
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Preparare il server di origine per Windows Server Essentials migration1

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per accertarsi che la migrazione dei dati e delle impostazioni dal server di origine al server di destinazione venga effettuata correttamente, completare i passaggi preliminari seguenti:  
  
#### <a name="to-prepare-for-migration"></a>Per preparare la migrazione  
  

1.  [Eseguire il backup del server di origine](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installare i Service Pack più recenti](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Valutare l'integrità del server di origine](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Eseguire lo strumento di preparazione alla migrazione nel server di origine](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Creazione di un piano per la migrazione di applicazioni line-of-business](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [Eseguire il backup del server di origine](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installare i Service Pack più recenti](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Valutare l'integrità del server di origine](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Eseguire lo strumento di preparazione alla migrazione nel server di origine](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Creazione di un piano per la migrazione di applicazioni line-of-business](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Eseguire il backup del server di origine  
 Eseguire il backup del server di origine prima di avviare il processo di migrazione. In questo modo, si proteggono i dati da un'eventuale perdita accidentale in caso di errore irreversibile durante la migrazione.  
  
##### <a name="to-back-up-the-source-server"></a>Per eseguire il backup del server di origine  
  
1.  Eseguire un backup completo del server di origine. Per ulteriori informazioni sul backup di Windows Small Business Server 2011 Essentials, vedere [altre informazioni sulla configurazione del backup del server](https://technet.microsoft.com/library/server-backup-support-1.aspx).  
  
2.  Verificare che l'esecuzione del backup abbia avuto esito positivo. Per verificare l'integrità del backup, selezionare file casuali dal backup, ripristinarli in un percorso alternativo e quindi controllare che i file ripristinati corrispondano ai file originali.  
  
###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Installare i Service Pack più recenti  
 Prima della migrazione, è necessario installare sul server di origine gli aggiornamenti e i Service Pack più recenti.  
  
###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Valutare l'integrità del server di origine  
 Prima di iniziare la migrazione, è importante valutare l'integrità del server di origine. Usare le procedure seguenti per verificare che gli aggiornamenti siano correnti, per generare un rapporto sull'integrità del sistema e per eseguire Windows Server Solutions Best Practice Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Scaricare e installare gli aggiornamenti critici e della sicurezza  
 L'installazione di aggiornamenti critici e della sicurezza nel server di origine aiuta a eseguire la migrazione correttamente e a proteggere la rete durante il processo di migrazione.  
  
###### <a name="to-check-for-the-latest-updates"></a>Per cercare gli ultimi aggiornamenti  
  
1.  Nel server di origine fare clic su **Start**, scegliere **Tutti i programmi**, quindi fare clic su **Windows Update**.  
  
2.  Fare clic su **Verifica disponibilità aggiornamenti**.  
  
3.  Se vengono trovati aggiornamenti, fare clic su **Installa aggiornamenti**.  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>Cercare gli errori critici nel Visualizzatore avvisi  
 È possibile cercare eventuali errori critici nel Visualizzatore avvisi nel dashboard.  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Eseguire Windows Server Solutions Best Practices Analyzer  
 È possibile eseguire Windows Server Solutions Best Practices Analyzer (BPA) per verificare che non ci siano problemi sul server, in rete o nel dominio prima di avviare il processo di migrazione. BPA raccoglie informazioni di configurazione dalle origini seguenti:  
  
-   Strumentazione gestione Windows (WMI) di Active Directory®  
  
-   Registro di sistema  
  
-   Metabase di Internet Information Services (IIS)  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Per usare Windows Server Solutions BPA per l'analisi del server di origine  
  
1. Scaricare e installare [Windows Server Solutions Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=15556) nell'Area download Microsoft.  
  
2. Al termine del download, fare clic su **Start**, scegliere **Tutti i programmi** e quindi **SBS Best Practices Analyzer Tool**.  
  
   > [!NOTE]
   >  Prima di procedere all'analisi del server, verificare la disponibilità di aggiornamenti.  
  
3. Nel riquadro di spostamento fare clic su **Avvia un'analisi**.  
  
4. Nel riquadro dei dettagli digitare l'etichetta dell'analisi e quindi fare clic su **Avvia l'analisi**. L'etichetta dell'analisi è il nome del report di analisi, ad esempio **SBS BPA Scan 1Jul2012**.  
  
5. Al termine dell'analisi, fare clic su **Visualizza un rapporto di questa analisi Best Practices**.  
  
   Dopo aver raccolto informazioni sulla configurazione del server, Windows Server Solutions BPA verifica che le informazioni siano corrette e quindi presenta agli amministratori un elenco di informazioni e problemi ordinati per gravità. L'elenco descrive ogni problema e offre un suggerimento o una possibile soluzione. Sono disponibili tre tipi di rapporti:  
  
|Tipo di rapporto|Descrizione|  
|-----------------|-----------------|  
|Rapporti elenco|Visualizza i rapporti in un elenco unidimensionale.|  
|Rapporti albero|Visualizza i rapporti in un elenco gerarchico.|  
|Altri rapporti|Visualizza i rapporti quali il registro di run-time.|  
  
 Per visualizzare la descrizione e le soluzioni per un problema, fare clic su un problema all'interno del rapporto. Non tutti i problemi segnalati da Windows SBS 2011 Essentials BPA influiscono sulla migrazione, ma è consigliabile risolvere il maggior numero possibile di problemi per assicurarsi che la migrazione abbia esito positivo.  
  
####  <a name="synchronize-the-source-server-time-with-an-external-time-source"></a><a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Sincronizzare l'ora del server di origine con un'origine ora esterna  
 L'ora sul server di origine deve avere una tolleranza massima di cinque minuti rispetto all'ora del server di destinazione e la data e il fuso orario devono essere gli stessi su entrambi i server. Se il server di origine è in esecuzione su una macchina virtuale, la data, l'ora e il fuso orario sul server host devono corrispondere a quelli del server di origine e del server di destinazione. Per assicurarsi che Windows Server Essentials sia installato correttamente, è necessario sincronizzare l'ora del server di origine con il server NTP (Network Time Protocol) su Internet.  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Per sincronizzare l'ora del server di origine con il server NTP  
  
1.  Accedere al server di origine con l'account e la password di un amministratore di dominio.  
  
2.  Fare clic su **Start**, scegliere **Esegui**, digitare **cmd** nella casella di testo e quindi premere INVIO.  
  
3.  Al prompt dei comandi digitare w32tm /config /syncfromflags:domhier /reliable:no /update e quindi premere INVIO.  
  
4.  Al prompt dei comandi digitare net stop w32time e quindi premere INVIO.  
  
5.  Al prompt dei comandi digitare net start w32time e quindi premere INVIO.  
  
> [!IMPORTANT]
>  Durante l'installazione di Windows Server Essentials, è possibile verificare l'ora del server di destinazione e modificarla, se necessario. Verificare che la differenza rispetto all'ora del server di origine non superi i cinque minuti. Al termine dell'installazione, il server di destinazione verrà sincronizzato con il server NTP. Tutti i computer aggiunti al dominio, incluso il server di origine, vengono sincronizzati con il server di destinazione che assume il ruolo di master emulatore del controller di dominio primario (PDC, Primary Domain Controller).  
  
###  <a name="run-the-migration-preparation-tool-on-the-source-server"></a><a name="BKMK_MPT"></a>Eseguire lo strumento di preparazione alla migrazione nel server di origine  
 Non è possibile eseguire un'installazione in modalità di migrazione senza aver prima eseguito lo strumento di preparazione alla migrazione sul server di origine. Questo strumento è progettato per preparare il server di origine e il dominio per la migrazione a Windows Server Essentials.  
  
> [!IMPORTANT]
>  Eseguire il backup del server di origine prima di eseguire lo strumento di preparazione alla migrazione. Tutte le modifiche che lo strumento di preparazione alla migrazione apporta allo schema sono irreversibili. Se si verificano dei problemi durante la migrazione, l'unico modo per riportare il server di origine allo stato precedente l'esecuzione dello strumento di preparazione alla migrazione è ripristinare il server da un backup del sistema.  
  
 Per eseguire lo strumento di preparazione alla migrazione, è necessario essere un membro dei gruppi Enterprise Admins, Schema Admins e Domain Admins.  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Per verificare di disporre delle autorizzazioni necessarie per eseguire lo strumento sul server di origine  
  
1. Sul server di origine, fare clic su **Start**, fare clic su **Strumenti di amministrazione**, quindi fare clic su **Utenti e computer di Active Directory**.  
  
2. Nell'albero della console fare clic per espandere il dominio e quindi fare clic su **Utenti**.  
  
3. Fare clic sull'account amministratore che si utilizza per la migrazione e fare clic su **Proprietà**.  
  
4. Fare clic sulla scheda **Membro di** e verificare che i gruppi Enterprise Admins, Schema Admins e Domain Admins siano elencati nella casella di testo **Membro di**.  
  
5. Se i gruppi non sono presenti nell'elenco, fare clic su **Aggiungi** e aggiungere ciascun gruppo non presente nell'elenco.  
  
   > [!NOTE]
   > - Se il servizio Netlogon non è stato avviato, si potrebbe un messaggio di errore relativo alle autorizzazioni.  
   >   -   È necessario disconnettersi dal server e quindi riconnettersi per applicare le modifiche.  
  
    È possibile utilizzare la versione più recente dell'agente di Windows Update affinché il processo di aggiornamento del server venga eseguito correttamente.  
  
   È possibile utilizzare la versione più recente dell'agente di Windows Update affinché il processo di aggiornamento del server venga eseguito correttamente.  
  
   Prima di poter installare Windows Update Agent nel server di origine, è necessario installare prima Windows PowerShell 2,0 e Microsoft Baseline Configuration Analyzer 2,0.  
  
-   Per scaricare e installare Windows PowerShell 2,0, vedere l' [articolo 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) della Microsoft Knowledge base.  
  
-   Per scaricare e installare Microsoft Baseline Configuration Analyzer 2,0, vedere [Microsoft Baseline Configuration analyzer 2,0](https://go.microsoft.com/fwlink/p/?LinkId=241484) nell'area download Microsoft.  
  
-   Per scaricare e installare la versione più recente dell'agente di Windows Update, vedere l'[articolo 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) nella Microsoft Knowledge Base.  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Per installare lo strumento di preparazione alla migrazione sul server di origine  
  
1. Inserire Windows Server Essentials DVD1 nell'unità DVD del server di origine.  
  
2. Aprire Esplora risorse, accedere alla cartella **\support\tools** del DVD e fare doppio clic sul file **sourcetool.msi**.  
  
   > [!NOTE]
   > - Se lo strumento di preparazione alla migrazione è già installato nel server, eseguirlo dal menu **Start**.  
   >   -   Per accertarsi che la migrazione venga eseguita senza problemi, è consigliabile scegliere di installare l'aggiornamento più recente.  
  
    La procedura guidata installa lo strumento di preparazione alla migrazione sul server di origine. Una volta completata l'installazione, lo strumento di preparazione alla migrazione viene eseguito automaticamente installa gli aggiornamenti più recenti.  
  
3. In Strumento di preparazione alla migrazione selezionare **Ho una copia di backup e sono pronto a procedere** e quindi fare clic su **Avanti**.  
  
   > [!WARNING]
   >  Se viene visualizzato un messaggio di errore relativo all'installazione di un hotfix, vedere Metodo 2: rinominare la cartella Catroot2 nell' [articolo 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) della Microsoft Knowledge base.  
  
    Lo strumento di preparazione alla migrazione prepara il dominio di origine per la migrazione estendendo lo schema di Active Directory. Una volta completata l'attività, fare clic su **Avanti** per continuare.  
  
4. Dopo aver preparato il dominio di origine, lo strumento di preparazione alla migrazione analizza il server di origine per identificare due tipi di potenziali problemi.  
  
   - **Errori** di Problemi rilevati nel server di origine che possono bloccare la migrazione o causare la mancata riuscita della migrazione. Seguire le istruzioni visualizzate nel messaggio di errore per risolvere i problemi e quindi fare clic su **Esegui nuova ricerca**.  
  
   - **Avvisi** di Problemi rilevati nel server di origine che possono causare problemi funzionali durante la migrazione. È consigliabile seguire le istruzioni visualizzare nel messaggio per risolvere i problemi prima di procedere con la migrazione.  
  
     Dopo aver risolto o valutato tutti i problemi, fare clic su **Avanti**.  
  
5. Nella finestra Strumento di preparazione alla migrazione, fare clic su **Fine**.  
  
6. Al termine dell'esecuzione dello strumento di preparazione alla migrazione, è possibile che venga richiesto di riavviare il server di origine prima di iniziare la migrazione a Windows Server Essentials.  
  
> [!NOTE]
>  È necessario completare una corretta esecuzione dello strumento di preparazione alla migrazione nel server di origine entro due settimane dall'installazione di Windows Server Essentials nel server di destinazione. In caso contrario, l'installazione di Windows Server Essentials nel server di destinazione verrà bloccata. In questo caso, è necessario eseguire di nuovo lo strumento di preparazione alla migrazione sul server di origine.  
  
###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Creazione di un piano per la migrazione di applicazioni line-of-business  
 Un'applicazione line-of-business (LOB) è un'applicazione critica del computer, essenziale per la conduzione di un'attività commerciale, Tra le applicazioni line-of-business vi sono le applicazioni di contabilità, di gestione della supply chain e di pianificazione delle risorse.  
  
 Se si prevede di eseguire la migrazione delle applicazioni line-of-business, è importante consultarsi con il fornitore di queste applicazioni per stabilire quale sia il metodo appropriato per eseguire la migrazione. È inoltre necessario individuare il dispositivo utilizzato per installare le applicazioni line-of-business sul server di destinazione.  
  
> [!NOTE]
>  Se è stato usato Windows Small Business Server 2011 Essentials SDK per sviluppare un componente aggiuntivo personalizzato per gli avvisi o per l'integrità del sistema e si vuole continuare a usare il componente aggiuntivo con Windows Server Essentials, è necessario aggiornare anche il componente aggiuntivo e distribuirlo nel server di destinazione.  
  
