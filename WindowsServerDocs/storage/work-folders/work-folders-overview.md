---
ms.assetid: c91c7196-ee0d-4856-8cfb-4c38494ccf1f
title: Panoramica di cartelle di lavoro
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 06/15/2020
description: 'Panoramica di cartelle di lavoro: un ruolo server in Windows Server che consente agli utenti di accedere in modo coerente ai file di lavoro da PC e dispositivi.'
ms.openlocfilehash: 8bd60cc0ab57935a7ce2da0ca33bd0d4c840fa2b
ms.sourcegitcommit: cb266c8ea42b9800babbbe96b17885e82b55787d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/16/2020
ms.locfileid: "84795728"
---
# <a name="work-folders-overview"></a>Panoramica di cartelle di lavoro

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

In questo argomento vengono illustrate le cartelle di lavoro, un servizio ruolo per i file server che eseguono Windows Server che offre agli utenti un modo coerente per accedere ai file di lavoro dai computer e dai dispositivi.  
  
Se si vuole scaricare o usare cartelle di lavoro in Windows 10, Windows 7 o un dispositivo Android o iOS, vedere gli argomenti seguenti:

- [Cartelle di lavoro per Windows 10](https://support.microsoft.com/help/12370/windows-10-work-folders)
- [Cartelle di lavoro per Windows 7 (download di 64 bit)](https://www.microsoft.com/download/details.aspx?id=42558)
- [Cartelle di lavoro per Windows 7 (download di 32 bit)](https://www.microsoft.com/download/details.aspx?id=42559)
- [Cartelle di lavoro per iOS](https://itunes.apple.com/app/work-folders/id950878067)
- [Cartelle di lavoro per Android](https://play.google.com/store/apps/details?id=com.microsoft.workfolders)

## <a name="role-description"></a>Descrizione del ruolo

 Cartelle di lavoro consente agli utenti di archiviare e accedere ai file di lavoro su computer e dispositivi personali, spesso chiamati dispositivi BYOD (Bring Your Own Device), oltre che sui PC aziendali. Gli utenti avranno a disposizione una posizione in cui archiviare i file di lavoro a cui possono accedere da qualsiasi luogo. Le organizzazioni mantengono il controllo sui dati aziendali archiviando i file in file server gestiti centralmente e, facoltativamente, specificando criteri per i dispositivi degli utenti, ad esempio le password di crittografia e schermata di blocco.  
  
 Le cartelle di lavoro possono essere distribuite con distribuzioni esistenti di Reindirizzamento cartelle, File offline e cartelle Home. Cartelle di lavoro archivia i file utente in una cartella nel server denominata *condivisione di sincronizzazione*. È possibile specificare una cartella che contiene già dati utente, che consente di adottare cartelle di lavoro senza eseguire la migrazione di server e dati o di eliminare immediatamente gradualmente la soluzione esistente.  
  
## <a name="practical-applications"></a>Applicazioni pratiche

 Gli amministratori possono usare cartelle di lavoro per fornire agli utenti l'accesso ai file di lavoro, mantenendo al tempo stesso l'archiviazione centralizzata e il controllo sui dati dell'organizzazione. Alcune applicazioni specifiche per cartelle di lavoro includono:  
  
-   Fornire un singolo punto di accesso ai file di lavoro dal lavoro di un utente e da computer e dispositivi personali  
  
-   Accedere ai file di lavoro offline e quindi sincronizzarli con la file server centrale quando il PC o il dispositivo ha una connettività Internet o Intranet  
  
-   Distribuire con distribuzioni esistenti di Reindirizzamento cartelle, File offline e Home Directory  
  
-   Usare tecnologie di gestione file server esistenti, ad esempio la classificazione dei file e le quote di cartelle, per gestire i dati utente  
  
-   Specificare i criteri di sicurezza per indicare ai PC e ai dispositivi degli utenti di crittografare cartelle di lavoro e usare una password della schermata di blocco  
  
-   Usare il clustering di failover con cartelle di lavoro per fornire una soluzione a disponibilità elevata  
  
## <a name="important-functionality"></a>Funzionalità importanti

 Cartelle di lavoro include le funzionalità seguenti.  
  
| Funzionalità | Disponibilità | Descrizione |  
| ------------------- | ------------------ | ----------------- |  
| Servizio ruolo cartelle di lavoro in Server Manager | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 | Servizi file e archiviazione consente di configurare le condivisioni di sincronizzazione (cartelle in cui sono archiviati i file di lavoro dell'utente), monitorare le cartelle di lavoro e gestire le condivisioni di sincronizzazione e l'accesso utente |
| Cmdlet di cartelle di lavoro | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 | Un modulo di Windows PowerShell che contiene cmdlet completi per la gestione dei server di cartelle di lavoro |  
| Integrazione di cartelle di lavoro con Windows | Windows 10<p> Windows 8.1<p> Windows RT 8.1<p> Windows 7 (download obbligatorio) | Cartelle di lavoro fornisce le funzionalità seguenti nei computer Windows:<p> -Elemento del pannello di controllo che configura e monitora cartelle di lavoro<br />-Integrazione di Esplora file che consente un facile accesso ai file nelle cartelle di lavoro<br />-Un motore di sincronizzazione che trasferisce i file da e verso una file server centrale, massimizzando al tempo stesso la durata della batteria e le prestazioni del sistema |
| App cartelle di lavoro per dispositivi | Android<p> Apple iPhone e iPad® | App che consente ai dispositivi più diffusi di accedere ai file in cartelle di lavoro |  
  
## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate
  
Nella tabella seguente vengono descritte alcune delle principali modifiche apportate alle cartelle di lavoro.  
  
| Caratteristica/funzionalità | Novità o aggiornamento | Descrizione |
| ---------------------------- | --------------------- | ----------------- |
| Registrazione migliorata | Novità di Windows Server 2019 | I registri eventi nel server cartelle di lavoro possono essere utilizzati per monitorare l'attività di sincronizzazione e identificare gli utenti che non hanno superato le sessioni di sincronizzazione. Usare l'ID evento 4020 nel registro eventi Microsoft-Windows-SyncShare/Operational per identificare gli utenti che non hanno superato le sessioni di sincronizzazione. Usare l'ID evento 7000 e l'ID evento 7001 nel registro eventi Microsoft-Windows-SyncShare/Reporting per monitorare gli utenti che completano correttamente le sessioni di sincronizzazione per il caricamento e il download. |
| Contatori delle prestazioni | Novità di Windows Server 2019 | Sono stati aggiunti i contatori delle prestazioni seguenti: byte scaricati/sec, byte caricati/sec, utenti connessi, file scaricati/sec, file caricati/sec, utenti con rilevamento delle modifiche, richieste in ingresso/sec e richieste in attesa. |
| Miglioramento delle prestazioni del server | Aggiornamento in Windows Server 2019 | Sono stati apportati miglioramenti alle prestazioni per gestire un numero maggiore di utenti per server. Il limite per ogni server varia ed è basato sul numero di file e sulla varianza del file. Per determinare il limite per server, è necessario aggiungere gli utenti al server in fasi. |
| Accesso ai file su richiesta | Aggiunto a Windows 10 versione 1803 | Consente di visualizzare e accedere a tutti i file. Si controllano i file archiviati nel PC e disponibili offline. Gli altri file sono sempre visibili e non richiedono spazio nel PC, ma è necessaria la connettività alle cartelle di lavoro file server per accedervi. |
| Supporto del proxy dell'applicazione Azure AD | Aggiunto a Windows 10 versione 1703, Android, iOS | Gli utenti remoti possono accedere in modo sicuro ai file nel server di cartelle di lavoro usando Azure AD proxy di applicazione. |
| Replica delle modifiche più veloce | Aggiornamento in Windows 10 e Windows Server 2016 | Con Windows Server 2012 R2, quando le modifiche ai file vengono sincronizzate con il server di Cartelle di lavoro, i client non ricevono una notifica delle modifiche e l'aggiornamento può arrivare anche 10 minuti dopo. Quando si usa Windows Server 2016, il server di cartelle di lavoro invia immediatamente una notifica ai client Windows 10 e le modifiche apportate ai file vengono sincronizzate immediatamente. Questa funzionalità è una novità di Windows Server 2016 e richiede un client Windows 10. Se si usa un client precedente o il server di cartelle di lavoro è Windows Server 2012 R2, il client continuerà a eseguire il polling ogni 10 minuti per le modifiche. |  
| Integrazione con Windows Information Protection (WIP) | Aggiunto a Windows 10 versione 1607 | Se un amministratore distribuisce WIP, le cartelle di lavoro possono applicare la protezione dei dati crittografando i dati nel computer. La crittografia usa una chiave associata all'ID organizzazione, che può essere cancellata in modalità remota usando un pacchetto di gestione di dispositivi mobili supportato, ad esempio Microsoft Intune. | 
  
## <a name="software-requirements"></a>Requisiti software

Per Cartelle di lavoro, è necessario che i file server e l'infrastruttura di rete soddisfino i requisiti software seguenti:  
  
-   Un server che esegue Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 per l'hosting di condivisioni di sincronizzazione con i file utente  
  
-   Un volume formattato con il file system NTFS per l'archiviazione dei file degli utenti  
  
-   Per applicare i criteri per le password ai computer con Windows 7, è necessario usare i criteri relativi alle password di Criteri di gruppo. È anche necessario escludere i computer con Windows 7 dai criteri per le password di Cartelle di lavoro (se in uso).

-   Un certificato server per ogni file server che ospiterà le cartelle di lavoro. Questi certificati devono provenire da un'autorità di certificazione (CA) considerata attendibile dagli utenti, idealmente una CA pubblica.

-   Opzionale Una foresta Active Directory Domain Services con le estensioni dello schema in Windows Server 2012 R2 per supportare il riferimento automatico di PC e dispositivi ai file server corretti quando si usano più file server.  
  
Per consentire agli utenti di eseguire la sincronizzazione tramite Internet, sono necessari requisiti aggiuntivi:  
  
-   La possibilità di rendere un server accessibile da Internet mediante la creazione di regole di pubblicazione nel proxy inverso o nel gateway di rete dell'organizzazione  
  
-   Opzionale Un nome di dominio registrato pubblicamente e la possibilità di creare record DNS pubblici aggiuntivi per il dominio  
  
-   (Facoltativo) L'infrastruttura di Active Directory Federation Services (ADFS) quando si utilizza l'autenticazione di ADFS  
  
Per Cartelle di lavoro, è necessario che i computer client soddisfino i requisiti software seguenti:  
  
-   I PC e i dispositivi devono eseguire uno dei sistemi operativi seguenti:  
  
    -   Windows 10  
  
    -   Windows 8.1  
  
    -   Windows RT 8.1  
  
    -   Windows 7  
  
    -   Android 4,4 KitKat e versioni successive  
  
    -   iOS 10.2 e versioni successive  
  
-   I computer con Windows 7 devono eseguire una delle edizioni seguenti di Windows:  
  
    -   Windows 7 Professional  
  
    -   Windows 7 Ultimate  
  
    -   Windows 7 Enterprise  
  
-   I PC Windows 7 devono essere aggiunti al dominio dell'organizzazione (non possono essere aggiunti a un gruppo di lavoro).  
  
-   Spazio disponibile sufficiente in un'unità locale formattata con NTFS per archiviare tutti i file dell'utente in cartelle di lavoro, oltre a 6 GB di spazio libero aggiuntivo se le cartelle di lavoro si trovano nell'unità di sistema, come per impostazione predefinita. Per impostazione predefinita, cartelle di lavoro usa il percorso seguente: **%USERPROFILE%\Work**  
  
     È comunque possibile modificare la posizione durante la configurazione. Le schede microSD e le unità USB formattate con il file system NTFS sono posizioni supportate anche se la sincronizzazione viene interrotta nel caso le unità vengano rimosse.  
  
     Per impostazione predefinita, le dimensioni massime consentite per ogni file sono 10 GB. Non esiste una limitazione dello spazio di archiviazione disponibile per ogni utente, anche se gli amministratori possono utilizzare la funzionalità relativa alle quote di Gestione risorse file server per implementare le quote.  
  
-   Cartelle di lavoro non supporta il rollback dello stato della macchina virtuale delle macchine virtuali client. È comunque possibile eseguire operazioni di backup e ripristino dalle macchine virtuali client mediante il backup dell'immagine del sistema o un'altra app per il backup.  
  
## <a name="work-folders-compared-to-other-sync-technologies"></a>Cartelle di lavoro rispetto ad altre tecnologie di sincronizzazione  

Nella tabella seguente viene illustrato come vengono posizionate le varie tecnologie di sincronizzazione Microsoft e quando utilizzarle.  
  
| | Cartelle di lavoro | File offline | OneDrive for Business | OneDrive |
| - | ------------------ | ------------------- | -------------------------- | -------------- |
| **Riepilogo tecnologia** | Sincronizza i file archiviati in un file server con PC e dispositivi | Sincronizza i file archiviati in un file server con i PC che hanno accesso alla rete aziendale (possono essere sostituiti da cartelle di lavoro) | Sincronizza i file archiviati in Office 365 o in SharePoint con PC e dispositivi all'interno o all'esterno di una rete aziendale e fornisce funzionalità di collaborazione dei documenti | Sincronizza i file personali archiviati in OneDrive con PC, computer Mac e dispositivi |
| **Progettato per fornire l'accesso utente ai file di lavoro** | Sì | Sì | Sì | No |
| **servizio cloud** | nessuno | nessuno | Office 365 | Microsoft OneDrive |
| **Server di rete interni** | File server che eseguono Windows Server 2012 R2 o Windows Server 2016 | File server | Server SharePoint (facoltativo) | nessuno |
| **Client supportati** | PC, iOS e Android | PC in una rete aziendale o connessi tramite DirectAccess, VPN o altre tecnologie di accesso remoto | PC, iOS, Android, Windows Phone | PC, computer Mac, Windows Phone, iOS e Android |
  
> [!NOTE]
>  Oltre alle tecnologie di sincronizzazione elencate nella tabella precedente, Microsoft offre altre tecnologie di replica, tra cui Replica DFS, progettato per la replica da server a server, e BranchCache, progettato come tecnologia di accelerazione WAN per le succursali. Per ulteriori informazioni, vedere Panoramica di [spazi dei nomi DFS e replica DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx) e [BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx) 
  
## <a name="server-manager-information"></a>Informazioni su Server Manager  

Cartelle di lavoro fa parte del ruolo Servizi file e archiviazione. È possibile installare cartelle di lavoro utilizzando l'aggiunta guidata ruoli e funzionalità o il `Install-WindowsFeature` cmdlet. Entrambi i metodi eseguono le operazioni seguenti:  
  
-   Aggiunge la pagina **cartelle di lavoro** ai **Servizi file e archiviazione** in Server Manager  
  
-   Installa il servizio condivisioni di sincronizzazione di Windows, utilizzato da Windows Server per ospitare le condivisioni di sincronizzazione  
  
-   Installa il modulo SyncShare di Windows PowerShell per gestire cartelle di lavoro nel server  
  
## <a name="interoperability-with-windows-azure-virtual-machines"></a>Interoperabilità con macchine virtuali di Microsoft Azure

 È possibile eseguire questo servizio ruolo di Windows Server in una macchina virtuale in Windows Azure. Questo scenario è stato testato con Windows Server 2012 R2 e Windows Server 2016.  
  
Per informazioni su come iniziare a usare le macchine virtuali di Windows Azure, visitare il [sito Web di Microsoft Azure](http://www.windowsazure.com/documentation/services/virtual-machines).  
  
## <a name="see-also"></a>Vedere anche

 Per altre informazioni correlate, vedere le risorse seguenti.  
  
| Tipo di contenuto | Riferimenti |
| ------------------ | ---------------- |
| **Valutazione del prodotto** | -   [Cartelle di lavoro per Android: rilasciata](https://blogs.technet.microsoft.com/filecab/2016/03/16/work-folders-for-android-released) (post di Blog)<br />-   [Cartelle di lavoro per iOS-versione app iPad](https://blogs.technet.com/b/filecab/archive/2015/01/16/work-folders-for-ios-ipad-app-release.aspx) (post di Blog)<br />-   [Introduzione a cartelle di lavoro in Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/09/introducing-work-folders-on-windows-server-2012-r2.aspx) (post di Blog)<br />-   [Introduzione alle cartelle di lavoro](https://channel9.msdn.com/posts/Introduction-to-Work-Folders) (video di Channel 9)<br />-   [Distribuzione di Cartelle di lavoro in un ambiente di testing](https://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (post di blog)<br />-   [Cartelle di lavoro per Windows 7](https://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (post di Blog) |
| **Distribuzione** | -   [Progettazione di un'implementazione di cartelle di lavoro](plan-work-folders.md)<br />-   [Distribuzione di cartelle di lavoro](deploy-work-folders.md)<br />-   [Distribuzione di cartelle di lavoro con AD FS e proxy applicazione Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [Distribuzione di cartelle di lavoro con Azure AD proxy di applicazione](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />- [Guida alla migrazione di File offline (CSC) a cartelle di lavoro](https://blogs.technet.microsoft.com/filecab/2016/08/12/offline-files-csc-to-work-folders-migration-guide/)<br />-   [Considerazioni sulle prestazioni per le distribuzioni di cartelle di lavoro](https://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [Cartelle di lavoro per Windows 7 (download di 64 bit)](https://www.microsoft.com/download/details.aspx?id=42558)<br />-   [Cartelle di lavoro per Windows 7 (download di 32 bit)](https://www.microsoft.com/download/details.aspx?id=42559) |
| **Operazioni** | -   [App iPad di cartelle di lavoro: domande frequenti](https://windows.microsoft.com/windows/work-folders-ipad-faq) (per gli utenti)<br />-   [Gestione certificati di cartelle di lavoro](https://blogs.technet.com/b/filecab/archive/2013/08/09/work-folders-certificate-management.aspx) (post di Blog)<br />-   [Monitoraggio delle distribuzioni di cartelle di lavoro di Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/10/15/monitoring-windows-server-2012-r2-work-folders-deployments.aspx) (post di Blog)<br />-   [Cmdlet SyncShare (cartelle di lavoro) in Windows PowerShell](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)<br />-   [Archiviazione e servizi file cmdlet di PowerShell scheda di riferimento rapido per Windows Server 2012 R2 Preview Edition](https://blogs.technet.com/b/filecab/archive/2013/07/30/storage-and-file-services-powershell-cmdlets-quick-reference-card-for-windows-server-2012-r2-preview-edition.aspx) |
| **Risoluzione dei problemi** | -   [Windows Server 2012 R2-risoluzione dei conflitti di porte con i siti Web e le cartelle di lavoro IIS](https://blogs.technet.com/b/filecab/archive/2013/10/15/windows-server-2012-r2-resolving-port-conflict-with-iis-websites-and-work-folders.aspx) (post di Blog)<br />-   [Errori comuni nelle cartelle di lavoro](https://social.technet.microsoft.com/wiki/contents/articles/30578.common-errors-in-work-folders.aspx) |
| **Risorse della community** | -   [Forum su Servizi file e archiviazione](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverfiles)<br />-   [Blog del team di archiviazione in Microsoft-file CAB](https://blogs.technet.com/b/filecab/)<br />-   [Blog del team di servizi directory](https://blogs.technet.com/b/askds/) |  
| **Tecnologie correlate** | -   [Archiviazione in Windows Server 2016](../storage.md)<br>-   [Servizi file e archiviazione](https://technet.microsoft.com/library/hh831487(v=ws.11).aspx)<br />-   [Gestione risorse file server](https://technet.microsoft.com/library/hh831701(v=ws.11).aspx)<br />-   [Reindirizzamento cartelle, File offline e profili utente mobili](https://technet.microsoft.com/library/hh848267(v=ws.11).aspx)<br />-   [BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx)<br />-   [Spazi dei nomi DFS e Replica DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx) |
