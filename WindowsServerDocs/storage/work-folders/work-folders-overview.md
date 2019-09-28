---
ms.assetid: c91c7196-ee0d-4856-8cfb-4c38494ccf1f
title: Panoramica di Cartelle di lavoro
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 06/07/2019
description: 'Una panoramica di Cartelle di lavoro: un ruolo del server in Windows Server che consente agli utenti di accedere ai file di lavoro da PC e dispositivi in modo uniforme.'
ms.openlocfilehash: 69c7319bf1bf777c2a06d4c3a0025220aa4b819d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365666"
---
# <a name="work-folders-overview"></a>Panoramica di Cartelle di lavoro

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

In questo argomento viene illustrata la funzionalità Cartelle di lavoro, un servizio ruolo per file server che eseguono Windows Server, che offre agli utenti l'accesso ai loro file di lavoro dai propri PC e dispositivi in modo uniforme.  
  
Se si vuole scaricare o usare cartelle di lavoro in Windows 10, Windows 7 o un dispositivo Android o iOS, vedere gli argomenti seguenti:

- [Cartelle di lavoro per Windows 10](https://support.microsoft.com/help/12370/windows-10-work-folders)
- [Cartelle di lavoro per Windows 7 (download di 64 bit)](https://www.microsoft.com/download/details.aspx?id=42558)
- [Cartelle di lavoro per Windows 7 (download di 32 bit)](https://www.microsoft.com/download/details.aspx?id=42559)
- [Cartelle di lavoro per iOS](https://itunes.apple.com/app/work-folders/id950878067)
- [Cartelle di lavoro per Android](https://play.google.com/store/apps/details?id=com.microsoft.workfolders)

## <a name="role-description"></a>Descrizione del ruolo

 Cartelle di lavoro consente agli utenti di archiviare e accedere ai file di lavoro su computer e dispositivi personali, spesso chiamati dispositivi BYOD (Bring Your Own Device), oltre che sui PC aziendali. Gli utenti avranno a disposizione una posizione in cui archiviare i file di lavoro a cui possono accedere da qualsiasi luogo. Le organizzazioni mantengono il controllo sui dati aziendali archiviando i file in file server gestiti centralmente e, facoltativamente, specificando criteri per i dispositivi degli utenti, ad esempio le password di crittografia e schermata di blocco.  
  
 È possibile distribuire Cartelle di lavoro con le distribuzioni esistenti delle cartelle di Reindirizzamento cartelle, File offline e home directory. In Cartelle di lavoro vengono archiviati i file utente in una cartella sul server denominata *condivisione di sincronizzazione*. È possibile specificare una cartella che contiene già dati sugli utenti e che consenta di adottare Cartelle di lavoro senza eseguire la migrazione di server e dati o eliminare immediatamente e gradualmente la soluzione esistente.  
  
## <a name="practical-applications"></a>Applicazioni pratiche

 Gli amministratori possono utilizzare Cartelle di lavoro per offrire agli utenti l'accesso ai loro file di lavoro pur mantenendo l'archiviazione centralizzata e il controllo sui dati dell'organizzazione. Alcune applicazioni specifiche per Cartelle di lavoro includono:  
  
-   Fornire un singolo punto di accesso ai file di lavoro da computer e dispositivi di lavoro e personali di un utente  
  
-   Accedere ai file di lavoro in modalità offline, quindi effettuare la sincronizzazione con il file server centrale quando il PC o il dispositivo accanto dispongono della connettività Internet o intranet  
  
-   Distribuire con le distribuzioni esistenti di cartelle di Reindirizzamento cartelle, File offline e home directory  
  
-   Usare tecnologie di gestione di file server esistenti, ad esempio classificazione di file e quote di cartelle, per gestire i dati sugli utenti  
  
-   Specificare i criteri di sicurezza per richiedere a PC e dispositivi dell'utente di crittografare Cartelle di lavoro e usare una password per la schermata di blocco  
  
-   Utilizzare Clustering di failover con Cartelle di lavoro per offrire una soluzione a disponibilità elevata  
  
## <a name="important-functionality"></a>Funzionalità importanti

 Cartelle di lavoro include la seguente funzionalità.  
  
| Funzionalità | Disponibilità | Descrizione |  
| ------------------- | ------------------ | ----------------- |  
| Servizio ruolo di Cartelle di lavoro in Server Manager | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 | I servizi file e archiviazione offrono un modo per configurare condivisioni di sincronizzazione (cartelle in cui vengono archiviati i file di lavoro dell'utente), consentono di monitorare Cartelle di lavoro e di gestire condivisioni di sincronizzazione e accesso degli utenti |
| Cmdlet di Cartelle di lavoro | Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 | Un modulo Windows PowerShell che contiene i cmdlet completi per la gestione dei server di Cartelle di lavoro |  
| Integrazione di Cartelle di lavoro con Windows | Windows 10<br /><br /> Windows 8.1<br /><br /> Windows RT 8.1<br /><br /> Windows 7 (download necessario) | Cartelle di lavoro offre la seguente funzionalità nei computer Windows:<br /><br /> -   Un elemento del Pannello di controllo che consente di configurare e monitorare Cartelle di lavoro<br />-   Integrazione di Esplora file che consente di accedere facilmente ai file in Cartelle di lavoro<br />-   Un motore di sincronizzazione che consente di trasferire file verso e da un file server centrale ottimizzando al contempo la durata della batteria e le prestazioni del sistema |
| App Cartelle di lavoro per dispositivi | Android<br /><br /> iPhone e iPad® Apple | Un'app che consente ai dispositivi comuni di accedere ai file in Cartelle di lavoro |  
  
## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate
  
Nella tabella riportata di seguito vengono descritte alcune delle modifiche principali apportate alla funzionalità Cartelle di lavoro.  
  
| Caratteristica/funzionalità | Novità o aggiornamento | Descrizione |
| ---------------------------- | --------------------- | ----------------- |
| Supporto del proxy applicazione di Azure AD | Aggiunto a Windows 10 versione 1703, Android, iOS | Gli utenti remoti possono accedere in modo sicuro ai file nel server di Cartelle di lavoro con il proxy applicazione di Azure AD. |
| Modifica più veloce della replica | Aggiornata in Windows 10 e Windows Server 2016 | Con Windows Server 2012 R2, quando le modifiche ai file vengono sincronizzate con il server di Cartelle di lavoro, i client non ricevono una notifica delle modifiche e l'aggiornamento può arrivare anche 10 minuti dopo. Quando si usa Windows Server 2016, il server di cartelle di lavoro invia immediatamente una notifica ai client Windows 10 e le modifiche apportate ai file vengono sincronizzate immediatamente. Questa funzionalità è una novità in Windows Server 2016 e richiede un client di Windows 10. Se si usa un client precedente o il server Cartelle di lavoro è Windows Server 2012 R2, il client continuerà a eseguire il polling delle modifiche ogni 10 minuti. |  
| Integrato con Windows Information Protection (WIP) | Aggiunto a Windows 10 versione 1607 | Se un amministratore distribuisce WIP, Cartelle di lavoro consente di applicare la protezione dei dati crittografando questi ultimi nel PC. La crittografia utilizza una chiave associata all'ID aziendale, che può essere cancellata in remoto utilizzando un pacchetto di gestione di dispositivi mobili supportato, ad esempio Microsoft Intune. |  
| Integrazione di Microsoft Office | Aggiunto a Windows 10 versione 1511 | In Windows 8.1 è possibile spostarsi a Cartelle di lavoro all'interno delle app di Office facendo clic su o toccando Questo PC, quindi spostandosi al percorso di Cartelle di lavoro nel PC. In Windows 10 è possibile semplificare ulteriormente l'accesso a Cartelle di lavoro, aggiungendolo all'elenco di percorsi che Office mostra durante il salvataggio o l'apertura di file. Per altre info, consultare [Cartelle di lavoro in Windows 10](https://windows.microsoft.com/windows-10/work-folders-in-windows-10) e [Risoluzione dei problemi utilizzando Cartelle di lavoro come un luogo in Microsoft Office](https://social.technet.microsoft.com/wiki/contents/articles/32881.troubleshooting-using-work-folders-as-a-place-in-microsoft-office.aspx). |  
  
## <a name="software-requirements"></a>Requisiti software

Per Cartelle di lavoro, è necessario che i file server e l'infrastruttura di rete soddisfino i requisiti software seguenti:  
  
-   Un server che esegue Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2 per l'hosting di condivisioni di sincronizzazione con i file utente  
  
-   Un volume formattato con il file system NTFS per l'archiviazione dei file degli utenti  
  
-   Per applicare i criteri per le password ai computer con Windows 7, è necessario usare i criteri relativi alle password di Criteri di gruppo. È anche necessario escludere i computer con Windows 7 dai criteri per le password di Cartelle di lavoro (se in uso).

-   Un certificato server per ogni file server che ospiterà Cartelle di lavoro. Questi certificati devono essere emessi da un'autorità di certificazione (CA) considerata attendibile dagli utenti, nel caso ideale una CA pubblica.

-   Opzionale Una foresta Active Directory Domain Services con le estensioni dello schema in Windows Server 2012 R2 per supportare il riferimento automatico di PC e dispositivi ai file server corretti quando si usano più file server.  
  
Per consentire agli utenti di eseguire la sincronizzazione tramite Internet, sono necessari requisiti aggiuntivi:  
  
-   La possibilità di rendere un server accessibile da Internet creando regole di pubblicazione nel proxy inverso o nel gateway di rete dell'organizzazione  
  
-   (Facoltativo) Un nome di dominio registrato pubblicamente e la possibilità di creare altri record DNS pubblici per il dominio  
  
-   (Facoltativo) L'infrastruttura di Active Directory Federation Services (AD FS) quando si utilizza l'autenticazione di AD FS  
  
Per Cartelle di lavoro, è necessario che i computer client soddisfino i requisiti software seguenti:  
  
-   È necessario che in PC e dispositivi sia eseguito uno dei sistemi operativi seguenti:  
  
    -   Windows 10  
  
    -   Windows 8.1  
  
    -   Windows RT 8.1  
  
    -   Windows 7  
  
    -   KitKat Android 4.4 e versioni successive  
  
    -   iOS 10.2 e versioni successive  
  
-   I computer con Windows 7 devono eseguire una delle edizioni seguenti di Windows:  
  
    -   Windows 7 Professional  
  
    -   Windows 7 Ultimate  
  
    -   Windows 7 Enterprise  
  
-   I computer con Windows 7 devono appartenere al dominio dell'organizzazione. Non possono appartenere a un gruppo di lavoro.  
  
-   Spazio libero sufficiente in un'unità locale formattata con NTFS per l'archiviazione di tutti i file degli utenti in Cartelle di lavoro oltre a 6 GB di spazio libero aggiuntivo se Cartelle di lavoro si trova nell'unità di sistema, come succede per impostazione predefinita. Cartelle di lavoro utilizza il seguente percorso per impostazione predefinita: **%PROFILOUTENTE%\Cartelle di lavoro**  
  
     È comunque possibile modificare la posizione durante la configurazione. Le schede microSD e le unità USB formattate con il file system NTFS sono posizioni supportate anche se la sincronizzazione viene interrotta nel caso le unità vengano rimosse.  
  
     Per impostazione predefinita, le dimensioni massime consentite per ogni file sono 10 GB. Non esiste una limitazione dello spazio di archiviazione disponibile per ogni utente, anche se gli amministratori possono utilizzare la funzionalità relativa alle quote di Gestione risorse file server per implementare le quote.  
  
-   Cartelle di lavoro non supporta il ripristino dello stato precedente delle macchine virtuali client. È comunque possibile eseguire operazioni di backup e ripristino dalle macchine virtuali client mediante il backup dell'immagine del sistema o un'altra app per il backup.  
  
## <a name="work-folders-compared-to-other-sync-technologies"></a>Cartelle di lavoro rispetto alle altre tecnologie di sincronizzazione  

Nella tabella seguente è illustrato come le varie tecnologie di sincronizzazione di Microsoft vengono posizionate e quando utilizzarle.  
  
| | Cartelle di lavoro | File offline | OneDrive for Business | OneDrive |
| - | ------------------ | ------------------- | -------------------------- | -------------- |
| **Riepilogo tecnologia** | Consente di sincronizzare i file archiviati su un file server con PC e dispositivi | Consente di sincronizzare i file archiviati su un file server con PC che hanno accesso alla rete aziendale (può essere sostituito da Cartelle di lavoro) | Consente di sincronizzare i file archiviati in Office 365 o in SharePoint con PC e dispositivi all'interno o all'esterno di una rete aziendale, e di offrire la funzionalità di condivisione dei documenti | Consente di sincronizzare i file personali archiviati in OneDrive con PC, computer e dispositivi Mac |
| **Progettato per fornire l'accesso utente ai file di lavoro** | Yes | Yes | Yes | No |
| **Servizio cloud** | Nessuno | Nessuno | Office 365 | Microsoft OneDrive |
| **Server di rete interni** | File server che eseguono Windows Server 2012 R2 o Windows Server 2016 | File server | Server SharePoint (facoltativo) | Nessuno |
| **Client supportati** | PC, iOS, Android | PC di una rete aziendale o connessi tramite DirectAccess, VPN o altre tecnologie di accesso remoto | PC, iOS, Android, Windows Phone | PC, computer Mac, Windows Phone, iOS, Android |
  
> [!NOTE]
>  Oltre alle tecnologie di sincronizzazione elencate nella tabella precedente, Microsoft offre altre tecnologie di replica, tra cui Replica DFS, progettato per la replica da server a server, e BranchCache, progettato come una tecnologia di accelerazione WAN per succursali. Per altre informazioni, consultare [Spazi dei nomi DFS e Replica DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx) e [Panoramica di BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx) 
  
## <a name="server-manager-information"></a>Informazioni su Server Manager  

Cartelle di lavoro fa parte del ruolo Servizi file e archiviazione. È possibile installare Cartelle di lavoro tramite la procedura guidata Aggiungi ruoli e funzionalità o `Install-WindowsFeature` cmdlet. Entrambi i metodi consentono di ottenere quanto segue:  
  
-   Consente di aggiungere la pagina **Cartelle di lavoro** a **Servizi file e archiviazione** in Server Manager  
  
-   Consente di installare il servizio Condivisioni di sincronizzazione di Windows, che viene utilizzato da Windows Server per ospitare le condivisioni di sincronizzazione  
  
-   Consente di installare il modulo Windows PowerShell di SyncShare per gestire Cartelle di lavoro sul server  
  
## <a name="interoperability-with-windows-azure-virtual-machines"></a>Interoperabilità con macchine virtuali Windows Azure

 È possibile eseguire questo servizio ruolo di Windows Server in una macchina virtuale in Windows Azure. Questo scenario è stato testato con Windows Server 2012 R2 e Windows Server 2016.  
  
Per altre informazioni introduttive sulle macchine virtuali di Windows Azure, visitare il [sito Web di Windows Azure](http://www.windowsazure.com/documentation/services/virtual-machines).  
  
## <a name="see-also"></a>Vedere anche

 Per altre informazioni correlate, vedere le risorse seguenti.  
  
| Tipo di contenuto | Riferimenti |
| ------------------ | ---------------- |
| **Valutazione del prodotto** | -   [cartelle di lavoro per Android-rilasciato](https://blogs.technet.microsoft.com/filecab/2016/03/16/work-folders-for-android-released) (post di Blog)<br />-   [cartelle di lavoro per iOS-versione app iPad](https://blogs.technet.com/b/filecab/archive/2015/01/16/work-folders-for-ios-ipad-app-release.aspx) (post di Blog)<br />-   [Introduzione a cartelle di lavoro in Windows Server 2012 R2](http://blogs.technet.com/b/filecab/archive/2013/07/09/introducing-work-folders-on-windows-server-2012-r2.aspx) (post di Blog)<br />-   [Introduzione a cartelle di lavoro](http://channel9.msdn.com/posts/Introduction-to-Work-Folders) (video di Channel 9)<br />-   [Distribuzione Lab di test di cartelle di lavoro](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (post di Blog)<br />[cartelle di lavoro -    per Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (post di Blog) |
| **Distribuzione** | -   [Progettazione di un'implementazione di cartelle di lavoro](plan-work-folders.md)<br />-   [distribuzione di cartelle di lavoro](deploy-work-folders.md)<br />-   [Distribuzione di cartelle di lavoro con AD FS e proxy applicazione Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [distribuzione di cartelle di lavoro con Azure ad proxy di applicazione](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />[Guida alla migrazione di -  file offline (CSC) a cartelle di lavoro](https://blogs.technet.microsoft.com/filecab/2016/08/12/offline-files-csc-to-work-folders-migration-guide/)<br />[considerazioni sulle prestazioni @no__t 0 per le distribuzioni di cartelle di lavoro](https://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />[cartelle di lavoro -    per Windows 7 (download di 64 bit)](https://www.microsoft.com/download/details.aspx?id=42558)<br />[cartelle di lavoro -    per Windows 7 (download di 32 bit)](https://www.microsoft.com/download/details.aspx?id=42559) |
| **Operazioni** | -    @ no__t-1Work cartella app iPad: Domande frequenti @ no__t-0 (per gli utenti)<br />[Gestione certificati di cartelle di lavoro](https://blogs.technet.com/b/filecab/archive/2013/08/09/work-folders-certificate-management.aspx) -    (post di Blog)<br />-   [monitoraggio delle distribuzioni di cartelle di lavoro di Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/10/15/monitoring-windows-server-2012-r2-work-folders-deployments.aspx) (post di Blog)<br />Cmdlet -   [SyncShare (cartelle di lavoro) in Windows PowerShell](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)<br />-   [archiviazione e servizi file cmdlet di PowerShell scheda di riferimento rapido per Windows Server 2012 R2 Preview Edition](http://blogs.technet.com/b/filecab/archive/2013/07/30/storage-and-file-services-powershell-cmdlets-quick-reference-card-for-windows-server-2012-r2-preview-edition.aspx) |
| **Risoluzione dei problemi** | -   [Windows Server 2012 R2-risoluzione dei conflitti di porte con i siti Web e le cartelle di lavoro di IIS](https://blogs.technet.com/b/filecab/archive/2013/10/15/windows-server-2012-r2-resolving-port-conflict-with-iis-websites-and-work-folders.aspx) (post di Blog)<br />Errori comuni -   [in cartelle di lavoro](https://social.technet.microsoft.com/wiki/contents/articles/30578.common-errors-in-work-folders.aspx) |
| **Risorse della community** | [Forum su servizi File](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverfiles) -    e archiviazione<br />-   [il team di archiviazione nel Blog di Microsoft-file CAB](http://blogs.technet.com/b/filecab/)<br />-   [Chiedi al Blog del team di servizi directory](http://blogs.technet.com/b/askds/) |  
| **Tecnologie correlate** | Archiviazione @no__t 0[in Windows Server 2016](../storage.md)<br>[Servizi file e archiviazione](https://technet.microsoft.com/library/hh831487(v=ws.11).aspx) -   <br />[Gestione risorse file Server](https://technet.microsoft.com/library/hh831701(v=ws.11).aspx) -   <br />[Reindirizzamento cartelle @no__t 0, file offline e profili utente mobili](https://technet.microsoft.com/library/hh848267(v=ws.11).aspx)<br />-   [BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx)<br />[spazi dei nomi DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx) -    e replica DFS |
