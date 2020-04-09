---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Server farm federativa che usa SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fd82edbcd2403416a08a4d707e5271ab2ced4b22
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853124"
---
# <a name="federation-server-farm-using-sql-server"></a>Server farm federativa che usa SQL Server

Questa topologia per Active Directory Federation Services \(AD FS\) differisce dal server farm della Federazione utilizzando il database interno di Windows \(la topologia di distribuzione di WID\) in quanto non replica i dati in ogni server federativo della farm. Al contrario, tutti i server federativi della farm possono leggere e scrivere dati in un database comune archiviato in un server che esegue Microsoft SQL Server che si trova nella rete aziendale.  
  
> [!IMPORTANT]  
> Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione  
Questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni di grandi dimensioni con più di 100 relazioni di trust che è necessario fornire loro utenti interni e gli utenti esterni con single sign-\-in \(SSO\) l'accesso a servizi o applicazione federata  
  
-   Organizzazioni che usano già SQL Server e vogliono sfruttare i vantaggi offerti dagli strumenti e dalle competenze esistenti  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'utilizzo di questa topologia?  
  
-   Il supporto per un numero maggiore di relazioni di trust \(più di 100\)  
  
-   Supporto per il rilevamento della riproduzione dei token \(una funzionalità di sicurezza\) e la risoluzione degli artefatti \(parte del protocollo Security Assertion Markup Language \(SAML\) 2,0\)  
  
-   Supporto per tutti i vantaggi di SQL Server, ad esempio il mirroring del database, il clustering di failover, la creazione di report e gli strumenti di gestione  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono le limitazioni dell'utilizzo di questa topologia?  
  
-   Per impostazione predefinita questa topologia non fornisce ridondanza del database. Anche se una Federazione server farm con la topologia WID replica automaticamente il database WID in ogni server federativo della farm, la Federazione server farm con SQL Server topologia contiene solo una copia del database  
  
> [!NOTE]  
> SQL Server supporta diverse opzioni di ridondanza dei dati e delle applicazioni, tra cui il clustering di failover, il mirroring del database e diversi tipi diversi di replica SQL Server.  
  
Microsoft Information Technology \(IT\) reparto utilizza il mirroring del database SQL Server in modalità di \(sincrona e con protezione elevata\-\) per offrire un supporto elevato per la disponibilità\-per l'istanza di SQL Server. SQL Server\-peer \(transazionale a\-peer\) e la replica di tipo merge non sono stati testati dal team di prodotto AD FS di Microsoft. Per ulteriori informazioni su SQL Server, vedere [Panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853) o [selezionando il tipo di replica appropriato](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versioni supportate di SQL Server  
Sono supportate le seguenti versioni SQL server con AD FS in Windows Server 2012 R2:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Come per la server farm federativa con topologia database interno di Windows, tutti i server federativi nella farm sono configurati per utilizzare un cluster Domain Name System \(DNS\) nome \(che rappresenta il nome del servizio federativo\) e l'indirizzo IP di un cluster come parte di bilanciamento carico di rete \(Bilanciamento carico di RETE\) configurazione del cluster. In questo modo l'host di bilanciamento carico di RETE di allocare le richieste client per i server federativi singoli. I proxy server federativi utilizzabile per le richieste client proxy per la server farm federativa.  
  
Nella figura seguente viene illustrato il modo in cui la società fittizia Contoso Pharmaceuticals ha distribuito il server farm della Federazione con SQL Server topologia nella rete aziendale. Viene inoltre illustrato come la società configurata la rete perimetrale con accesso a un server DNS, un host di bilanciamento carico di RETE aggiuntiva che utilizza lo stesso nome DNS del cluster \(fs.contoso.com\) utilizzato nel cluster di bilanciamento carico di RETE alla rete aziendale e con due proxy applicazione web \(wap1 e wap2\).  
  
