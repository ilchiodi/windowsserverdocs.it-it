---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Pianificazione della capacità per i server ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 956d026edf5de87a2c20d058f9be5cfec186191b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408005"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Pianificazione della capacità per i server ADFS


  
> [!NOTE]  
> Il contenuto fornito in questo argomento non riflette i test effettivi eseguiti sui server che eseguono Windows Server 2012. L'argomento verrà aggiornato dopo l'esecuzione dei test richiesti.  
  
La pianificazione della capacità \(per\) Active Directory Federation Services ad FS è il processo di previsione dei periodi di utilizzo di picco per il servizio federativo\-e la pianificazione o la scalabilità verticale della distribuzione del server di ad FS per soddisfare tali carichi requisiti.  
  
Questa sezione descrive le linee guida per la distribuzione dei ruoli server federativo e proxy server federativo ed è basata sui test di Lab eseguiti dal team del prodotto AD FS presso Microsoft. La lettura di questo contenuto renderà più facile:  
  
-   Stimare attentamente le esigenze hardware per la distribuzione AD FS specifica dell'organizzazione, ad esempio il numero di server di AD FS.  
  
-   Proiettare accuratamente il picco di utilizzo previsto per\-le richieste di accesso, pianificare la crescita e garantire che la distribuzione di ad FS sia in grado di gestire il picco di utilizzo previsto.  
  
Prima di procedere alla lettura di questo contenuto sulla pianificazione della capacità, è consigliabile completare le attività nell'ordine riportato nelle due tabelle seguenti. Nella prima tabella sono inclusi i collegamenti alle attività consigliate per disporre del contesto rilevante per questa discussione sulla pianificazione.  
  
