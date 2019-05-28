---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Server farm federativa che usa SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 585d0195b096056ba769f4e9a08d5c4d2156b96a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191455"
---
# <a name="federation-server-farm-using-sql-server"></a>Server farm federativa che usa SQL Server

Questa topologia di Active Directory Federation Services \(ADFS\) differisce dalla server farm federativa utilizzando Database interno di Windows \(WID\) topologia di distribuzione in cui non viene replicato i dati da ogni server federativo nella farm. Al contrario, tutti i server federativi della farm possono leggere e scrivere dati in un database comune che viene archiviato in un server che esegue Microsoft SQL Server che si trova nella rete aziendale.  
  
> [!IMPORTANT]  
> Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
## <a name="deployment-considerations"></a>Considerazioni sulla distribuzione  
Questa sezione vengono descritte varie considerazioni sui destinatari, vantaggi e limitazioni di cui è associate a questa topologia di distribuzione.  
  
### <a name="who-should-use-this-topology"></a>Chi dovrebbe utilizzare questa topologia?  
  
-   Le organizzazioni di grandi dimensioni con più di 100 relazioni di trust che è necessario fornire loro utenti interni e gli utenti esterni con single sign-\-in \(SSO\) l'accesso a servizi o applicazione federata  
  
-   Le organizzazioni che già utilizzano SQL Server e si desidera sfruttare gli strumenti esistenti e competenze  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quali sono i vantaggi dell'utilizzo di questa topologia?  
  
-   Supporto per numeri elevati di relazioni di trust \(più di 100\)  
  
-   Supporto per il rilevamento riproduzione token \(una funzionalità di sicurezza\) e la risoluzione artefatto \(fa parte di Security Assertion Markup Language \(SAML\) protocollo 2.0\)  
  
-   Strumenti di supporto per i vantaggi di SQL Server, ad esempio il mirroring del database, il clustering di failover, reporting e gestione  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quali sono le limitazioni dell'utilizzo di questa topologia?  
  
-   Per impostazione predefinita questa topologia non fornisce ridondanza del database. Anche se una server farm federativa con topologia WID vengono replicati automaticamente il database interno di Windows in ogni server federativo nella farm, la server farm federativa con topologia SQL Server contiene solo una copia del database  
  
> [!NOTE]  
> SQL Server supporta molti dati diversi e opzioni di ridondanza dell'applicazione tra cui clustering di failover del mirroring del database e diversi tipi di replica di SQL Server.  
  
Microsoft Information Technology \(IT\) reparto utilizza il mirroring del database di SQL Server in alta\-safety \(sincrono\) modalità e clustering di failover per fornire elevata\- supporto della disponibilità per l'istanza di SQL Server. SQL Server transazionale \(peer\-al\-peer\) e replica di tipo merge non sono stati testati dal team del prodotto AD FS in Microsoft. Per ulteriori informazioni su SQL Server, vedere [Panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853) o [selezionando il tipo di replica appropriato](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versioni supportate di SQL Server  
Sono supportate le seguenti versioni SQL server con AD FS in Windows Server 2012 R2:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Come per la server farm federativa con topologia database interno di Windows, tutti i server federativi nella farm sono configurati per utilizzare un cluster Domain Name System \(DNS\) nome \(che rappresenta il nome del servizio federativo\) e l'indirizzo IP di un cluster come parte di bilanciamento carico di rete \(Bilanciamento carico di RETE\) configurazione del cluster. In questo modo l'host di bilanciamento carico di RETE di allocare le richieste client per i server federativi singoli. I proxy server federativi utilizzabile per le richieste client proxy per la server farm federativa.  
  
La figura seguente mostra come la società fittizia Contoso Pharmaceuticals distribuite la server farm federativa con topologia SQL Server nella rete aziendale. Viene inoltre illustrato come la società configurata la rete perimetrale con accesso a un server DNS, un host di bilanciamento carico di RETE aggiuntiva che utilizza lo stesso nome DNS del cluster \(fs.contoso.com\) utilizzato nel cluster di bilanciamento carico di RETE alla rete aziendale e con due proxy applicazione web \(wap1 e wap2\).  
  
