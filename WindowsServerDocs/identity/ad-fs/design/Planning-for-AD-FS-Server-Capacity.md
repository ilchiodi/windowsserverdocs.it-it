---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: "Pianificazione della capacità per i Server ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-ad-fs-server-capacity"></a>Pianificazione della capacità per i Server ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
> [!NOTE]  
> Il contenuto disponibile in questo argomento non riflette test reali effettuati su server che eseguono Windows Server 2012. In questo argomento verrà aggiornato dopo l'esecuzione dei test richiesti è stata eseguita.  
  
Capacità di pianificazione di Active Directory Federation Services \(AD FS\) è il processo di pianificazione e delle previsioni periodi di massimo utilizzo per il servizio federativo o scaling\ per la distribuzione di server AD FS a scopo di soddisfare tali requisiti di carico.  
  
Questa sezione vengono fornite istruzioni di distribuzione per il server federativo e i ruoli di proxy server federativo e si basa sul testing in cui è stata eseguita dal team del prodotto AD FS in Microsoft. Lo scopo di questo contenuto è che consentono di:  
  
-   Effettuare una stima accurata all'hardware necessario per ADFS distribuzione specifica dell'organizzazione, ad esempio il numero di server AD FS.  
  
-   Con precisione i periodi di massimo utilizzo per accesso le richieste, piano della crescita di progetto e assicurarsi che la distribuzione di ADFS è in grado di gestire periodi di massimo utilizzo previsti.  
  
Prima di procedere con la lettura di questo contenuto la pianificazione della capacità, è consigliabile completare le attività nell'ordine riportato nelle due tabelle seguenti. Nella prima tabella, vengono forniti collegamenti alle attività consigliate che consentiranno per disporre del contesto rilevante per questa discussione sulla pianificazione di capacità.  
  
