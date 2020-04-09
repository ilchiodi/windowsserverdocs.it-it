---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Considerazioni sulla topologia di distribuzione di ADFS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 42db104691bce75c37adf19eb97abba3bf579cf8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855874"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considerazioni sulla topologia di distribuzione di ADFS

In questo argomento vengono descritte importanti considerazioni che consentono di pianificare e progettare quali Active Directory Federation Services \(AD FS topologia di distribuzione\) da utilizzare nell'ambiente di produzione. Questo argomento è un punto di partenza per rivedere e valutare le considerazioni che influiscono sulle funzionalità o funzionalità che saranno disponibili dopo la distribuzione di AD FS. Ad esempio, a seconda del tipo di database scelto per archiviare il database di configurazione di AD FS, viene infine determinato se è possibile implementare determinate funzionalità di Security Assertion Markup Language \(\) SAML che richiedono SQL Server.  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinazione del tipo di database di configurazione AD FS da usare  
AD FS utilizza un database per archiviare la configurazione e, in alcuni casi, i dati transazionali correlati al Servizio federativo. È possibile utilizzare il software AD FS per selezionare i\-compilati nel database interno di Windows \(WID\) o Microsoft SQL Server 2005 o versione successiva per archiviare i dati nella Servizio federativo.  

I due tipi di database sono relativamente equivalenti per la maggior parte degli scopi. Tuttavia, esistono alcune differenze da tenere presenti prima di iniziare a leggere altre informazioni sulle diverse topologie di distribuzione che è possibile usare con AD FS. Nella tabella seguente vengono descritte le differenze tra le funzionalità supportate tra un database WID e un database di SQL Server.  

Funzionalità di AD FS  

|Caratteristica|Supportata in Database interno di Windows|Supportata in SQL Server|Altre informazioni su questa funzionalità|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Distribuzione in una server farm federativa|Sì, con un limite di 30 server federativi per ogni farm|Sì. Non esiste un limite imposto per il numero di server federativi che si possono distribuire in una singola farm.|[Determinare la topologia di distribuzione di AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Nota sulla risoluzione degli artefatti SAML **:** questa funzionalità non è necessaria per i Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office scenari di SharePoint.|No|Sì|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[Procedure consigliate per la pianificazione e la distribuzione sicure di AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-rilevamento riproduzione token federazione|No|Sì|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[Procedure consigliate per la pianificazione e la distribuzione sicure di AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

Funzionalità di database  

|Caratteristica|Supportata in Database interno di Windows|Supportata in SQL Server|Altre informazioni su questa funzionalità|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Ridondanza di base del database tramite la replica pull, in cui uno o più server che ospitano un\-di lettura solo una copia del database richiedono modifiche apportate in un server di origine che ospita una copia di lettura\/scrittura del database|Sì|No|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Ridondanza dei database con soluzioni di disponibilità elevata\-, ad esempio il clustering di failover o il mirroring \(solo a livello di database\) **Nota:** tutte le topologie di distribuzione ad FS supportano il clustering a livello di servizio ad FS.|No|Sì|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[Panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>Considerazioni su SQL Server  
Se si seleziona SQL Server come database di configurazione per la distribuzione di AD FS, è necessario considerare i fatti di distribuzione seguenti.  

-   **Funzionalità SAML e relativo effetto sulle dimensioni e la crescita del database**. Quando le funzionalità di risoluzione artefatto SAML o di rilevamento riproduzione token SAML sono abilitate, AD FS archivia le informazioni nel database di configurazione SQL Server per ogni token AD FS emesso. La crescita del database SQL Server come risultato di questa attività non è considerata significativa e dipende dal periodo di memorizzazione della riproduzione dei token configurato. Ogni record di artefatto ha una dimensione pari a circa 30 kilobyte \(KB\).  

-   **Numero di server richiesti per la distribuzione**. Sarà necessario aggiungere almeno un server aggiuntivo \(al numero totale di server necessari per distribuire l'infrastruttura AD FS\) che fungerà da host dedicato dell'istanza di SQL Server. Se si intende utilizzare il clustering di failover o il mirroring per fornire la tolleranza di errore e la scalabilità per il database di configurazione di SQL Server, è necessario un minimo di due server SQL.  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Possibile impatto del tipo di database di configurazione scelto sulle risorse hardware  
L'impatto sulle risorse hardware in un server federativo distribuito in una farm utilizzando WID anziché un server federativo distribuito in una farm che utilizza il database di SQL Server non è significativo. Tuttavia, è importante tenere presente che, quando si utilizza WID per la farm, ogni server federativo della farm deve archiviare, gestire e mantenere le modifiche della replica per la copia locale del database di configurazione AD FS continuando anche a fornire le normali operazioni richieste dall'Servizio federativo.  

In confronto, i server federativi distribuiti in una farm che utilizza il database di SQL Server non contengono necessariamente un'istanza locale del database di configurazione AD FS. Potrebbero quindi avere esigenze leggermente inferiori in termini di risorse hardware.  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Verifica della capacità dell'ambiente di produzione di supportare una distribuzione di ADFS  
Oltre ai server federativi che verranno distribuiti e a seconda della configurazione dell'ambiente di produzione esistente, potrebbe essere necessario disporre dei seguenti server aggiuntivi per fornire l'infrastruttura necessaria per supportare la nuova distribuzione di AD FS:  

-   Controller di dominio Active Directory  

-   Autorità di certificazione \(CA\)  

-   Server Web per ospitare i metadati federativi  

-   Bilanciamento carico di rete \(NLB\)  

## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
