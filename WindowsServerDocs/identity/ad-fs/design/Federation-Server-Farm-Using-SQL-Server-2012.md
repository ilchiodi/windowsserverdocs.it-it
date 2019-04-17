---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Server Farm federativa con SQL Server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Server Farm federativa con SQL Server

>Si applica a: Windows Server 2012

Questa topologia di Active Directory Federation Services \(AD FS\) differisce dalla server farm federativa con topologia di distribuzione \(WID\) Database interno di Windows che non replica i dati per ogni server federativo nella farm. Al contrario, tutti i server federativi della farm possono leggere e scrivere dati in un database comune che viene archiviato in un server che esegue Microsoft SQL Server che si trova nella rete aziendale.  
  
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
Le seguenti versioni SQL server sono supportate con ADFS installato con Windows Server 2012:  
  
-   SQL Server 2008 \ / R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Analogamente alla server farm federativa con topologia database interno di Windows, tutti i server federativi nella farm sono configurati per utilizzare un nome Domain Name System \(DNS\) cluster \ (che rappresenta il servizio federativo di name\) e l'indirizzo IP di un cluster come parte della configurazione del cluster di bilanciamento carico di rete \(NLB\). In questo modo l'host di bilanciamento carico di rete di allocare le richieste client per i server federativi singoli. I proxy server federativi utilizzabile per le richieste client proxy per la server farm federativa.  
  
Nella figura seguente viene illustrato come la società fittizia Contoso Pharmaceuticals distribuite la server farm federativa con topologia SQL Server nella rete aziendale. Viene inoltre illustrato come la società configurata la rete perimetrale con accesso a un server DNS, un host di bilanciamento carico di rete aggiuntiva che utilizza la stessa cluster DNS nome \(fs.contoso.com\) utilizzato nel cluster di bilanciamento carico di rete alla rete aziendale, e con due proxy server federativo \(fsp1 and fsp2\).  
  
![Server farm con SQL](media/FarmSQLProxies.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi o server federativi, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md) o [requisiti di risoluzione dei nomi per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
