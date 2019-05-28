---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Pianificazione della capacità per i server ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0191c822ec068c5486a1b0d5da4c1ae2ee9e4d31
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191104"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Pianificazione della capacità per i server ADFS


  
> [!NOTE]  
> Il contenuto fornito in questo argomento non riflette test reali effettuati su server che esegue Windows Server 2012. L'argomento verrà aggiornato dopo l'esecuzione dei test richiesti.  
  
Pianificazione della capacità per Active Directory Federation Services \(ADFS\) è il processo di previsione periodi di utilizzo per il servizio federativo e la pianificazione o ridimensionamento\-backup della distribuzione di server AD FS a scopo di soddisfare tali caricare requisiti.  
  
Questa sezione vengono fornite indicazioni di distribuzione per il ruolo proxy server federativo sia nel server federativo e si basa sui test di laboratorio che è stata eseguita dal team del prodotto AD FS in Microsoft. La lettura di questo contenuto renderà più facile:  
  
-   Effettuare una stima accurata all'hardware necessario per specifica distribuzione di AD FS dell'organizzazione, ad esempio il numero di server AD FS.  
  
-   L'utilizzo di picco previsto per l'accesso del progetto in modo accurato\-nelle richieste, pianificare la crescita e assicurarsi che la distribuzione di AD FS è in grado di gestire che prevede l'utilizzo di picco.  
  
Prima di procedere alla lettura di questo contenuto sulla pianificazione della capacità, è consigliabile completare le attività nell'ordine riportato nelle due tabelle seguenti. Nella prima tabella sono inclusi i collegamenti alle attività consigliate per disporre del contesto rilevante per questa discussione sulla pianificazione.  
  
