---
ms.assetid: a7c39656-81ee-4c2b-80ef-4d017dd11b07
title: Pianificazione di una distribuzione di Cartelle di lavoro
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 4/5/2017
description: Come pianificare una distribuzione di Cartelle di lavoro, tra cui requisiti di sistema e modalità di preparazione dell'ambiente di rete.
ms.openlocfilehash: e62cd61350299461d725c5d84209230ce1cc41a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365738"
---
# <a name="planning-a-work-folders-deployment"></a>Pianificazione di una distribuzione di Cartelle di lavoro

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

In questo argomento viene illustrato il processo di progettazione per un'implementazione di Cartelle di lavoro supponendo che l'utente disponga delle conoscenze seguenti:  
  
- Una conoscenza di base di Cartelle di lavoro (ovvero delle informazioni disponibili in [Cartelle di lavoro](work-folders-overview.md))  
  
- Una conoscenza di base dei concetti di Active Directory Domain Services (AD DS)  
  
- Una conoscenza di base della condivisione file di Windows e delle tecnologie correlate  
  
- Una conoscenza di base dell'utilizzo dei certificati SSL  
  
- Una conoscenza di base sull'abilitazione dell'accesso Web alle risorse interne mediante un proxy inverso Web  
  
  Nelle sezioni seguenti sono disponibili indicazioni su come progettare un'implementazione di Cartelle di lavoro. La distribuzione di Cartelle di lavoro è illustrata nell'argomento seguente, ovvero [Distribuzione di Cartelle di lavoro](deploy-work-folders.md).  
  
##  <a name="BKMK_SOFT"></a>Requisiti software  

Per Cartelle di lavoro, è necessario che i file server e l'infrastruttura di rete soddisfino i requisiti software seguenti:  
  
-   Un server che esegue Windows Server 2012 R2 o Windows Server 2016 per l'hosting delle condivisioni di sincronizzazione con file dell'utente  
  
-   Un volume formattato con il file system NTFS per l'archiviazione dei file degli utenti  
  
-   Per applicare i criteri per le password ai computer con Windows 7, è necessario usare i criteri relativi alle password di Criteri di gruppo. È anche necessario escludere i computer con Windows 7 dai criteri per le password di Cartelle di lavoro (se in uso).

-   Un certificato server per ogni file server che ospiterà Cartelle di lavoro. Questi certificati devono essere emessi da un'autorità di certificazione (CA) considerata attendibile dagli utenti, nel caso ideale una CA pubblica.

-   (Facoltativo) Una foresta di Active Directory Domain Services con estensioni dello schema in Windows Server 2012 R2 per supportare il riferimento automatico di computer e dispositivi al file server corretto quando si usano più file server. 
  
Per consentire agli utenti di eseguire la sincronizzazione tramite Internet, sono necessari requisiti aggiuntivi:  
  
-   La possibilità di rendere un server accessibile da Internet creando regole di pubblicazione nel proxy inverso o nel gateway di rete dell'organizzazione  
  
-   (Facoltativo) Un nome di dominio registrato pubblicamente e la possibilità di creare altri record DNS pubblici per il dominio  
  
-   (Facoltativo) L'infrastruttura di Active Directory Federation Services (AD FS) quando si utilizza l'autenticazione di AD FS  
  
Per Cartelle di lavoro, è necessario che i computer client soddisfino i requisiti software seguenti:  
  
-   Nei computer deve essere in esecuzione uno dei sistemi operativi seguenti:  
  
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
  
