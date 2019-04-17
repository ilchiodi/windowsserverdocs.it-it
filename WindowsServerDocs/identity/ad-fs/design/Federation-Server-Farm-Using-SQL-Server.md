---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Server Farm federativa con SQL Server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2333f79c733415833b1d54afc8c385700ac5581e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Server Farm federativa con SQL Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Questa topologia di Active Directory Federation Services \(AD FS\) differisce dalla server farm federativa con topologia di distribuzione \(WID\) Database interno di Windows che non replica i dati per ogni server federativo nella farm. Al contrario, tutti i server federativi della farm possono leggere e scrivere dati in un database comune che viene archiviato in un server che esegue Microsoft SQL Server che si trova nella rete aziendale.  
  
> [!IMPORTANT]  
> Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione di  
In questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Organizzazioni di grandi dimensioni con più di 100 relazioni di trust che è necessario fornire i relativi utenti interni e gli utenti esterni con accesso accesso single-on \(SSO\) di servizi o applicazione federata  
  
-   Le organizzazioni che già utilizzano SQL Server e si desiderano sfruttare gli strumenti esistenti e competenze  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'uso di questa topologia?  
  
-   Supporto per un numero maggiore di relazioni di trust \(more than 100\)  
  
-   Supporto per il rilevamento riproduzione token \(a security feature\) e risoluzione artefatto \ (parte di Security Assertion Markup Language \(SAML\) 2.0 protocol\)  
  
-   Strumenti di supporto per i vantaggi di SQL Server, ad esempio il mirroring del database, il clustering di failover, reporting e gestione  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono i limiti di utilizzo di questa topologia?  
  
-   Per impostazione predefinita questa topologia non fornisce ridondanza del database. Anche se una server farm federativa con topologia WID vengono replicati automaticamente il database interno di Windows in ogni server federativo nella farm, la server farm federativa con topologia SQL Server contiene solo una copia del database  
  
> [!NOTE]  
> SQL Server supporta molti dati diversi e opzioni di ridondanza dell'applicazione tra il clustering di failover, il mirroring del database e diversi tipi di replica di SQL Server.  
  
Il reparto Microsoft Information Technology \(IT\) utilizza il mirroring del database di SQL Server in modalità di \(synchronous\) elevata sicurezza e clustering di failover per fornire il supporto di disponibilità elevata per l'istanza di SQL Server. Non sono stati testati dal team del prodotto AD FS in Microsoft SQL Server transazionale \(peer\-to\-peer\) e replica di tipo merge. Per ulteriori informazioni su SQL Server, vedere [panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853) o [selezionando il tipo di replica appropriato](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versioni supportate di SQL Server  
Le seguenti versioni SQL server sono supportate con AD FS in Windows Server 2012 R2:  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Analogamente alla server farm federativa con topologia database interno di Windows, tutti i server federativi nella farm sono configurati per utilizzare un nome Domain Name System \(DNS\) cluster \ (che rappresenta il servizio federativo di name\) e l'indirizzo IP di un cluster come parte della configurazione del cluster di bilanciamento carico di rete \(NLB\). In questo modo l'host di bilanciamento carico di rete di allocare le richieste client per i server federativi singoli. I proxy server federativi utilizzabile per le richieste client proxy per la server farm federativa.  
  
Nella figura seguente viene illustrato come la società fittizia Contoso Pharmaceuticals distribuite la server farm federativa con topologia SQL Server nella rete aziendale. Viene inoltre illustrato come la società configurata la rete perimetrale con accesso a un server DNS, un host di bilanciamento carico di rete aggiuntiva che utilizza la stessa cluster DNS nome \(fs.contoso.com\) utilizzato nel cluster di bilanciamento carico di rete alla rete aziendale, e con due proxy applicazione web \(wap1 and wap2\).  
  