![server farm con SQL](media/SQLFarmADFSBlue.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi o i proxy applicazione Web, vedere la sezione relativa ai requisiti di risoluzione dei nomi in [ad FS requisiti](AD-FS-Requirements.md) e [pianificare l'infrastruttura del proxy dell'applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opzioni di disponibilità elevata per SQL Server farm  
In Windows Server 2012 R2, ADFS sono disponibili due nuove opzioni per supportare la disponibilità elevata nella farm ADFS utilizzando SQL Server.  
  
-   Supporto per i gruppi di disponibilità AlwaysOn SQL Server  
  
-   Supporto per la disponibilità elevata distribuito geograficamente, replica di tipo merge SQL Server  
  
In questa sezione viene descritta ciascuna di queste opzioni, quali problemi vengono risolti rispettivamente e alcune considerazioni importanti per decidere quali opzioni per la distribuzione.  
  
> [!NOTE]  
> Farm di ADFS di Active Directory che utilizzano Database interno di Windows \(WID\) ridondanza dei dati di base con lettura\/accesso in scrittura sul nodo del server federativo primario e leggere\-accedere solo in nodi secondari.  Questa operazione può essere utilizzata in una topologia locale o geograficamente distribuita.  
>   
> Quando si utilizza WID tenere presente le limitazioni seguenti:  
>   
> -   Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.  
> -   Una farm database interno di Windows non supporta la risoluzione di rilevamento o elemento riproduzione token \(fa parte di Security Assertion Markup Language \(SAML\) protocollo\).  
  
Nella tabella seguente fornisce un riepilogo di utilizzo di una farm database interno di Windows.  
  
||||  
|-|-|-|  
||1 \- 100 attendibilità della relying Party|Più di 100 attendibilità della relying Party|  
|1 \- 30 AD FS nodi|Database interno di Windows supportati|Non supportato utilizzando WID \- SQL necessarie|  
|Nodi più di 30 AD FS|Non supportato utilizzando WID \- SQL necessarie|Non supportato utilizzando WID \- SQL necessarie|  
  
### <a name="alwayson-availability-groups"></a>Gruppi di disponibilità AlwaysOn  
**Panoramica**  
  
Gruppi di disponibilità AlwaysOn sono stati introdotti in SQL Server 2012 e forniscono un nuovo modo per creare un'istanza di SQL Server la disponibilità elevata.  I gruppi di disponibilità AlwaysOn combinano elementi di clustering e mirroring del database per la ridondanza e il failover a livello di istanza SQL e a livello di database.  Diversamente dalle opzioni di disponibilità elevata precedenti, i gruppi di disponibilità AlwaysOn non richiedono un \(di archiviazione comune o una rete di archiviazione\) a livello di database.  
  
Un gruppo di disponibilità è costituito da una replica primaria \(un set di lettura\-scrivere database primari\) e da uno a quattro repliche di disponibilità \(set di database secondari corrispondenti\).  Il gruppo di disponibilità supporta una sola copia di lettura\-scrittura \(il\)della replica primaria e da una a quattro leggere\-solo le repliche di disponibilità.  Ogni replica di disponibilità deve risiedere in un nodo diverso di un singolo cluster di failover di Windows Server \(cluster WSFC\).  Per ulteriori informazioni sui gruppi di disponibilità AlwaysOn, vedere [Panoramica di Gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Dal punto di vista dei nodi di una farm di Server SQL di ADFS, il gruppo di disponibilità AlwaysOn sostituisce la singola istanza di SQL Server come criteri \/ database elemento.  Il listener del gruppo di disponibilità è quello che il client \(il servizio token di sicurezza AD FS\) USA per connettersi a SQL.  
  
Il diagramma seguente mostra un SQL Server Farm ADFS con gruppo di disponibilità AlwaysOn.  
  
![server farm con SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Gruppi di disponibilità AlwaysOn richiedono che le istanze di SQL Server si trovino in Windows Server Failover Clustering \(WSFC\) nodi.  
  
> [!NOTE]  
> Solo una replica di disponibilità può fungere da una destinazione del failover automatico, altri tre si baserà su failover manuale.  
  
**Considerazioni principali sulla distribuzione**  
  
Se si prevede di utilizzare i gruppi di disponibilità AlwaysOn in combinazione con SQL Server replica di tipo merge, prendere nota dei problemi descritti in "Considerazioni sulla distribuzione chiave per l'utilizzo di AD FS con SQL Server replica di tipo merge" riportata di seguito.  In particolare, quando un gruppo di disponibilità AlwaysOn contenente un database che rappresenta un Sottoscrittore di replica esegue il failover, la sottoscrizione di replica ha esito negativo. Per riprendere la replica, un relativo amministratore deve riconfigurare manualmente il sottoscrittore.  Vedere la descrizione SQL Server di un problema specifico nei [sottoscrittori della replica e Gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx) e le istruzioni generali di supporto per i gruppi di disponibilità AlwaysOn con le opzioni di replica in [replica, rilevamento modifiche, Change Data Capture e ](https://technet.microsoft.com/library/hh403414.aspx)gruppi di disponibilità AlwaysOn \(SQL Server\).  
  
**Configurazione di AD FS per l'utilizzo di un gruppo di disponibilità AlwaysOn**  
  
La configurazione di una farm ADFS con gruppi di disponibilità AlwaysOn richiede una modifica quasi irrilevante per la procedura di distribuzione di ADFS:  
  
1.  Prima di configurare i gruppi di disponibilità AlwaysOn, è necessario creare i database che si desidera eseguire il backup.  AD FS crea i relativi database come parte dell'installazione e della configurazione iniziale del primo nodo servizio federativo di una nuova farm di SQL Server AD FS.  Come parte della configurazione di AD FS, è necessario specificare una stringa di connessione SQL, pertanto sarà necessario configurare il primo nodo della AD FS farm per connettersi direttamente a un'istanza di SQL \(questa è solo\)temporanea.   Per istruzioni specifiche sulla configurazione di una farm di AD FS, inclusa la configurazione di un nodo della AD FS farm con una stringa di connessione SQL Server, vedere [configurare un server federativo](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Dopo avere creati i database di ADFS, assegnarli a gruppi di disponibilità AlwaysOn e creare il listener TCP/IP comune utilizzando gli strumenti di SQL Server ed elaborare in [creazione e configurazione di gruppi di disponibilità \(SQL Server\)](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Infine, utilizzare PowerShell per modificare le proprietà di AD FS per aggiornare la stringa di connessione SQL per utilizzare l'indirizzo DNS del listener del gruppo di disponibilità AlwaysOn.  
  
    Esempi di comandi PSH per aggiornare la stringa di connessione SQL per il database di configurazione AD FS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring="data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true"  
    PS:\>$temp.put()  
  
    ```  
  
4.  Comandi PSH di esempio per aggiornare la stringa di connessione SQL per il database del servizio di risoluzione dell'artefatto AD FS:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection "Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True"  
    ```  
  
### <a name="sql-server-merge-replication"></a>Replica di tipo Merge SQL Server  
Inoltre introdotto in SQL Server 2012, replica di tipo merge consente la ridondanza dei dati dei criteri ADFS con le seguenti caratteristiche:  
  
-   Funzionalità di lettura e scrittura in tutti i nodi \(non solo il\) primario  
  
-   Più piccole quantità di dati replicati in modo asincrono per evitare l'introduzione di latenza per il sistema  
  
Il diagramma seguente illustra una farm di SQL Server ADFS geograficamente ridondante con la replica di tipo merge \(1 server di pubblicazione, 2 sottoscrittori\):  
  
![server farm con SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considerazioni sulla distribuzione chiave per l'utilizzo di AD FS con SQL Server replica di tipo merge \(i numeri di nota nel diagramma precedente\)**  
  
-   Database di distribuzione non è supportato per l'utilizzo con i gruppi di disponibilità AlwaysOn o il mirroring del database.  Vedere le istruzioni di supporto SQL Server per i gruppi di disponibilità AlwaysOn con le opzioni di replica in [replica, rilevamento modifiche, Change Data Capture e Gruppi di disponibilità AlwaysOn \(SQL Server ](https://technet.microsoft.com/library/hh403414.aspx)\).  
  
-   Quando viene eseguito il failover di un gruppo di disponibilità AlwaysOn contenente un database che opera come sottoscrittore di replica, la sottoscrizione di replica non verrà completata. Per riprendere la replica, un relativo amministratore deve riconfigurare manualmente il sottoscrittore.  Vedere la descrizione SQL Server di un problema specifico nei [sottoscrittori della replica e Gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx) e le istruzioni generali di supporto per i gruppi di disponibilità AlwaysOn con le opzioni di replica [replica, rilevamento modifiche, Change Data Capture e ](https://technet.microsoft.com/library/hh403414.aspx)gruppi di disponibilità AlwaysOn \(SQL Server\).  
  
Per istruzioni più dettagliate su come configurare ADFS utilizzare una replica di tipo merge SQL Server, vedere [installazione ridondanza geografica con la replica di SQL Server](https://technet.microsoft.com/library/dn632406.aspx).  
  
## <a name="see-also"></a>Vedi anche  
[Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