> [!NOTE]
>  Verificare di installare l'aggiornamento cumulativo della disponibilità generale di Windows 8.1 e Windows Server 2012 R2 su tutti i server di Cartelle di lavoro e qualsiasi computer client che esegua Windows 8.1 o Windows Server 2012 R2. Per ulteriori informazioni, vedere l'[articolo 2883200](https://support.microsoft.com/kb/2883200) della Microsoft Knowledge Base.  
  
## <a name="deployment-scenarios"></a>Scenari di distribuzione  
 È possibile implementare Cartelle di lavoro in un numero qualsiasi di file server in un ambiente del cliente. Ciò consente di adattare l'implementazione di Cartelle di lavoro alle esigenze del cliente creando distribuzioni estremamente personalizzate. La maggior parte delle distribuzioni rientra comunque in uno dei tre scenari di base seguenti.  
  
### <a name="single-site-deployment"></a>Distribuzione in un singolo sito  
 In questo tipo di distribuzione i file server sono ospitati in un sito centrale dell'infrastruttura del cliente. La distribuzione in un singolo sito viene utilizzata per lo più per i clienti che dispongono di un'infrastruttura altamente centralizzata o di piccole succursali che non utilizzano file server locali. Questo modello di distribuzione può risultare più semplice da amministrare per il personale IT poiché tutte le risorse server sono locali ed è probabile che anche le operazioni di entrata o uscita tramite Internet siano centralizzate in questa posizione. Questo modello di distribuzione si basa inoltre su una buona connettività WAN tra il sito centrale e ogni succursale e per gli utenti delle succursali può verificarsi un'interruzione del servizio a causa delle condizioni della rete.  
  
### <a name="multiple-site-deployment"></a>Distribuzione in più siti  
 In questo tipo di distribuzione, i file server sono ospitati in più posizioni dell'infrastruttura del cliente. Ciò significa la presenza di più centri dati o di succursali con file server individuali. La distribuzione in più siti viene utilizzata per lo più in ambienti di grandi dimensioni o per clienti che dispongono di diverse succursali di grandi dimensioni che gestiscono risorse server locali. Questo modello di distribuzione risulta più complesso da amministrare per il personale IT e si basa su un preciso coordinamento degli archivi dati e sulla gestione di Active Directory Domain Services (AD DS) per fare in modo che gli utenti utilizzino il server di sincronizzazione corretto per Cartelle di lavoro.  
  
### <a name="hosted-deployment"></a>Distribuzione ospitata  
 In questo tipo di distribuzione i server di sincronizzazione sono distribuiti in una soluzione IAAS (Infrastructure-as-a-Service) come Macchine virtuali Windows Azure. Il vantaggio di questo metodo di distribuzione consiste nel fatto di rendere la disponibilità dei file server meno dipendente dalla connettività WAN all'interno dell'azienda di un cliente. Se un dispositivo è in grado di connettersi a Internet, può raggiungere il proprio server di sincronizzazione. I server distribuiti nell'ambiente ospitato devono comunque essere in grado di raggiungere il dominio di Active Directory dell'organizzazione per autenticare gli utenti e per i requisiti dell'infrastruttura del cliente è necessaria una complessità aggiuntiva in locale per gestire tale connessione.  
  
## <a name="deployment-technologies"></a>Tecnologie di distribuzione  
 Le distribuzioni di Cartelle di lavoro sono costituite da varie tecnologie che collaborano per fornire il servizio ai dispositivi presenti sia nella rete interna che in quella esterna. Prima di progettare una distribuzione di Cartelle di lavoro, i clienti devono conoscere i requisiti di ciascuna delle tecnologie seguenti.  
  
### <a name="active-directory-domain-services"></a>Servizi di dominio di Active Directory  
 AD DS fornisce due servizi importanti in una distribuzione di Cartelle di lavoro. In qualità di back-end per l'autenticazione di Windows, AD DS fornisce innanzitutto i servizi di sicurezza e autenticazione utilizzati per consentire l'accesso ai dati degli utenti. Se un controller di dominio non può essere raggiunto, un file server non potrà autenticare una richiesta in ingresso e il dispositivo non sarà in grado di accedere ai dati archiviati nella condivisione di sincronizzazione di tale file server.  
  
 AD DS (con l'aggiornamento dello schema di Windows Server 2012 R2) consente inoltre di gestire per ogni utente l'attributo msDS-SyncServerURL, che permette di indirizzare automaticamente gli utenti al server di sincronizzazione appropriato.  
  
### <a name="file-servers"></a>File server  
 I file server che eseguono Windows Server 2012 R2 o Windows Server 2016, ospitano il servizio ruolo Cartelle di lavoro e le condivisioni di sincronizzazione in cui sono archiviati i dati di Cartelle di lavoro degli utenti. I file server possono inoltre ospitare i dati archiviati da altre tecnologie in uso nella rete interna, ad esempio condivisioni di file, e possono essere inseriti in un cluster in modo da fornire la tolleranza di errore per i dati degli utenti.  
  
###  <a name="GroupPolicy"></a>Criteri di gruppo  
 Se nell'ambiente sono disponibili computer con Windows 7, è consigliabile attenersi alle indicazioni seguenti:  
  
- Usare Criteri di gruppo per controllare i criteri per le password per tutti i computer appartenenti al dominio che usano Cartelle di lavoro.  
  
- Usare il criterio **Blocca automaticamente lo schermo e richiedi una password** di Cartelle di lavoro nei computer che non appartengono al dominio.  
  
  È anche possibile usare Criteri di gruppo per specificare un server di Cartelle di lavoro per i computer che fanno parte del dominio. In questo modo la configurazione di Cartelle di lavoro risulta più semplice. In caso contrario, gli utenti dovrebbero infatti accedere ai propri indirizzi di posta elettronica di lavoro per controllare le impostazioni (supponendo che Cartelle di lavoro sia configurato correttamente) oppure accedere all'URL di Cartelle di lavoro fornito loro mediante posta elettronica o un altro mezzo di comunicazione.  
  
  È inoltre possibile utilizzare Criteri di gruppo per forzare la configurazione di Cartelle di lavoro per utente o per computer, anche se questa operazione causa la sincronizzazione di Cartelle di lavoro in ogni computer a cui un utente accede (se si utilizza l'impostazione del criterio per utente) e impedisce agli utenti di specificare una posizione alternativa per Cartelle di lavoro nel proprio computer (ad esempio in una scheda microSD per risparmiare spazio di archiviazione nell'unità principale). Prima di forzare la configurazione automatica, è pertanto consigliabile valutare attentamente le proprie esigenze.  
  
### <a name="windows-intune"></a>Windows Intune  
 Windows Intune fornisce un livello di sicurezza e gestibilità non altrimenti disponibile per i dispositivi che non fanno parte del dominio. È possibile utilizzare Windows Intune per configurare e gestire i dispositivi personali degli utenti, ad esempio i tablet che si connettono a Cartelle di lavoro tramite Internet. Windows Intune può fornire i dispositivi con l'URL del server di sincronizzazione da usare. in caso contrario, gli utenti devono immettere l'indirizzo di posta elettronica dell'ufficio per cercare le impostazioni (se si pubblica un URL di cartelle di lavoro pubbliche nel formato https://workfolders. <em>contoso.com</em>) oppure immettere direttamente l'URL del server di sincronizzazione.  
  
 Se non si distribuisce Windows Intune, gli utenti devono configurare manualmente i dispositivi esterni, operazione che può causare l'aumento delle richieste di assistenza al personale dell'help desk.  
  
 È inoltre possibile utilizzare Windows Intune per cancellare selettivamente i dati di Cartelle di lavoro dal dispositivo di un utente senza influire sul resto dei dati. Questa funzionalità può ad esempio risultare utile se un utente lascia l'organizzazione o subisce il furto del proprio dispositivo.  
  
### <a name="web-application-proxyazure-ad-application-proxy"></a>Proxy applicazioni Web/Proxy applicazione di Azure AD  
 Cartelle di lavoro si basa sul concetto di consentire ai dispositivi connessi a Internet di recuperare dati aziendali dalla rete interna in modo sicuro, per fare in modo che gli utenti possano avere i dati sempre a portata di mano su tablet e dispositivi che non potrebbero altrimenti accedere ai file di lavoro. A tale scopo, è necessario utilizzare un proxy inverso per pubblicare gli URL dei server di sincronizzazione e renderli disponibili per i client Internet. 
 
Cartelle di lavoro supporta l'utilizzo di Proxy applicazione Web, Proxy applicazione di Azure AD o soluzioni di proxy inversi di terze parti: 

-  Proxy applicazione Web è una soluzione di proxy inverso in locale. Per informazioni, vedere [Proxy applicazione Web in Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).  
  
-  Proxy applicazione di Azure AD è una soluzione di proxy inverso cloud. Per ulteriori informazioni, vedere [Come fornire l'accesso remoto sicuro alle applicazioni locali](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

## <a name="additional-design-considerations"></a>Ulteriori considerazioni relative alla progettazione  
 Oltre a comprendere i componenti illustrati in precedenza, i clienti dovrebbero dedicare un po' di tempo per pensare al numero di server e di condivisioni di sincronizzazione che desiderano installare e se sfruttare o meno il cluster di failover per fornire la tolleranza di errore in tali server di sincronizzazione.  
  
### <a name="number-of-sync-servers"></a>Numero di server di sincronizzazione  
 Ogni cliente può utilizzare più server di sincronizzazione in un solo ambiente. Questa è la configurazione consigliata per diversi motivi:  
  
- Distribuzione geografica degli utenti: ad esempio, file server delle succursali o centri dati distinti in base all'area geografica  
  
- Requisiti dell'archiviazione dati: determinati reparti aziendali possono avere specifiche esigenze di archiviazione o gestione dei dati che risultano più semplici con un server dedicato  
  
- Bilanciamento del carico: in ambienti di grandi dimensioni l'archiviazione dei dati degli utenti in più server può contribuire a migliorare le prestazioni e il tempo di attività del server.  
  
  Per informazioni sulla scalabilità e le prestazioni del server di Cartelle di lavoro, vedere le [considerazioni sulle prestazioni delle distribuzioni di Cartelle di lavoro](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx).  
  
> [!NOTE]
>  Quando si utilizzano più server di sincronizzazione, è consigliabile configurare l'individuazione automatica del server per gli utenti. Questo processo si basa sulla configurazione di un attributo in ogni account utente di AD DS. L'attributo è denominato **msDS-SyncServerURL** ed è disponibile negli account utente dopo l'aggiunta al dominio di un controller di dominio di Windows Server 2012 R2 oppure dopo l'applicazione degli aggiornamenti dello schema di Active Directory. Questo attributo deve essere impostato per ogni utente per fare in modo che gli utenti si connettano al server di sincronizzazione appropriato. L'individuazione automatica dei server consente alle organizzazioni di pubblicare cartelle di lavoro con un URL "descrittivo", ad esempio *https://workfolders.contoso.com* , indipendentemente dal numero di server di sincronizzazione in uso.  
  
### <a name="number-of-sync-shares"></a>Numero di condivisioni di sincronizzazione  
 Un server di sincronizzazione può gestire più condivisioni di sincronizzazione. Questa configurazione può risultare utile per diversi motivi:  
  
-   Requisiti di controllo e sicurezza: se i dati utilizzati da un determinato reparto devono essere controllati in modo più specifico o conservati per un periodo di tempo più lungo, gli amministratori possono utilizzare condivisioni di sincronizzazione distinte per tenere separate le cartelle degli utenti con livelli di controllo diversi.  
  
-   Quote o screening dei file diversi: se si desidera impostare quote di archiviazione diverse oppure distinguere i tipi di file consentiti in Cartelle di lavoro in base a gruppi di utenti diversi, può risultare utile usare condivisioni di sincronizzazione distinte.  
  
-   Controllo dei reparti: se i compiti amministrativi sono suddivisi in base al reparto, l'utilizzo di condivisioni distinte per ogni reparto consente agli amministratori di applicare quote o altri criteri.  
  
-   Distinzione dei criteri di dispositivi: se in un'organizzazione è necessario utilizzare più criteri per i dispositivi (ad esempio crittografando Cartelle di lavoro) per gruppi di utenti distinti, l'uso di più condivisioni consente di ottenere questo risultato.  
  
-   Capacità di archiviazione: se in un file server sono disponibili più volumi, è possibile utilizzare condivisioni aggiuntive per sfruttare tali volumi. Una singola condivisione può infatti accedere solo al volume in cui è ospitata e non può sfruttare i vantaggi offerti dagli altri volumi del file server.  
  
#### <a name="access-to-sync-shares"></a>Accesso alle condivisioni di sincronizzazione  

Mentre il server di sincronizzazione a cui un utente accede è determinato dall'URL immesso nel client (oppure dall'URL pubblicato per tale utente in AD DS se si utilizza l'individuazione automatica del server), l'accesso alle singole condivisioni di sincronizzazione è determinato dalle autorizzazioni impostate per le condivisioni.  
  
Se un cliente ospita più condivisioni di sincronizzazione nello stesso server, è pertanto necessario assicurarsi che i singoli utenti dispongano delle autorizzazioni per accedere a una sola di queste condivisioni. In caso contrario, quando gli utenti si connettono al server, è possibile che il loro client si connetta alla condivisione sbagliata. Per evitare questo problema, è possibile creare un gruppo di sicurezza distinto per ogni condivisione di sincronizzazione.  
  
L'accesso alla cartella di un singolo utente in una condivisione di sincronizzazione è determinato dai diritti di proprietà per tale cartella. Per impostazione predefinita, quando si crea una condivisione di sincronizzazione, Cartelle di lavoro concede agli utenti l'accesso esclusivo ai propri file disabilitando l'ereditarietà e rendendoli proprietari delle proprie cartelle.  
  
## <a name="design-checklist"></a>Elenco di controllo della progettazione  

L'insieme di domande elencate di seguito possono essere utilizzate dai clienti per progettare l'implementazione di Cartelle di lavoro che meglio si adatta al proprio ambiente. È consigliabile esaminare questo elenco di controllo prima di tentare di distribuire i server.  
  
-   Destinatari  
  
    -   Quali utenti utilizzeranno Cartelle di lavoro?  
  
    -   In che modo sono organizzati gli utenti? (per area geografica, in base all'ufficio, in base al reparto e così via)  
  
    -   Sono presenti utenti con esigenze particolari di archiviazione, sicurezza o conservazione dei dati?  
  
    -   Sono presenti utenti con esigenze specifiche per i criteri dei dispositivi, ad esempio la crittografia?  
  
    -   Quali computer client e dispositivi è necessario supportare? (Windows 8.1, Windows RT 8.1, Windows 7)  
  
         Se si supportano computer con Windows 7 e si vogliono usare i criteri per le password, escludere dai criteri per le password di Cartelle di lavoro il dominio in cui sono archiviati gli account dei computer e usare invece i criteri per le password di Criteri di gruppo per i computer appartenenti al dominio specifico.  
  
    -   È necessario operare con altre soluzioni di gestione dei dati degli utenti, come Reindirizzamento cartelle, o eseguire migrazioni dei dati da queste soluzioni?  
  
    -   È necessario consentire agli utenti di più domini di eseguire la sincronizzazione con un unico server tramite Internet?  
  
    -   È necessario fornire il supporto per gli utenti che non appartengono al gruppo Local Administrators nei loro computer che fanno parte del dominio? (Se sì, è necessario escludere i domini pertinenti dai criteri dei dispositivi di Cartelle di lavoro, come i criteri relativi alla crittografia e alle password)  
  
-   Pianificazione dell'infrastruttura e della capacità  
  
    -   In quali siti devono essere posizionati i server di sincronizzazione nella rete?  
  
    -   Saranno presenti server di sincronizzazione ospitati da un'infrastruttura come un provider di servizi (IaaS), ad esempio Macchine virtuali di Azure?  
  
    -   Saranno necessari server dedicati per gruppi di utenti specifici? Se sì, quanti utenti avrà ogni server dedicato?  
  
    -   Dove si trovano i punti di entrata e uscita nella rete?  
  
    -   I server di sincronizzazione verranno inseriti in un cluster per la tolleranza di errore?  
  
    -   Sarà necessario fare in modo che i server di sincronizzazione gestiscano più volumi di dati per ospitare i dati degli utenti?  
  
-   Sicurezza dei dati  
  
    -   Sarà necessario creare più condivisioni di sincronizzazione in ogni server di sincronizzazione?  
  
    -   Quali gruppi verranno utilizzati per consentire l'accesso alle condivisioni di sincronizzazione?  
  
    -   Se si utilizzano più server di sincronizzazione, quale gruppo di sicurezza si creerà per la capacità delegata di modificare la proprietà msDS-SyncServerURL degli oggetti utente?  
  
    -   Sono presenti esigenze di sicurezza o controllo specifiche per singole condivisioni di sincronizzazione?  
  
    -   È necessario implementare l'autenticazione a più fattori?  
  
    -   È necessario poter cancellare in remoto i dati di Cartelle di lavoro dai computer e dai dispositivi?  
  
-   Accesso dei dispositivi  
  
    -   Quale URL verrà utilizzato per consentire l'accesso per i dispositivi basati su Internet (*l'URL predefinito necessario per l'individuazione automatica del server basata sulla posta elettronica è cartellelavoro.nomedominio*)?  
  
    -   In che modo l'URL verrà pubblicato in Internet?  
  
    -   Verrà utilizzata l'individuazione automatica del server?  
  
    -   Verrà utilizzato Criteri di gruppo per configurare i computer che fanno parte del dominio?  
  
    -   Verrà utilizzato Windows Intune per configurare dispositivi esterni?  
  
    -   Verrà richiesta la registrazione per consentire la connessione dei dispositivi?  
  
## <a name="next-steps"></a>Passaggi successivi  
 Dopo aver progettato l'implementazione di Cartelle di lavoro, è possibile passare alla distribuzione di Cartelle di lavoro. Per ulteriori informazioni, vedere [Distribuzione di Cartelle di lavoro](deploy-work-folders.md).  
  
## <a name="see-also"></a>Vedere anche  
 Per altre informazioni correlate, vedere le risorse seguenti.  
  
|Tipo di contenuto|Riferimenti|  
|------------------|----------------|  
|**Valutazione del prodotto**|-   [Cartelle di lavoro](work-folders-overview.md)<br />[cartelle di lavoro -    per Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (post di Blog)|  
|**Distribuzione**|-   [Progettazione di un'implementazione di cartelle di lavoro](plan-work-folders.md)<br />-   [distribuzione di cartelle di lavoro](deploy-work-folders.md)<br />-   [Distribuzione di cartelle di lavoro con AD FS e proxy applicazione Web (WAP)](deploy-work-folders-adfs-overview.md)<br />- [distribuzione di cartelle di lavoro con Azure ad proxy di applicazione](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />[considerazioni sulle prestazioni @no__t 0 per le distribuzioni di cartelle di lavoro](https://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />[cartelle di lavoro -    per Windows 7 (download di 64 bit)](https://www.microsoft.com/download/details.aspx?id=42558)<br />[cartelle di lavoro -    per Windows 7 (download di 32 bit)](https://www.microsoft.com/download/details.aspx?id=42559)<br />-   [Distribuzione Lab di test di cartelle di lavoro](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (post di Blog)|
