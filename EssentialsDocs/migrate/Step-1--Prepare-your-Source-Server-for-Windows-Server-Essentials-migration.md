---
title: 'Passaggio 1: Preparare il server di origine per la migrazione a Windows Server Essentials'
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2efb1badde6d0ca11bc3b7526fdfb377d9f95d3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827862"
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Passaggio 1: Preparare il server di origine per la migrazione a Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Questo argomento illustra come eseguire il backup del server di origine, valutare l'integrità del sistema del server di origine, installare gli aggiornamenti e i Service Pack più recenti e verificare la configurazione della rete.  
  
## <a name="to-prepare-for-migration"></a>Per preparare la migrazione  
 Per accertarsi che la migrazione dei dati e delle impostazioni dal server di origine al server di destinazione venga effettuata correttamente, completare i passaggi preliminari seguenti:  
  
1.  [Eseguire il backup del Server di origine](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installare il service pack più recenti](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Eliminare il Log in impostazione account servizio](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  
  
4.  [Valutare l'integrità del Server di origine](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  
  
5.  [Creare un piano per la migrazione delle applicazioni line-of-business](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  
  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a> Eseguire il backup del Server di origine  
 Eseguire il backup del server di origine prima di avviare il processo di migrazione. In questo modo, si proteggono i dati da un'eventuale perdita accidentale in caso di errore irreversibile durante la migrazione.  
  
##### <a name="to-back-up-the-source-server"></a>Per eseguire il backup del server di origine  
  
1.  Usare una delle risorse nella tabella seguente come guida per l'esecuzione di un backup completo del server di origine.  
  
2.  Verificare che l'esecuzione del backup abbia avuto esito positivo. Per verificare l'integrità del backup, selezionare file casuali dal backup, ripristinarli in un percorso alternativo e quindi controllare che i file ripristinati corrispondano ai file originali.  
  
   |Prodotto|Risorsa|
   |---|---|
   |Windows Small Business Server 2003|[Backup e ripristino di Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |Windows Small Business Server 2008|[Backup e ripristino dei dati su Windows Small Business Server 2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[Backup e ripristino](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |Windows Small Business Server 2011 Essentials|[Altre informazioni sulla configurazione del server backup](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[Gestione dei Backup del Server](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[Gestire Backup e ripristino in Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a> Installare il service pack più recenti  
 Prima della migrazione, è necessario installare sul server di origine gli aggiornamenti e i Service Pack più recenti.  
  
###  <a name="BKMK_DeleteSvcAcctSetting"></a> Eliminare il Log in impostazione account servizio  
 Se si sta eseguendo la migrazione da Windows Small Business Server 2003 o Windows Server 2003, eliminare l'impostazione account **Accesso come servizio** da Criteri di gruppo.  
  
##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Per eliminare il Log in come un'impostazione di account di servizio  
  
1.  Per aprire lo strumento **Gestione Criteri di gruppo**, fare clic su **Start**, scegliere **Pannello di controllo**, fare clic su **Strumenti di amministrazione**, quindi fare clic su **Gestione Criteri di gruppo**.  
  
2.  Fare clic con il pulsante destro del mouse su **Criterio controller di dominio predefiniti**, quindi scegliere **Modifica**.  
  
3.  Passare a **Configurazione computer\Impostazioni di Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti utente**.  
  
4.  Nel riquadro dei dettagli fare doppio clic su **Accesso come servizio**.  
  
5.  Deselezionare la casella di controllo **Definisci le impostazioni relative ai criteri** .  
  
6.  Delete \\\localhost\SYSVOL\\<domainname\>\scripts\SBS_LOGIN_SCRIPT.bat.  
  
###  <a name="BKMK_EvaluateHealth"></a> Valutare l'integrità del Server di origine  
 Prima di iniziare la migrazione, è importante valutare l'integrità del server di origine. Usare le procedure seguenti per verificare che gli aggiornamenti siano correnti, per generare un rapporto sull'integrità del sistema e per eseguire Windows Server Solutions Best Practice Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Scaricare e installare gli aggiornamenti critici e della sicurezza  
 L'installazione di aggiornamenti critici e della sicurezza nel server di origine aiuta a eseguire la migrazione correttamente e a proteggere la rete durante il processo di migrazione.  
  
###### <a name="to-check-for-the-latest-updates"></a>Per cercare gli ultimi aggiornamenti  
  
1.  Nel server di origine fare clic su **Start**, scegliere **Tutti i programmi**, quindi fare clic su **Windows Update**.  
  
2.  Fare clic su **Verifica disponibilità aggiornamenti**.  
  
3.  Se vengono trovati aggiornamenti, fare clic su **Installa aggiornamenti**.  
  
#### <a name="run-the-best-practices-analyzer"></a>Eseguire Best Practices Analyzer  
 È possibile eseguire Best Practices Analyzer (BPA) per verificare che non ci siano problemi sul server, in rete o nel dominio prima di avviare il processo di migrazione. BPA raccoglie informazioni di configurazione dalle origini seguenti:  
  
-   Strumentazione gestione Windows (WMI) di Active Directory  
  
-   Registro di sistema  
  
-   Internet Information Services (IIS)  
  
###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Per usare BPA per l'analisi del server di origine  
  
1.  La tabella seguente include collegamenti a Microsoft Download Center, in cui è possibile scaricare e installare Best Practices Analyzer (BPA) per il server di origine.  
  
   |Se è in esecuzione nel Server di origine|è possibile ottenere gli strumenti di BPA|
   |---|---|
   |Windows SBS 2003|[Sito Web Microsoft Windows Small Business Server 2003 Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=5334)
   |Windows SBS 2008|[Sito Web Microsoft Windows Small Business Server 2008 Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=6231)  
   |Windows SBS 2011 Essentials o Windows SBS 2011 Standard|[Sito Web di Windows Server Solutions Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=15556) 
   |Windows Server Essentials o Windows Server 2012|Dashboard del server  
  
2.  Al termine del download, fare clic su **Start**, scegliere **Tutti i programmi** e quindi **SBS Best Practices Analyzer Tool**.  
  
    > [!NOTE]
    >  Prima di procedere all'analisi del server, verificare la disponibilità di aggiornamenti.  
  
3.  Nel riquadro di spostamento fare clic su **Avvia un'analisi**.  
  
     Se il Server di origine esegue Windows Server Essentials, eseguire le operazioni seguenti:  
  
    1.  Accedere al server di destinazione come amministratore e quindi aprire il dashboard.  
  
    2.  Nel dashboard fare clic sulla scheda **Dispositivi**.  
  
    3.  Nel <**Server** >**attività** riquadro, fare clic su **Best Practices Analyzer**.  
  
4.  Nel riquadro dei dettagli digitare l'etichetta dell'analisi e quindi fare clic su **Avvia l'analisi**. L'etichetta dell'analisi è il nome del rapporto di analisi, ad esempio **SBS BPA Scan 1Jul2013**.  
  
5.  Al termine dell'analisi, fare clic su **Visualizza un rapporto di questa analisi Best Practices**.  
  
 Dopo aver raccolto informazioni sulla configurazione del server, lo strumento BPA verifica che le informazioni siano corrette e quindi presenta agli amministratori un elenco di informazioni e problemi ordinati per gravità. L'elenco descrive ogni problema e offre un suggerimento o una possibile soluzione. Sono disponibili tre tipi di rapporti:  
  
|Tipo di rapporto|Descrizione
|-----------------|----------------- 
|Rapporti elenco|Visualizza i rapporti in un elenco unidimensionale. 
|Rapporti albero|Visualizza i rapporti in un elenco gerarchico.

Per visualizzare la descrizione e le soluzioni per un problema, fare clic su un problema all'interno del rapporto. Non tutti i problemi segnalati dallo strumento BPA influiscono sulla migrazione, ma è consigliabile risolvere il maggior numero possibile di problemi affinché la migrazione abbia esito positivo.  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a> Sincronizzare l'ora del Server di origine con un'origine ora esterna  
 L'ora sul server di origine deve avere una tolleranza massima di cinque minuti rispetto all'ora del server di destinazione e la data e il fuso orario devono essere gli stessi su entrambi i server. Se il server di origine è in esecuzione su una macchina virtuale, la data, l'ora e il fuso orario sul server host devono corrispondere a quelli del server di origine e del server di destinazione. Per garantire che Windows Server Essentials sia installato correttamente, è necessario sincronizzare l'ora del Server di origine al server di protocollo NTP (Network Time) su Internet.  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Per sincronizzare l'ora del server di origine con il server NTP  
  
1.  Accedere al server di origine con l'account e la password di un amministratore di dominio.  
  
2.  Fare clic su **Start**, scegliere **Esegui**, digitare **cmd** nella casella di testo e quindi premere INVIO.  
  
3.  Al prompt dei comandi, digitare w32tm /config /syncfromflags: DOMHIER /Reliable: no /update e quindi premere INVIO.  
  
4.  Al prompt dei comandi, digitare net stop w32time e quindi premere INVIO.  
  
5.  Al prompt dei comandi, digitare net start w32time e quindi premere INVIO.  
  
> [!IMPORTANT]
>  Durante l'installazione di Windows Server Essentials, sarà possibile verificare l'ora nel Server di destinazione e modificarla, se necessario. Verificare che la differenza rispetto all'ora del server di origine non superi i cinque minuti. Al termine dell'installazione, il server di destinazione verrà sincronizzato con il server NTP. Tutti i computer aggiunti al dominio, incluso il server di origine, vengono sincronizzati con il server di destinazione che assume il ruolo di master emulatore del controller di dominio primario (PDC, Primary Domain Controller).  
  
###  <a name="BKMK_MigrateLOB"></a> Creare un piano per la migrazione delle applicazioni line-of-business  
 Un'applicazione line-of-business (LOB) è un'applicazione critica del computer, essenziale per la conduzione di un'attività commerciale, Tra le applicazioni line-of-business vi sono le applicazioni di contabilità, di gestione della supply chain e di pianificazione delle risorse.  
  
 Se si prevede di eseguire la migrazione delle applicazioni line-of-business, è importante consultarsi con il fornitore di queste applicazioni per stabilire quale sia il metodo appropriato per eseguire la migrazione. È inoltre necessario individuare il dispositivo utilizzato per installare le applicazioni line-of-business sul server di destinazione.  
  
> [!NOTE]
>  Se è stato usato Windows Small Business Server 2011 Essentials SDK per lo sviluppo di integrità di sistema personalizzata o avviso aggiuntivo e si desidera continuare a usare il componente aggiuntivo con Windows Server Essentials, è anche necessario aggiornare il componente aggiuntivo e distribuirlo nel Server di destinazione.  
  
  
### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Creare un piano per la migrazione della posta elettronica ospitata in Windows SBS 2011, Windows SBS 2008 e Windows SBS 2003  
 In Windows SBS 2011, Windows SBS 2008 e Windows SBS 2003, la posta elettronica è fornita tramite Microsoft Exchange Server. Tuttavia, Windows Server Essentials non fornisce servizio di posta elettronica nella posta in arrivo. Se attualmente si usa un server che esegue Windows SBS 2011, Windows SBS 2008 o Windows SBS 2003 per ospitare la posta elettronica aziendale s, è necessario eseguire la migrazione a un'alternativa in locale o ospitato soluzione.  
  
> [!NOTE]
>  Dopo l'aggiornamento e la preparazione del server di origine per la migrazione, si consiglia di creare una copia di backup del server aggiornato prima di continuare con il processo di migrazione.  
  
#### <a name="migrate-email-to-microsoft-office-365"></a>Eseguire la migrazione della posta elettronica a Microsoft Office 365  
 Se si è scelto di usare Microsoft Office 365 come soluzione di posta elettronica per il dominio, seguire le indicazioni in [Eseguire la migrazione di tutte le cassette postali sul cloud con una migrazione del cutover di Exchange](http://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) per avviare la migrazione della posta elettronica a Office 365. È consigliabile completare la migrazione della posta elettronica prima di installare Windows Server Essentials.  
  
> [!NOTE]
>  Il passaggio per rimuovere il Server di Exchange in locale nel Server di origine è obbligatorio se si prevede di integrare Windows Server Essentials con Office 365. Per informazioni su come eseguire la migrazione delle cartelle pubbliche di Exchange Server a Office 365, vedere il post di blog relativo agli [script di migrazione delle cartelle pubbliche di Microsoft Exchange 2013 per Office 365](http://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).  
>   
>  Dopo aver completato l'installazione, è necessario attivare la funzionalità di integrazione di Office 365 in Windows Server Essentials eseguendo la **integra con Microsoft Office 365** attività.  
  
> [!IMPORTANT]
>  Per consentire allo strumento di migrazione di Office 365 di connettersi a Exchange Server nel server di origine, è necessario abilitare RPC su HTTP nel server di origine. Per informazioni su come abilitare RPC su HTTP, vedere [Come distribuire RPC su HTTP per la prima volta in Small Business Server 2003 (Standard o Premium)](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Se l'esecuzione dello strumento di migrazione di Office 365 dopo aver abilitato RPC su HTTP ha esito negativo, controllare l'impostazione **ValidPorts** nel Registro di sistema in HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy e verificare che il nome di dominio completo per il server di origine sia presente nell'elenco. Se non è presente, aggiungerlo manualmente attenendosi all'esempio riportato di seguito:  
>   
>  remote. *contoso*.com:6001-6002;remote. *contoso*.com:6004 (sostituire *contoso* con il nome del dominio in uso).  
  
#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Eseguire la migrazione della posta elettronica a un altro Exchange Server locale  
 Per informazioni sulla migrazione della posta elettronica a un altro Exchange Server locale, vedere [integrare un Server di Exchange On-Premises con Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx). È consigliabile configurare il nuovo Server di Exchange in locale dopo aver installato Windows Server Essentials e quindi completare la migrazione della posta elettronica prima di abbassare di livello il Server di origine.  
  
> [!NOTE]
>  Il connettore POP3 di Windows Small Business Server non è incluso in Exchange Server. Al termine della migrazione dei dati della posta elettronica a un altro Exchange Server, non sarà più possibile utilizzare la funzionalità Connettore POP3.  
  
> [!NOTE]
>  Dopo l'aggiornamento e la preparazione del server di origine per la migrazione, è consigliabile creare una copia di backup del server aggiornato prima di continuare con il processo di migrazione.  
  
## <a name="next-steps"></a>Passaggi successivi  
 È stato preparato il Server di origine per la migrazione a Windows Server Essentials.  Passare quindi a [passaggio 2: Installare Windows Server Essentials come nuovo controller di dominio replica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).  

Per visualizzare tutti i passaggi, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