|Attività consigliata|Descrizione|Riferimenti|  
|--------------------|---------------|-------------|  
|Comprendere i requisiti per la distribuzione di server federativi AD FS e proxy server federativi|Esaminare i principali requisiti hardware e software necessari per la distribuzione di server federativi e proxy server federativi.|[Appendice A: Verifica dei requisiti di AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Selezionare il tipo di database di configurazione di ADFS da distribuire nell'organizzazione|Prima di iniziare a usare i dati di pianificazione della capacità in questa sezione, è innanzitutto necessario determinare quale configurazione di AD FS del database che sarà distribuito, digitare uno dei Database interno di Windows \(WID\) o un Structured Query Language \(SQL\) database.|[Il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Considerazioni sulla topologia di distribuzione di AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Determinare il tipo di layout della topologia da usare con il nuovo database di configurazione di ADFS selezionato.|Dopo avere scelto il tipo di database di configurazione di ADFS da usare nella distribuzione, si dovrà considerare quale topologia di distribuzione presenta la maggiore analogia con la posizione in cui saranno collocati i server federativi e i proxy server federativi nell'ambiente di produzione.|[Determinare la topologia di distribuzione di AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Comprendere chiave AD FS correlati di pianificazione della capacità termini|Esaminare le definizioni dei termini usati nella discussione sulla pianificazione di capacità di ADFS di pianificazione della capacità comuni.|Vedere la sezione[Termini relativi alla pianificazione della capacità di ADFS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) in questo argomento.|  
  
Dopo avere esaminato il contenuto della tabella precedente, si potranno completare le attività prerequisite indicate nella tabella seguente.  
  
|Attività prerequisita|Descrizione|Riferimenti|  
|---------------------|---------------|-------------|  
|Scaricare la capacità di ADFS pianificazione foglio di calcolo di ridimensionamento|Il foglio di calcolo di dimensionamento di pianificazione della capacità di AD FS può aiutare a determinare il numero di server federativi necessari per una distribuzione di AD FS federation server farm. Le istruzioni relative alla modalità d'uso di questo foglio di calcolo sono disponibili tramite il collegamento indicato di seguito per l'attività successiva.|[Foglio di pianificazione della capacità di calcolo AD FS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Raccogliere i dati relativi al numero di utenti che richiedono l'accesso single sign\-sul \(SSO\) accesso alle attestazioni destinazione\-compatibile con applicazioni e i periodi di utilizzo di picco previsto associati con questo tipo di accesso|I dati utente raccolti saranno usati per i valori di input richiesti nel contesto del foglio di calcolo per il dimensionamento relativo alla pianificazione della capacità di ADFS.|[Stimare il numero di server federativi per l'organizzazione](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|AD FS pianificazione della capacità di calcolo per Windows Server 2016|Foglio di pianificazione aggiornato per Windows Server 2016|[AD FS per Windows Server 2016 Capacity Planning](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Termini relativi pianificazione di capacità di AD FS  
Nella tabella seguente vengono descritti termini importanti usati spesso in questa sezione della Guida alla progettazione di AD FS di pianificazione della capacità. Per un elenco più completo dei termini di AD FS, vedere [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Nome|Definizione|  
|--------|--------------|  
|Utenti simultanei|Numero stimato di utenti che prevedibilmente invieranno richieste al servizio nell'arco di un periodo di tempo specifico, solitamente un periodo di massima attività.|  
|Utenti attivi|Numero medio approssimativo di utenti attivi in un sistema nell'arco di un periodo di tempo specifico, ma che non inviano necessariamente richieste.|  
|Utenti definiti|Numero massimo teorico di utenti, basato in genere sul numero di utenti con account definiti nel sistema.|  
|Richieste al secondo|Il numero di richieste sia inviate dai client \(quando si parla del carico in un sistema\) o elaborate dai server \(quando si parla di velocità effettiva server\) in un secondo. Questa metrica è usata nella pianificazione della capacità della memoria e del processore del server.|  
|Velocità di risposta e uso del server di destinazione|Metrica delle operazioni riuscite che delimita l'intervallo di prestazioni del server accettabili. In genere, se nel server di destinazione la velocità di risposta è inferiore o l'uso è superiore, il sistema è considerato sovraccarico ed è necessaria maggiore capacità.|  
|Database interno di Windows \(WID\)|Il database di configurazione ADFS AD predefinita che può essere usato come alternativa a SQL Server in alcune distribuzioni di AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Ambiente di configurazione usato durante i test di ADFS  
Questa sezione descrive l'ambiente di configurazione che il team del prodotto AD FS usato per eseguire i test. Per raccogliere dati relativi a prestazioni e scalabilità nei test del server federativo, il team ha usato la seguente configurazione di hardware, software e rete:  
  
-   Doppio Quad Core a 2,27 gigahertz \(GHz\) \(8 core\)  
  
-   16\-GB RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Rete Gigabit  
  
> [!NOTE]  
> Anche se è stato usato 16 GB di RAM nel server federativo durante i test, una dimensione di memoria più moderata, ad esempio 4 GB di RAM per ogni server federativo è utilizzabile per la maggior parte delle distribuzioni di AD FS. Le indicazioni fornite in questo AD FS pianificazione della capacità di contenuto e i risultati restituiti da AD FS capacità pianificazione foglio di calcolo di si basano su presupposti che ciascun server federativo userà circa 4 GB's di RAM per la maggior parte dei produzione di AD FS ambienti.  
  
Per raccogliere dati relativi a prestazioni e scalabilità nei test del proxy server federativo, il team del prodotto ha usato la seguente configurazione:  
  
-   Doppio Quad Core a 2,24 GHz \(4 core\)  
  
-   4\-GB RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Rete Gigabit  
  
> [!NOTE]  
> Consigli sulla capacità per i server AD FS possono variare notevolmente, in base alle specifiche scelte per la configurazione hardware e di rete da usare in un determinato ambiente. Come punto di riferimento, le indicazioni relative al dimensionamento fornite in questo contenuto si basano su un obiettivo d'uso dell'80% sui computer citati in precedenza.  
  
## <a name="measure-ad-fs-server-capacity"></a>Misurare la capacità del server ADFS  
In genere, i componenti hardware che incidono sulle prestazioni e la scalabilità del server sono la CPU, la memoria, il disco e le schede di rete. Fortunatamente, ognuno dei componenti di AD FS esigenze molto limitate in memoria e spazio su disco. La connettività di rete è un requisito ovvio. Di conseguenza, per misurare la capacità dei server, i test di carico eseguiti sui server federativi e sui proxy server federativi sono concentrati su due aree principali:  
  
-   **Richieste ADFS raggiungono il picco al secondo:** Il numero di accesso\-nelle richieste che elaborate al secondo su server federativi. Questa misura può essere utile per determinare quanti utenti simultanei possono accedere a un server specifico. Si può usare questa misura insieme a quella relativa all'utilizzo della CPU per valutare l'effetto di questa misura sulle prestazioni.  
  
-   **Utilizzo della CPU:** percentuale in base alla quale viene misurata la capacità della CPU. Questa misura consente di determinare il carico complessivo della CPU che si sono verificati in base al numero di accesso in ingresso\-nelle richieste al secondo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Altre informazioni sulla pianificazione della capacità di ADFS  
Dopo aver completato le attività prerequisite e avere familiarità con termini correlati e i requisiti hardware, è possibile utilizzare la capacità aggiuntiva seguente contenuto di pianificazione per aiutare a determinare il numero consigliato di server AD FS necessari per il distribuzione:  
  
-   [Pianificazione della capacità per i server federativi](Planning-for-Federation-Server-Capacity.md)  
  
-   [Pianificazione della capacità per i proxy server federativi](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