![server farm con SQL](media/SQLFarmADFSBlue.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con server federativo o proxy applicazione web, vedere "Requisiti di risoluzione dei nomi" sezione [requisiti per ADFS](AD-FS-Requirements.md) e [pianificare l'infrastruttura di Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Opzioni di disponibilità elevata per SQL Server farm  
In Windows Server 2012 R2, ADFS sono disponibili due nuove opzioni per supportare la disponibilità elevata nella farm ADFS utilizzando SQL Server.  
  
-   Supporto per i gruppi di disponibilità AlwaysOn SQL Server  
  
-   Supporto per la disponibilità elevata distribuito geograficamente, replica di tipo merge SQL Server  
  
In questa sezione viene descritta ciascuna di queste opzioni, quali problemi vengono risolti rispettivamente e alcune considerazioni importanti per decidere quali opzioni per la distribuzione.  
  
> [!NOTE]  
> Farm di ADFS di Active Directory che utilizzano Database interno di Windows \(WID\) ridondanza dei dati di base con lettura\/accesso in scrittura sul nodo del server federativo primario e leggere\-accedere solo in nodi secondari.  Può essere utilizzato un geograficamente locale o di una topologia distribuita geograficamente.  
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
  
Gruppi di disponibilità AlwaysOn sono stati introdotti in SQL Server 2012 e forniscono un nuovo modo per creare un'istanza di SQL Server la disponibilità elevata.  Gruppi di disponibilità AlwaysOn combinano gli elementi di clustering e il mirroring del database per la ridondanza e failover sia al livello di istanza SQL a livello di database.  A differenza delle opzioni di disponibilità elevata precedente, i gruppi di disponibilità AlwaysOn non richiedono una risorsa di archiviazione comune \(o rete SAN\) a livello di database.  
  
Un gruppo di disponibilità è costituito da una replica primaria \(un set di lettura\-scrivere database primari\) e da uno a quattro repliche di disponibilità \(set di database secondari corrispondenti\).  Il gruppo di disponibilità supporta una singola lettura\-scrivere copia \(la replica primaria\), lettura e da uno a quattro\-solo le repliche di disponibilità.  Ogni replica di disponibilità deve risiedere in un nodo diverso di un singolo Windows Server Failover Clustering \(WSFC\) cluster.  Per ulteriori informazioni sulla funzionalità AlwaysOn gruppi, vedere [Panoramica di gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Dal punto di vista dei nodi di una farm di Server SQL di ADFS, il gruppo di disponibilità AlwaysOn sostituisce la singola istanza di SQL Server come criteri \/ database elemento.  Il listener del gruppo di disponibilità è il client che \(il servizio token di sicurezza ADFS\) utilizza per connettersi a SQL.  
  
Il diagramma seguente mostra un SQL Server Farm ADFS con gruppo di disponibilità AlwaysOn.  
  
![server farm con SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Gruppi di disponibilità AlwaysOn richiedono che le istanze di SQL Server si trovino in Windows Server Failover Clustering \(WSFC\) nodi.  
  
> [!NOTE]  
> Solo una replica di disponibilità può fungere da una destinazione del failover automatico, altri tre si baserà su failover manuale.  
  
**Considerazioni sulla distribuzione di chiavi**  
  
Se si prevede di utilizzare gruppi di disponibilità AlwaysOn in combinazione con la replica di tipo merge di SQL Server, per prendere nota dei problemi descritti in "Considerazioni sulla distribuzione di chiave per l'utilizzo di ADFS con replica di tipo Merge SQL Server" di seguito.  In particolare, quando un gruppo di disponibilità AlwaysOn contenente un database che è un sottoscrittore di replica esegue il failover, la sottoscrizione di replica ha esito negativo. Per riprendere la replica, un amministratore di replica deve riconfigurare manualmente il sottoscrittore.  Vedere la descrizione di SQL Server del problema specifico [sottoscrittori della replica e gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx) e complessiva supportano le istruzioni per i gruppi di disponibilità AlwaysOn con le opzioni di replica in [replica, il rilevamento delle modifiche, Change Data Capture e gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configurazione di AD FS per usare un gruppo di disponibilità AlwaysOn**  
  
La configurazione di una farm ADFS con gruppi di disponibilità AlwaysOn richiede una modifica quasi irrilevante per la procedura di distribuzione di ADFS:  
  
1.  Prima di configurare i gruppi di disponibilità AlwaysOn, è necessario creare i database che si desidera eseguire il backup.  ADFS crea i relativi database come parte del programma di installazione e configurazione iniziale del primo nodo del servizio federativo di una nuova farm di Server SQL di ADFS.  Come parte della configurazione di ADFS, è necessario specificare una stringa di connessione SQL, sarà necessario configurare il primo nodo della farm ADFS per connettersi direttamente a un'istanza di SQL \(questa condizione è temporanea\).   Per indicazioni specifiche sulla configurazione di una farm di ADFS, inclusa la configurazione di un nodo della farm ADFS con una stringa di connessione SQL server, vedere [configurare un Server federativo](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Dopo avere creati i database di ADFS, assegnarli a gruppi di disponibilità AlwaysOn e creare il listener TCP/IP comune utilizzando gli strumenti di SQL Server ed elaborare in [creazione e configurazione di gruppi di disponibilità \(SQL Server\)](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Infine, utilizzare PowerShell per modificare le proprietà di ADFS per aggiornare la stringa di connessione SQL per utilizzare l'indirizzo DNS del listener del gruppo di disponibilità AlwaysOn.  
  
    Esempi di comandi PSH per aggiornare la stringa di connessione SQL per il database di configurazione di AD FS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Esempi di comandi PSH per aggiornare la stringa di connessione SQL per il database di servizio AD FS risoluzione artefatto:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Replica di tipo Merge SQL Server  
Inoltre introdotto in SQL Server 2012, replica di tipo merge consente la ridondanza dei dati dei criteri ADFS con le seguenti caratteristiche:  
  
-   Lettura e scrittura di funzionalità in tutti i nodi \(non appena il database primario\)  
  
-   Più piccole quantità di dati replicati in modo asincrono per evitare l'introduzione di latenza per il sistema  
  
Il diagramma seguente illustra una farm di SQL Server ADFS geograficamente ridondante con la replica di tipo merge \(1 server di pubblicazione, 2 sottoscrittori\):  
  
![server farm con SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considerazioni sulla distribuzione di chiave per l'uso di AD FS con la replica di tipo Merge di SQL Server \(i numeri nel diagramma precedente\)**  
  
-   Database di distribuzione non è supportato per l'utilizzo con i gruppi di disponibilità AlwaysOn o il mirroring del database.  Vedere SQL Server supportano le istruzioni per i gruppi di disponibilità AlwaysOn con le opzioni di replica in [replica, il rilevamento delle modifiche, Change Data Capture e gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Quando un gruppo di disponibilità AlwaysOn contenente un database che è un sottoscrittore di replica esegue il failover, la sottoscrizione di replica ha esito negativo. Per riprendere la replica, un amministratore di replica deve riconfigurare manualmente il sottoscrittore.  Vedere la descrizione di SQL Server del problema specifico [sottoscrittori della replica e gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh882436.aspx) e complessiva supportano le istruzioni per i gruppi di disponibilità AlwaysOn con le opzioni di replica [replica, il rilevamento delle modifiche, Change Data Capture e gruppi di disponibilità AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
Per istruzioni più dettagliate su come configurare ADFS utilizzare una replica di tipo merge SQL Server, vedere [installazione ridondanza geografica con la replica di SQL Server](https://technet.microsoft.com/library/dn632406.aspx).  
  
## <a name="see-also"></a>Vedere anche  
[Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

