---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Server farm federativa che usa SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 862cbc74833e2d4e9f385ba961b58a1f703e6611
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853134"
---
# <a name="federation-server-farm-using-sql-server"></a>Server farm federativa che usa SQL Server

Questa topologia per Active Directory Federation Services \(AD FS\) differisce dal server farm della Federazione utilizzando il database interno di Windows \(la topologia di distribuzione di WID\) in quanto non replica i dati in ogni server federativo della farm. Al contrario, tutti i server federativi della farm possono leggere e scrivere dati in un database comune archiviato in un server che esegue Microsoft SQL Server che si trova nella rete aziendale.  
  
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
Le seguenti versioni SQL server sono supportate con ADFS installato con Windows Server 2012:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Come per la server farm federativa con topologia database interno di Windows, tutti i server federativi nella farm sono configurati per utilizzare un cluster Domain Name System \(DNS\) nome \(che rappresenta il nome del servizio federativo\) e l'indirizzo IP di un cluster come parte di bilanciamento carico di rete \(Bilanciamento carico di RETE\) configurazione del cluster. In questo modo l'host di bilanciamento carico di RETE di allocare le richieste client per i server federativi singoli. I proxy server federativi utilizzabile per le richieste client proxy per la server farm federativa.  
  
Nella figura seguente viene illustrato il modo in cui la società fittizia Contoso Pharmaceuticals ha distribuito il server farm della Federazione con SQL Server topologia nella rete aziendale. Viene inoltre illustrato come la società configurata la rete perimetrale con accesso a un server DNS, un host di bilanciamento carico di RETE aggiuntiva che utilizza lo stesso nome DNS del cluster \(fs.contoso.com\) utilizzato nel cluster di bilanciamento carico di RETE alla rete aziendale e con due server federativi \(fsp1 e fsp2\).  
  
![server farm con SQL](media/FarmSQLProxies.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi o server federativi, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md) o [nome requisiti di risoluzione per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
