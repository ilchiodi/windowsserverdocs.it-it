---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Considerazioni sulla topologia di distribuzione ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eee14ee7bb50e1a82f35caf9fbacda0b86d3a1ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considerazioni sulla topologia di distribuzione ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive le considerazioni importanti che consentono di pianificare e progettare la topologia di distribuzione Active Directory Federation Services \(AD FS\) da utilizzare nell'ambiente di produzione. In questo argomento è un punto di partenza per rivedere e valutare le considerazioni che riguardano le funzionalità o le funzionalità saranno disponibili dopo la distribuzione di ADFS. Ad esempio, a seconda di quali database scelto per archiviare il database di configurazione di ADFS in ultima analisi determina se è possibile implementare alcune funzionalità di Security Assertion Markup Language \(SAML\) che richiedono SQL Server.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinazione del tipo di database di configurazione di ADFS per utilizzare  
ADFS utilizza un database per archiviare la configurazione e, in alcuni casi, i dati transazionali relativi al servizio federativo. È possibile utilizzare il software ADFS per selezionare il predefinita \(WID\) Database interno di Windows o Microsoft SQL Server 2005 o versione successiva per archiviare i dati nel servizio federativo.  
  
Per la maggior parte dei casi, i due tipi di database sono relativamente equivalenti. Tuttavia, esistono alcune differenze da tenere presenti prima di iniziare leggere altre informazioni sulle diverse topologie di distribuzione che è possibile utilizzare con AD FS. La tabella seguente descrive le funzionalità supportate le differenze tra un database interno di Windows e un database SQL Server.  
  
Funzionalità di AD FS  
  
|Funzionalità|Database interno di Windows è supportata?|Supportata in SQL Server?|Ulteriori informazioni su questa funzionalità|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Distribuzione di farm di server federativo|Sì, con un limite di cinque server federativi per ogni farm.|Sì. Non esiste alcun limite imposto per il numero di server federativi che è possibile distribuire in una singola farm.|[Determinare la topologia di distribuzione AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Risoluzione artefatto SAML **Nota:** questa funzionalità non è richiesta per scenari Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sì|[Il ruolo del Database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedure consigliate per la pianificazione sicuro e distribuzione di ADFS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|Rilevamento riproduzione token SAML\/WS-Federation|No|Sì|[Il ruolo del Database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedure consigliate per la pianificazione sicuro e distribuzione di ADFS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
Funzionalità di database  
  
|Funzionalità|Database interno di Windows è supportata?|Supportata in SQL Server?|Ulteriori informazioni su questa funzionalità|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Ridondanza del database di base con la replica pull in cui uno o più server che ospita una sola copia del database richiedono le modifiche vengono apportate in un server di origine che ospita una copia di lettura/scrittura del database|Sì|No|[Il ruolo del Database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Ridondanza del database con soluzioni a disponibilità elevata, ad esempio clustering di failover o mirroring \ (at only\ il livello di database) **Nota:** tutte le topologie di distribuzione di ADFS supportano il clustering a livello di servizio ADFS.|No|Sì|[Il ruolo del Database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>Considerazioni su SQL Server  
Se si seleziona SQL Server come database di configurazione per la distribuzione di ADFS, considerare i seguenti aspetti.  
  
-   **SAML funzionalità e i relativi effetti sulle dimensioni del database e la crescita**. Quando sono abilitate la risoluzione artefatto SAML o funzionalità di rilevamento riproduzione token SAML, ADFS archivia le informazioni nel database di configurazione SQL Server per ogni token di ADFS rilasciato. La crescita del database di SQL Server in seguito a questa attività non è considerata significativa e dipende dal periodo di memorizzazione riproduzione token configurato. Ogni record artefatto sono una dimensione di circa 30 kilobyte \(KB\).  
  
-   **Numero di server necessari per la distribuzione**. È necessario aggiungere almeno un server aggiuntivo \ (per il numero totale di server necessari per distribuire il infrastructure\ AD FS) che fungerà da un host dedicato dell'istanza di SQL Server. Se si prevede di usare clustering di failover o mirroring per fornire la tolleranza di errore e la scalabilità per il database di configurazione di SQL Server, è necessario un minimo di due server SQL.  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Come il tipo di database di configurazione che si seleziona potrebbe influire sulle risorse hardware  
L'impatto sulle risorse hardware in un server federativo che viene distribuito in una farm con database interno di Windows anziché un server federativo che viene distribuito in una farm con database di SQL Server non è significativo. Tuttavia, è importante considerare che quando si usa database interno di Windows per la farm, ogni server federativo della farm deve archiviare, gestire e mantenere le modifiche della replica per la copia locale del database di configurazione di ADFS continuando a eseguire le normali operazioni che richiede il servizio federativo.  
  
In confronto, i server federativi che vengono distribuiti in una farm che utilizza il database di SQL Server non contengono necessariamente un'istanza locale del database di configurazione di ADFS. Pertanto, potrebbero rendono esigenze leggermente inferiori risorse hardware.  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Verifica che l'ambiente di produzione può supportare una distribuzione di ADFS  
Oltre ai server federativi che sarà distribuito e a seconda di come impostare l'ambiente di produzione esistente, i seguenti server aggiuntivi potrebbe essere necessario fornire l'infrastruttura necessaria per supportare la nuova distribuzione di ADFS:  
  
-   Controller di dominio Active Directory  
  
-   Autorità di certificazione \(CA\)  
  
-   Server Web per ospitare i metadati federativi  
  
-   \(NLB\) del bilanciamento del carico di rete  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