![Server farm con SQL](media/SQLFarmADFSBlue.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con server federativo o proxy applicazione web, vedere "Requisiti di risoluzione dei nomi" sezione [requisiti per ADFS](AD-FS-Requirements.md) e [pianificare l'infrastruttura di Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opzioni di disponibilità elevata per SQL Server farm  
In Windows Server 2012 R2, ADFS sono disponibili due nuove opzioni per supportare la disponibilità elevata nella farm ADFS utilizzando SQL Server.  
  
-   Supporto per i gruppi di disponibilità AlwaysOn SQL Server  
  
-   Supporto per la disponibilità elevata distribuito geograficamente utilizzando la replica di tipo merge SQL Server  
  
Questa sezione viene descritta ciascuna di queste opzioni, quali problemi vengono risolti rispettivamente e alcune considerazioni importanti per decidere quali opzioni per la distribuzione.  
  
> [!NOTE]  
> Farm di ADFS di Active Directory che utilizzano Database interno di Windows \(WID\) fornire la ridondanza dei dati di base con accesso in lettura/scrittura sul nodo del server federativo primario e l'accesso in sola lettura in nodi secondari.  Può essere utilizzato un geograficamente locale o di una topologia distribuita geograficamente.  
>   
> Quando si utilizza WID tenere presente le limitazioni seguenti:  
>   
> -   Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.  
> -   Una farm database interno di Windows non supporta la risoluzione di rilevamento o elemento riproduzione token \ (in parte il \(SAML\) protocol\ Security Assertion Markup Language).  
  
Nella tabella seguente fornisce un riepilogo di utilizzo di una farm database interno di Windows.  
  
||||  
|-|-|-|  
||1 \-100 attendibilità della Relying party|Più di 100 attendibilità della Relying party|  
|1 \-30 AD FS nodi|Database interno di Windows supportati|Non supportato utilizzando WID \-SQL necessarie|  
|Più di 30 AD FS nodi|Non supportato utilizzando WID \-SQL necessarie|Non supportato utilizzando WID \-SQL necessarie|  
  
### <a name="alwayson-availability-groups"></a>Gruppi di disponibilità AlwaysOn  
**Panoramica**  
  
Gruppi di disponibilità AlwaysOn sono stati introdotti in SQL Server 2012 e forniscono un nuovo modo per creare un'istanza di SQL Server la disponibilità elevata.  Gruppi di disponibilità AlwaysOn combinano gli elementi di clustering e del mirroring del database per la ridondanza e failover sia il livello di istanza SQL a livello di database.  A differenza delle opzioni di disponibilità elevata precedente, gruppi di disponibilità AlwaysOn non richiedono una risorsa di archiviazione comune \ (o isolamento di rete di archiviazione area) a livello di database.  
  
Un gruppo di disponibilità è costituito da una replica primaria \ (un set di lettura / scrittura databases\ primario) e da uno a quattro repliche di disponibilità \ (insiemi di databases\ secondario corrispondente).  Il gruppo di disponibilità supporta una copia di lettura / scrittura unico \ (replica\ primario), e da uno a quattro repliche di disponibilità sola.  Ogni replica di disponibilità deve risiedere in un nodo diverso di un singolo cluster di Windows Server Failover Clustering \(WSFC\).  Per ulteriori informazioni sulla funzionalità AlwaysOn gruppi, vedere [Panoramica di gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Dal punto di vista dei nodi di una farm di Server SQL di ADFS, il gruppo di disponibilità AlwaysOn sostituisce la singola istanza di SQL Server come criteri \ / database elemento.  Il listener del gruppo di disponibilità è il client che \ (di AD FS sicurezza token Service \) viene utilizzato per connettersi a SQL.  
  
Il diagramma seguente mostra un SQL Server Farm ADFS con gruppo di disponibilità AlwaysOn.  
  
![Server farm con SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Gruppi di disponibilità AlwaysOn richiedono che le istanze di SQL Server si trovano in Windows Server Failover Clustering \(WSFC\) nodi.  
  
> [!NOTE]  
> Solo una replica di disponibilità può fungere da una destinazione del failover automatico, altri tre si baserà su failover manuale.  
  
**Considerazioni sulla distribuzione di chiave**  
  
Se si prevede di utilizzare gruppi di disponibilità AlwaysOn in combinazione con la replica di tipo merge SQL Server, per prendere nota dei problemi descritti in "Considerazioni sulla distribuzione di chiave per l'utilizzo di ADFS con replica di tipo Merge SQL Server" di seguito.  In particolare, quando un gruppo di disponibilità AlwaysOn contenente un database che è un sottoscrittore di replica esegue il failover, la sottoscrizione di replica ha esito negativo. Per riprendere la replica, un amministratore di replica deve riconfigurare manualmente il sottoscrittore.  Vedere la descrizione di SQL Server del problema specifico [\(SQL Server\) sottoscrittori della replica e gruppi di disponibilità AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) e complessiva supportano le istruzioni per i gruppi di disponibilità AlwaysOn con le opzioni di replica in [\(SQL Server\) replica, il rilevamento delle modifiche, Change Data Capture e gruppi di disponibilità AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configurazione di ADFS per utilizzare un gruppo di disponibilità AlwaysOn**  
  
La configurazione di una farm ADFS con gruppi di disponibilità AlwaysOn richiede una modifica quasi irrilevante per la procedura di distribuzione di ADFS:  
  
1.  I database che si desidera eseguire il backup devono essere creati prima di configurare i gruppi di disponibilità AlwaysOn.  ADFS crea i relativi database come parte del programma di installazione e configurazione iniziale del primo nodo del servizio federativo di una nuova farm di Server SQL di ADFS.  Come parte della configurazione di ADFS, è necessario specificare una stringa di connessione SQL, sarà necessario configurare il primo nodo della farm ADFS per connettersi direttamente a un'istanza di SQL \ (questo è solo temporary\).   Per indicazioni specifiche sulla configurazione di una farm di ADFS, inclusa la configurazione di un nodo della farm ADFS con una stringa di connessione SQL server, vedere [configurare un Server federativo](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Dopo avere creati i database di ADFS, assegnarli a gruppi di disponibilità AlwaysOn e creare il listener TCP/IP comune utilizzando gli strumenti di SQL Server ed elaborare in [\(SQL Server\) creazione e la configurazione di gruppi di disponibilità](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Infine, utilizzare PowerShell per modificare le proprietà di ADFS per aggiornare la stringa di connessione SQL per utilizzare l'indirizzo DNS del listener del gruppo di disponibilità AlwaysOn.  
  
    Esempi di comandi PSH per aggiornare la stringa di connessione SQL per il database dei criteri ADFS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Esempi di comandi PSH per aggiornare la stringa di connessione SQL per il database dei criteri ADFS:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Replica di tipo Merge SQL Server  
Inoltre introdotto in SQL Server 2012, replica di tipo merge consente la ridondanza dei dati dei criteri ADFS con le seguenti caratteristiche:  
  
-   Lettura e scrittura di funzionalità in tutti i nodi \ (non solo primary\)  
  
-   Piccole quantità di dati replicati in modo asincrono per evitare l'introduzione di latenza per il sistema  
  
Il diagramma seguente illustra una farm SQL Server ADFS geograficamente ridondante con la replica di tipo merge \ (1 server di pubblicazione, subscribers\ 2):  
  
![Server farm con SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considerazioni sulla distribuzione di chiave per l'utilizzo di ADFS con replica di tipo Merge di SQL Server \ (i numeri in above\ il diagramma)**  
  
-   Database di distribuzione non è supportato per l'utilizzo con gruppi di disponibilità AlwaysOn o il mirroring del database.  Vedere SQL Server supportano le istruzioni per gruppi di disponibilità AlwaysOn con le opzioni di replica in [\(SQL Server\) replica, il rilevamento delle modifiche, Change Data Capture e gruppi di disponibilità AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Quando un gruppo di disponibilità AlwaysOn contenente un database che è un sottoscrittore di replica esegue il failover, la sottoscrizione di replica ha esito negativo. Per riprendere la replica, un amministratore di replica deve riconfigurare manualmente il sottoscrittore.  Vedere la descrizione di SQL Server del problema specifico [\(SQL Server\) sottoscrittori della replica e gruppi di disponibilità AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) e complessiva supportano le istruzioni per i gruppi di disponibilità AlwaysOn con le opzioni di replica [\(SQL Server\) replica, il rilevamento delle modifiche, Change Data Capture e gruppi di disponibilità AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
Per istruzioni più dettagliate su come configurare ADFS utilizzare una replica di tipo merge di SQL Server, vedere [installazione ridondanza geografica con la replica di SQL Server](https://technet.microsoft.com/en-us/library/dn632406.aspx).  
  
## <a name="see-also"></a>Vedere anche  
[Pianificare la topologia di distribuzione AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di ADFS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

