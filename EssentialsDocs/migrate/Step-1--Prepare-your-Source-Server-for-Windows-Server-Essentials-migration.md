---
title: 'Passaggio 1: Preparare la migrazione di Server di origine per Windows Server Essentials'
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Passaggio 1: Preparare la migrazione di Server di origine per Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

In questo argomento viene illustrato come eseguire il backup del Server di origine, valutare l'integrità di sistema del Server di origine, installare il service pack e le correzioni più recenti e verificare la configurazione di rete.  
  
## <a name="to-prepare-for-migration"></a>Per preparare la migrazione  
 Completare i passaggi preliminari seguenti per assicurarsi che le impostazioni e i dati nel Server di origine eseguire la migrazione correttamente al Server di destinazione.  
  
1.  [Eseguire il backup del Server di origine](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installare il service pack più recenti](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Eliminare il registro su come un'impostazione account del servizio](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  
  
4.  [Valutare l'integrità del Server di origine](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  
  
5.  [Creare un piano per la migrazione delle applicazioni line-of-business](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  
  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Eseguire il backup del Server di origine  
 Eseguire il backup del Server di origine prima di iniziare il processo di migrazione. Effettuare un backup consente di proteggere i dati da perdita accidentale in caso di errore irreversibile durante la migrazione.  
  
##### <a name="to-back-up-the-source-server"></a>Per eseguire il backup del Server di origine  
  
1.  Utilizzare una delle risorse nella tabella seguente per consentono di eseguire un backup completo del Server di origine.  
  
2.  Verificare che il backup è stato eseguito correttamente. Per verificare l'integrità del backup, selezionare file casuali dal backup, ripristinarli in un percorso alternativo e quindi verificare che i file ripristinati siano uguali ai file originali.  
  
   |Prodotto|Risorsa|
   |---|---|
   |Windows Small Business Server 2003|[Eseguire il backup e ripristino di Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |Windows Small Business Server 2008|[Eseguire il backup e ripristino dei dati su Windows Small Business Server 2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[Backup e ripristino](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |Windows Small Business Server 2011 Essentials|[Altre informazioni sulla configurazione del backup del server](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[Backup del Server di gestione](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[Gestire Backup e ripristino in Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Installare il service pack più recenti  
 Nel Server di origine prima della migrazione, è necessario installare gli aggiornamenti più recenti e i service pack.  
  
###  <a name="BKMK_DeleteSvcAcctSetting"></a>Eliminare il registro su come un'impostazione account del servizio  
 Se durante la migrazione da Windows Small Business Server 2003 o Windows Server 2003, eliminare il **accedere come un servizio** account impostazione di criteri di gruppo.  
  
##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Per eliminare il registro su come un'impostazione account del servizio  
  
1.  Per aprire il **Gestione criteri di gruppo** strumento, fare clic su **Start**, fare clic su **Pannello di controllo**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Gestione criteri di gruppo**.  
  
2.  Fare doppio clic su **criterio controller di dominio predefiniti**, quindi fare clic su **modifica**.  
  
3.  Passare a **assegnazione di Computer Configurazione computer\Impostazioni Windows\Impostazioni sicurezza\Criteri locali\Assegnazione diritti**.  
  
4.  Nel riquadro dei dettagli fare doppio clic su **accedere come un servizio**.  
  
5.  Cancella il **definire queste impostazioni dei criteri** casella di controllo.  
  
6.  Eliminare \scripts\sbs_login_script.bat. \\\localhost\SYSVOL\\ < domainname\ >.  
  
###  <a name="BKMK_EvaluateHealth"></a>Valutare l'integrità del Server di origine  
 È importante valutare l'integrità del Server di origine prima di iniziare la migrazione. Utilizzare le procedure seguenti per verificare che gli aggiornamenti siano correnti, per generare un rapporto di integrità di sistema e per l'esecuzione di Windows Server soluzioni Best Practice Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Download e installazione critici e sicurezza degli aggiornamenti  
 L'installazione critici e aggiornamenti protezione nel Server di origine contribuisce a garantire che la migrazione sia stata completata e consente di proteggere la rete durante il processo di migrazione.  
  
###### <a name="to-check-for-the-latest-updates"></a>Per cercare gli aggiornamenti più recenti  
  
1.  Dal Server di origine, fare clic su **Start**, fare clic su **tutti i programmi**, quindi fare clic su **Windows Update**.  
  
2.  Fare clic su **verifica disponibilità aggiornamenti**.  
  
3.  Se vengono trovati aggiornamenti, fare clic su **installare gli aggiornamenti**.  
  
#### <a name="run-the-best-practices-analyzer"></a>Eseguire Best Practices Analyzer  
 È possibile eseguire Best Practices Analyzer (BPA) per verificare che vi siano problemi sul server, rete o dominio prima di iniziare il processo di migrazione. BPA raccoglie informazioni di configurazione dalle origini seguenti:  
  
-   Windows Management Instrumentation (WMI) di Active Directory  
  
-   Il Registro di sistema  
  
-   Internet Information Services (IIS)  
  
###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Per usare BPA per l'analisi del Server di origine  
  
1.  Nella tabella seguente vengono forniti collegamenti a Microsoft Download Center in cui è possibile scaricare e installare Best Practices Analyzer (BPA) per il Server di origine.  
  
   |Se il Server di origine è in esecuzione|è possibile ottenere gli strumenti BPA da|
   |---|---|
   |Windows SBS 2003|[Sito Web Microsoft Windows Small Business Server 2003 Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=5334)
   |Windows SBS 2008|[Sito Web Microsoft Windows Small Business Server 2008 Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=6231)  
   |Windows SBS 2011 Essentials o Windows SBS 2011 Standard|[Sito Web di Windows Server Solutions Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=15556) 
   |Windows Server Essentials o Windows Server 2012|Il dashboard di server  
  
2.  Al termine del download, fare clic su **Start**, scegliere **tutti i programmi**, quindi fare clic su **SBS Best Practices Analyzer Tool**.  
  
    > [!NOTE]
    >  Controllare gli aggiornamenti prima di acquisire il server.  
  
3.  Nel riquadro di spostamento, fare clic su **avviare un'analisi**.  
  
     Se il Server di origine è in esecuzione Windows Server Essentials, eseguire le operazioni seguenti:  
  
    1.  Accedere al Server di destinazione come amministratore e quindi aprire il Dashboard.  
  
    2.  Nel Dashboard, fare clic su di **dispositivi** scheda.  
  
    3.  Nel <**Server** >**attività** riquadro, fare clic su **Best Practices Analyzer**.  
  
4.  Nel riquadro dei dettagli, digitare l'etichetta dell'analisi, quindi fare clic su **avvia l'analisi**. L'etichetta dell'analisi è il nome del rapporto di analisi, ad esempio, **SBS BPA Scan 1jul2013**.  
  
5.  Al termine dell'analisi, fare clic su **visualizzare un report di questa analisi Best Practices**.  
  
 Dopo lo strumento BPA raccoglie informazioni sulla configurazione del server, verifica che le informazioni siano corrette e quindi presenta agli amministratori un elenco di informazioni e problemi ordinati per gravità. L'elenco descrive ogni problema e fornisce una suggerimento o una possibile soluzione. Sono disponibili tre tipi di report:  
  
|Tipo di report|Descrizione
|-----------------|----------------- 
|Rapporti elenco|Visualizza i rapporti in un elenco unidimensionale. 
|Rapporti albero|Visualizza i rapporti in un elenco gerarchico.

Per visualizzare la descrizione e le soluzioni per un problema, fare clic il problema nel report. Non tutti i problemi segnalati dallo strumento BPA influiscono sulla migrazione, ma è consigliabile risolvere molti dei problemi di come è possibile garantire che la migrazione abbia esito positivo.  
  
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
  
###  <a name="BKMK_MigrateLOB"></a>Creare un piano per la migrazione delle applicazioni line-of-business  
 Un'applicazione di line-of-business (LOB) è un'applicazione critica del computer che è essenziale per la conduzione di un'azienda. Le applicazioni LOB includono gestione contabilità, della supply chain e pianificazione delle risorse.  
  
 Quando si prevede di eseguire la migrazione delle applicazioni LOB, consultare i fornitori di applicazioni LOB per determinare il metodo appropriato per la migrazione di ogni applicazione. È inoltre necessario individuare il supporto che viene utilizzato per installare le applicazioni LOB nel Server di destinazione.  
  
> [!NOTE]
>  Se hai usato Windows Small Business Server 2011 Essentials SDK per sviluppare un integrità di sistema personalizzato o avviso aggiuntivo e si desidera continuare a usare il componente aggiuntivo con Windows Server Essentials, è anche necessario aggiornare il componente aggiuntivo e distribuirlo nel Server di destinazione.  
  
  
### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Creare un piano per la migrazione della posta elettronica ospitata su Windows SBS 2011, Windows SBS 2008 e Windows SBS 2003  
 In Windows SBS 2011, Windows SBS 2008 e Windows SBS 2003, posta elettronica è fornito da Microsoft Exchange Server. Tuttavia, Windows Server Essentials non fornisce un servizio di posta elettronica della posta in arrivo. Se attualmente si utilizza un server che esegue Windows SBS 2011, Windows SBS 2008 o Windows SBS 2003 per ospitare la posta elettronica aziendale s, è necessario eseguire la migrazione di un locale alternativo o soluzione ospitata.  
  
> [!NOTE]
>  Dopo l'aggiornamento e preparare il Server di origine per la migrazione, è consigliabile creare un backup del server aggiornato prima di continuare il processo di migrazione.  
  
#### <a name="migrate-email-to-microsoft-office-365"></a>Migrazione della posta elettronica per Microsoft Office 365  
 Se si è scelto di usare Microsoft Office 365 come soluzione di posta elettronica per il dominio, seguire le indicazioni in [eseguire la migrazione di tutte le cassette postali sul Cloud con Cutover di Exchange](http://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) per avviare la migrazione della posta elettronica a Office 365. È consigliabile completare la migrazione della posta elettronica prima di installare Windows Server Essentials.  
  
> [!NOTE]
>  Il passaggio per la rimozione di Exchange Server locale nel Server di origine è obbligatorio se si intende integrare Windows Server Essentials con Office 365. Per informazioni su come eseguire la migrazione delle cartelle pubbliche di Exchange Server a Office 365, vedere il post di blog [script Microsoft Exchange 2013 pubblico cartelle di migrazione per Office 365](http://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).  
>   
>  Dopo aver completato l'installazione, attivare la funzionalità di integrazione di Office 365 in Windows Server Essentials eseguendo il **integra con Microsoft Office 365** attività.  
  
> [!IMPORTANT]
>  Per consentire allo strumento di migrazione di Office 365 di connettersi al Server di Exchange che è in esecuzione nel Server di origine, è necessario abilitare RPC su HTTP nel Server di origine. Per informazioni su come abilitare RPC su HTTP, vedere [come distribuire RPC su HTTP per la prima volta in Small Business Server 2003 (Standard o Premium)](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Se è possibile eseguire correttamente lo strumento di migrazione di Office 365 dopo aver abilitato RPC su HTTP, visualizzare il **ValidPorts** l'impostazione del Registro di sistema in HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy e verificare che sia elencato il nome di dominio completo (FQDN) del Server di origine. Se non è presente, aggiungerlo manualmente utilizzando l'esempio seguente:  
>   
>  remoto. *Contoso*.com:6001-6002; remoto. *Contoso*com: 6004 (sostituire *contoso* con il nome del dominio).  
  
#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Migrazione della posta elettronica a un altro Server di Exchange in locale  
 Per informazioni sulla migrazione della posta elettronica a un altro Exchange Server locale, vedere [integrare un Server Exchange locale con Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx). È consigliabile impostare il nuovo Server di Exchange in locale dopo aver installato Windows Server Essentials e quindi completare la migrazione della posta elettronica prima di abbassare di livello il Server di origine.  
  
> [!NOTE]
>  Connettore POP3 di Windows Small Business Server non è incluso in Exchange Server. Dopo la migrazione di dati della posta elettronica a un altro Exchange Server, è più possibile usare la funzionalità connettore POP3.  
  
> [!NOTE]
>  Dopo l'aggiornamento e preparare il Server di origine per la migrazione, è necessario creare un backup del server aggiornato prima di continuare il processo di migrazione.  
  
## <a name="next-steps"></a>Passaggi successivi  
 Il Server di origine è stato preparato per la migrazione a Windows Server Essentials.  Passare quindi a [passaggio 2: installare Windows Server Essentials come nuovo controller di dominio replica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).  

Per visualizzare tutti i passaggi, vedere [la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

