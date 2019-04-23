---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Server farm federativa che usa SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863952"
---
# <a name="federation-server-farm-using-sql-server"></a>Server farm federativa che usa SQL Server

>Si applica a: Windows Server 2012

Questa topologia di Active Directory Federation Services \(ADFS\) differisce dalla server farm federativa utilizzando Database interno di Windows \(WID\) topologia di distribuzione in cui non viene replicato i dati da ogni server federativo nella farm. Al contrario, tutti i server federativi della farm possono leggere e scrivere dati in un database comune che viene archiviato in un server che esegue Microsoft SQL Server che si trova nella rete aziendale.  
  
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
Le seguenti versioni SQL server sono supportate con ADFS installato con Windows Server 2012:  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Consigli di layout posizionamento e la rete di server  
Come per la server farm federativa con topologia database interno di Windows, tutti i server federativi nella farm sono configurati per utilizzare un cluster Domain Name System \(DNS\) nome \(che rappresenta il nome del servizio federativo\) e l'indirizzo IP di un cluster come parte di bilanciamento carico di rete \(Bilanciamento carico di RETE\) configurazione del cluster. In questo modo l'host di bilanciamento carico di RETE di allocare le richieste client per i server federativi singoli. I proxy server federativi utilizzabile per le richieste client proxy per la server farm federativa.  
  
La figura seguente mostra come la società fittizia Contoso Pharmaceuticals distribuite la server farm federativa con topologia SQL Server nella rete aziendale. Viene inoltre illustrato come la società configurata la rete perimetrale con accesso a un server DNS, un host di bilanciamento carico di RETE aggiuntiva che utilizza lo stesso nome DNS del cluster \(fs.contoso.com\) utilizzato nel cluster di bilanciamento carico di RETE alla rete aziendale e con due server federativi \(fsp1 e fsp2\).  
  
![server farm con SQL](media/FarmSQLProxies.gif)  
  
Per ulteriori informazioni su come configurare l'ambiente di rete per l'utilizzo con i server federativi o server federativi, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md) o [nome requisiti di risoluzione per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