|Attività consigliata|Descrizione|Riferimento|  
|--------------------|---------------|-------------|  
|Comprendere i requisiti per la distribuzione di server federativi AD FS e i proxy server federativi|Rivedere importanti requisiti hardware e software necessari per la distribuzione di server federativo e proxy server federativi.|[Appendice a: revisione AD requisiti per ADFS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Selezionare il tipo di database di configurazione di ADFS che sarà distribuito nell'organizzazione|Prima di utilizzare i dati di pianificazione della capacità in questa sezione, è innanzitutto necessario determinare il tipo di database di configurazione di ADFS che sarà distribuito, \(WID\) Database interno di Windows o un database Structured Query Language \(SQL\).|[Il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Considerazioni sulla topologia di distribuzione ADFS](AD-FS-Deployment-Topology-Considerations.md)|  
|Determinare il tipo di layout della topologia da usare con la nuova selezione di database di configurazione AD FS|Dopo avere scelto il tipo di database di configurazione di ADFS per utilizzare nella distribuzione, è necessario prendere in considerazione la topologia di distribuzione più si avvicina alla quale devi server federativi sul posto e i proxy server federativi all'interno dell'ambiente di produzione.|[Determinare la topologia di distribuzione AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Comprendere chiave AD FS correlate pianificazione della capacità condizioni|Verificare le definizioni dei termini utilizzati in tutta la capacità di ADFS discussione sulla pianificazione di pianificazione della capacità comuni.|Vedere la sezione [termini la pianificazione della capacità di ADFS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) in questo argomento|  
  
Dopo avere esaminato il contenuto della tabella precedente, è ora possibile completare le attività prerequisite indicate nella tabella seguente.  
  
|Attività prerequisita|Descrizione|Riferimento|  
|---------------------|---------------|-------------|  
|Scaricare la capacità di ADFS pianificazione foglio di calcolo di ridimensionamento|Foglio di calcolo di AD FS capacità pianificazione ridimensionamento può aiutarti a determinare il numero di server federativi necessari per una distribuzione di AD FS federation server farm. Istruzioni su come utilizzare questo foglio di calcolo sono disponibili tramite il collegamento indicato di seguito per l'attività successiva.|[AD FS pianificazione della capacità di calcolo](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Raccogliere dati sul numero di utenti che richiedono l'accesso accesso single-on \(SSO\) per l'applicazione in grado di riconoscere claims\ di destinazione e i periodi di periodi di massimo utilizzo associati a questo tipo di accesso|Raccogliere dati utente da utilizzare per i valori di input richiesti nel contesto di AD FS capacità pianificazione ridimensionamento foglio di lavoro.|[Stimare il numero di server federativi per l'organizzazione](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|AD FS pianificazione della capacità di calcolo per Windows Server 2016|Foglio di lavoro Pianificazione aggiornato per Windows Server 2016|[Windows ADFS pianificazione della capacità del Server 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Termini relativi pianificazione di capacità di AD FS  
Nella tabella seguente sono descritti termini importanti usati di frequente in questa sezione della Guida alla progettazione di ADFS la pianificazione della capacità. Per un elenco completo dei termini di ADFS, vedere [Understanding Key AD FS concetti](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Termine|Definizione|  
|--------|--------------|  
|Utenti simultanei|Numero stimato di utenti che prevedibilmente invieranno richieste al servizio in un determinato periodo di tempo, in genere un periodo di massima attività.|  
|Utenti attivi|Numero medio approssimativo di utenti attivi in un sistema, ma non inviano necessariamente richieste, durante un periodo di tempo specifico.|  
|Utenti definiti|Un numero massimo teorico di utenti, in genere in base al numero di utenti con account definiti nel sistema.|  
|Richieste al secondo|Il numero di richieste di essere inviati dai client \ (in relazione al carico in una trascrizione) o elaborate dai server \ (in relazione throughput\ server) in un secondo. Questa metrica è usata nella pianificazione della capacità della memoria e processore del server.|  
|Risposta server di destinazione e utilizzo|Metrica delle operazioni riuscite che è associato l'intervallo di prestazioni del server accettabili. In generale, se la velocità di risposta scende di sotto o l'utilizzo supera il numero di destinazione, il sistema è considerato sovraccarico ed è necessaria maggiore capacità.|  
|\(WID\) di Database interno di Windows|Il database di configurazione ADFS predefinito Active Directory che può essere utilizzato come alternativa a SQL Server in alcune distribuzioni di AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Ambiente di configurazione usato durante i test di AD FS  
In questa sezione viene descritto l'ambiente di configurazione che il team del prodotto AD FS utilizzato per eseguire i test. Il team ha usato l'hardware del computer seguenti, software e configurazione di rete per raccogliere dati sulle prestazioni e scalabilità nei test del server federativo:  
  
-   Doppio Quad Core a 2,27 gigahertz \(GHz\) \(8 cores\)  
  
-   16 GB DI RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Rete Gigabit  
  
> [!NOTE]  
> Sebbene 16 GB di RAM è stato utilizzato nel server federativo durante i test, una quantità di memoria inferiore, ad esempio 4 GB di RAM per ogni server federativo è possibile utilizzare per la maggior parte delle distribuzioni di AD FS. I consigli forniti in questo AD FS pianificazione della capacità e i risultati forniti da AD FS capacità pianificazione foglio di calcolo di contenuto si basano sui presupposti che ogni server federativo userà circa 4 GB s di RAM per la maggior parte degli ambienti di produzione di ADFS.  
  
Per raccogliere dati sulle prestazioni e scalabilità per il test del proxy server federativo, il team del prodotto usato la seguente configurazione:  
  
-   Doppio Quad Core a 2,24 GHz \(4 cores\)  
  
-   4 \ GB DI RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Rete Gigabit  
  
> [!NOTE]  
> Consigli sulla capacità per i server ADFS può variare notevolmente, in base alle specifiche scelte per la configurazione hardware e di rete da utilizzare in un ambiente specifico. Come punto di riferimento, le indicazioni relative al dimensionamento fornite in questo contenuto si basa su un obiettivo d'uso dell'80% sui computer citati in precedenza.  
  
## <a name="measure-ad-fs-server-capacity"></a>Capacità del server ADFS di misurazione  
In genere, i componenti hardware che influiscono sulla scalabilità e prestazioni del server sono la CPU, memoria, disco e schede di rete. Fortunatamente, ognuno dei componenti di ADFS richiede minima richiesta di memoria e spazio su disco. Connettività di rete è un requisito ovvio. Di conseguenza, i test di carico eseguiti sui server federativi e proxy server federativi concentreranno su due aree principali per misurare la capacità del server:  
  
-   **Richieste ADFS al secondo di picco:** il numero di richieste di accesso elaborate al secondo su server federativi. Questa misura può aiutarti a determinare quanti utenti simultanei possono accedere a un server specifico. È possibile utilizzare questa misura in combinazione con la relativa all'utilizzo della CPU per comprendere l'effetto di questa misura sulle prestazioni.  
  
-   **Utilizzo della CPU:** la percentuale da quale CPU viene misurata la capacità. Questa misura può aiutarti a determinare il carico complessivo della CPU che si sono verificati in base al numero di richieste di accesso in ingresso al secondo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Altre informazioni sulla pianificazione della capacità di ADFS  
Dopo aver completato le attività prerequisite e avere acquisito familiarità con i termini correlati e i requisiti hardware, è possibile utilizzare le seguenti capacità aggiuntiva pianificazione contenuto per determinare il numero consigliato di server AD FS necessari per la distribuzione:  
  
-   [Pianificazione della capacità del Server federativo](Planning-for-Federation-Server-Capacity.md)  
  
-   [Pianificazione della capacità di Proxy Server federativo](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