|Attività consigliata|Descrizione|Riferimenti|  
|--------------------|---------------|-------------|  
|Informazioni sui requisiti per la distribuzione di AD FS server federativi e i proxy server federativi|Esaminare i principali requisiti hardware e software necessari per la distribuzione di server federativi e proxy server federativi.|[Appendice A: Verifica dei requisiti di AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Consente di selezionare il tipo di database di configurazione AD FS da distribuire nell'organizzazione|Prima di iniziare a utilizzare i dati di pianificazione della capacità in questa sezione, è necessario innanzitutto determinare quale tipo di database di configurazione di ad FS verrà distribuito, \(ovvero\) database interno di Windows wid o Structured Query Language \(Database\) SQL.|[Ruolo del database di configurazione ad FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Considerazioni sulla topologia di distribuzione di AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Determinare il tipo di layout della topologia da usare con il nuovo database di configurazione di ADFS selezionato.|Dopo avere scelto il tipo di database di configurazione di ADFS da usare nella distribuzione, si dovrà considerare quale topologia di distribuzione presenta la maggiore analogia con la posizione in cui saranno collocati i server federativi e i proxy server federativi nell'ambiente di produzione.|[Determinare la topologia di distribuzione di AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Informazioni sui termini principali per la pianificazione della capacità (AD FS)|Esaminare le definizioni dei termini comuni relativi alla pianificazione della capacità utilizzati durante la discussione sulla pianificazione della capacità AD FS.|Vedere la sezione[Termini relativi alla pianificazione della capacità di ADFS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) in questo argomento.|  
  
Dopo avere esaminato il contenuto della tabella precedente, si potranno completare le attività prerequisite indicate nella tabella seguente.  
  
|Attività prerequisita|Descrizione|Riferimenti|  
|---------------------|---------------|-------------|  
|Scaricare il foglio di calcolo dimensionamento della pianificazione della capacità AD FS|Il foglio di calcolo per la pianificazione della capacità di AD FS può essere utile per determinare il numero di server federativi necessari per una distribuzione server farm AD FS Federazione. Le istruzioni relative alla modalità d'uso di questo foglio di calcolo sono disponibili tramite il collegamento indicato di seguito per l'attività successiva.|[Foglio di calcolo per la pianificazione della capacità AD FS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Raccogliere dati sul numero di utenti che richiederanno l'accesso Single\-Sign \(-\) on SSO all'applicazione in\-grado di riconoscere attestazioni di destinazione e i periodi di utilizzo di picco previsti associati a questo accesso|I dati utente raccolti saranno usati per i valori di input richiesti nel contesto del foglio di calcolo per il dimensionamento relativo alla pianificazione della capacità di ADFS.|[Stimare il numero di server federativi per l'organizzazione](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Foglio di calcolo per la pianificazione della capacità di AD FS per Windows Server 2016|Foglio di foglio di pianificazione aggiornato per Windows Server 2016|[AD FS la pianificazione della capacità di Windows Server 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Termini della pianificazione della capacità di AD FS  
La tabella seguente descrive i termini importanti usati spesso in questa sezione relativa alla pianificazione della capacità della Guida alla progettazione di AD FS. Per un elenco più completo dei termini di AD FS, vedere [Understanding Key ad FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Nome|Definizione|  
|--------|--------------|  
|Utenti simultanei|Numero stimato di utenti che prevedibilmente invieranno richieste al servizio nell'arco di un periodo di tempo specifico, solitamente un periodo di massima attività.|  
|Utenti attivi|Numero medio approssimativo di utenti attivi in un sistema nell'arco di un periodo di tempo specifico, ma che non inviano necessariamente richieste.|  
|Utenti definiti|Numero massimo teorico di utenti, basato in genere sul numero di utenti con account definiti nel sistema.|  
|Richieste al secondo|Numero di richieste \(inviate dai client quando si parla del carico su un sistema\) o elaborate dai server \(quando si parla della velocità effettiva\) del server in un secondo. Questa metrica è usata nella pianificazione della capacità della memoria e del processore del server.|  
|Velocità di risposta e uso del server di destinazione|Metrica delle operazioni riuscite che delimita l'intervallo di prestazioni del server accettabili. In genere, se nel server di destinazione la velocità di risposta è inferiore o l'uso è superiore, il sistema è considerato sovraccarico ed è necessaria maggiore capacità.|  
|Database \(interno di Windows wid\)|Il database di configurazione AD FS predefinito che può essere usato come alternativa a SQL Server in alcune distribuzioni di AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Ambiente di configurazione usato durante i test di ADFS  
In questa sezione viene descritto l'ambiente di configurazione utilizzato dal team del prodotto AD FS per eseguire i test. Per raccogliere dati relativi a prestazioni e scalabilità nei test del server federativo, il team ha usato la seguente configurazione di hardware, software e rete:  
  
-   Dual Quad Core 2,27 gigahertz \(GHz\) \(8 core\)  
  
-   16\-GB DI RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Rete Gigabit  
  
> [!NOTE]  
> Sebbene i 16 GB di RAM siano stati usati nel server federativo durante i test, è possibile usare una quantità di memoria più moderata, ad esempio 4 GB di RAM per server federativo, per la maggior parte delle distribuzioni AD FS. I consigli forniti in questo AD FS contenuto della pianificazione della capacità insieme ai risultati forniti dal foglio di calcolo della pianificazione della capacità AD FS sono basati su presupposti che ciascun server federativo utilizzerà approssimativamente 4GB's di RAM per la maggior parte della produzione AD FS ambienti.  
  
Per raccogliere dati relativi a prestazioni e scalabilità nei test del proxy server federativo, il team del prodotto ha usato la seguente configurazione:  
  
-   Dual Core Quad Core 2,24 \(GHz 4 core\)  
  
-   4\-GB DI RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Rete Gigabit  
  
> [!NOTE]  
> Le raccomandazioni relative alla capacità per i server AD FS possono variare notevolmente, a seconda delle specifiche scelte per l'hardware e la configurazione di rete da usare in un determinato ambiente. Come punto di riferimento, le indicazioni relative al dimensionamento fornite in questo contenuto si basano su un obiettivo d'uso dell'80% sui computer citati in precedenza.  
  
## <a name="measure-ad-fs-server-capacity"></a>Misurare la capacità del server ADFS  
In genere, i componenti hardware che incidono sulle prestazioni e la scalabilità del server sono la CPU, la memoria, il disco e le schede di rete. Fortunatamente, ogni componente AD FS richiede una quantità minima di memoria e spazio su disco. La connettività di rete è un requisito ovvio. Di conseguenza, per misurare la capacità dei server, i test di carico eseguiti sui server federativi e sui proxy server federativi sono concentrati su due aree principali:  
  
-   **Picco AD FS richieste al secondo:** Numero di richieste di\-accesso elaborate al secondo nei server federativi. Questa misura può essere utile per determinare quanti utenti simultanei possono accedere a un server specifico. Si può usare questa misura insieme a quella relativa all'utilizzo della CPU per valutare l'effetto di questa misura sulle prestazioni.  
  
-   **Consumo CPU:** percentuale in base alla quale viene misurata la capacità della CPU. Questa misura può essere utile per determinare il carico complessivo della CPU che si è verificato in base al numero\-di richieste di accesso in ingresso al secondo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Altre informazioni sulla pianificazione della capacità di ADFS  
Dopo aver completato le attività dei prerequisiti e avere acquisito familiarità con i termini correlati e i requisiti hardware, è possibile usare i seguenti contenuti aggiuntivi per la pianificazione della capacità per determinare il numero consigliato di AD FS server necessari per il distribuzione  
  
-   [Pianificazione della capacità per i server federativi](Planning-for-Federation-Server-Capacity.md)  
  
-   [Pianificazione della capacità per i proxy server federativi](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
