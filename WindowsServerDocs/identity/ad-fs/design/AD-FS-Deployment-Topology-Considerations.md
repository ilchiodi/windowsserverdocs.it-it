---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Considerazioni sulla topologia di distribuzione di ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cf646dedef85add8607c7940275e3c3fae90a661
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445345"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considerazioni sulla topologia di distribuzione di ADFS

In questo argomento descrive importanti considerazioni che consentono di pianificare e progettare quali Active Directory Federation Services \(ADFS\) topologia di distribuzione da usare nell'ambiente di produzione. In questo argomento è un punto di partenza per rivedere e valutare le considerazioni che riguardano le caratteristiche o funzionalità saranno disponibili all'utente dopo la distribuzione di AD FS. Ad esempio, a seconda di quale database tipo scelto per archiviare il database di configurazione di ADFS determinerà in ultima analisi se è possibile implementare determinati Security Assertion Markup Language \(SAML\) funzionalità che richiedono SQL Server.  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinazione del tipo di database di configurazione di ADFS da usare  
ADFS utilizza un database per archiviare la configurazione e, in alcuni casi, i dati transazionali relativi al servizio federativo. È possibile usare il software ADFS per selezionare il compilato\-nel Database interno di Windows \(WID\) o Microsoft SQL Server 2005 o versione successiva per archiviare i dati nel servizio federativo.  

I due tipi di database sono relativamente equivalenti per la maggior parte degli scopi. Tuttavia, esistono alcune differenze da tenere presenti prima di iniziare la lettura di più sulle tecnologie di distribuzione diversi che è possibile utilizzare con ADFS. Nella tabella seguente vengono descritte le differenze nelle funzionalità supportate tra un database interno di Windows e un database di SQL Server.  

Funzionalità di AD FS  

|Funzionalità|Supportata in Database interno di Windows|Supportata in SQL Server|Altre informazioni su questa funzionalità|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Distribuzione in una server farm federativa|Sì, con un limite di 30 server federativi per ogni farm|Sì. Non esiste un limite imposto per il numero di server federativi che si possono distribuire in una singola farm.|[Determinare la topologia di distribuzione di AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Risoluzione artefatto SAML **Nota:** Questa funzionalità non è richiesta per scenari Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Yes|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedure consigliate per la pianificazione e la distribuzione sicure di AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-rilevamento riproduzione token federazione|No|Yes|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Procedure consigliate per la pianificazione e la distribuzione sicure di AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

Funzionalità di database  

|Funzionalità|Supportata in Database interno di Windows|Supportata in SQL Server|Altre informazioni su questa funzionalità|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Tramite la ridondanza dei database Basic effettuare il pull della replica, in cui uno o più server che ospita un'operazione di lettura\-copia solo delle modifiche di richiesta del database che vengono eseguite su un server di origine che ospita una lettura\/scrivere copia del database|Yes|No|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Ridondanza del database con elevata\-soluzioni di disponibilità, ad esempio clustering di failover o mirroring \(a livello di database solo\) **Nota:** Tutte le topologie di distribuzione di AD FS supportano il clustering a livello del servizio AD FS.|No|Yes|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Panoramica delle soluzioni a disponibilità elevata](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>Considerazioni su SQL Server  
È consigliabile considerare i seguenti aspetti se si seleziona SQL Server come database di configurazione per la distribuzione di AD FS.  

-   **Funzionalità SAML e relativo effetto sulle dimensioni e la crescita del database**. Quando sono abilitate le funzionalità di rilevamento riproduzione token SAML o di risoluzione artefatto SAML, ADFS archivia le informazioni nel database di configurazione SQL Server per ogni token di ADFS rilasciato. L'aumento delle dimensioni del database di SQL Server in seguito a questa attività viene considerato non essere significativo e dipende dal periodo di conservazione di riproduzione token configurato. Ogni record artefatto sono una dimensione pari a circa 30 kilobyte \(KB\).  

-   **Numero di server richiesti per la distribuzione**. È necessario aggiungere almeno un server aggiuntivo \(e il numero totale di server necessari per distribuire l'infrastruttura AD FS\) che fungerà da un host dedicato dell'istanza di SQL Server. Se si prevede di usare clustering di failover o il mirroring per fornire tolleranza di errore e scalabilità per il database di configurazione di SQL Server, è necessario un minimo di due istanze di SQL Server.  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Possibile impatto del tipo di database di configurazione scelto sulle risorse hardware  
L'impatto sulle risorse hardware in un server federativo distribuito in una farm utilizzando WID invece di un server federativo distribuito in una farm con database di SQL Server non è significativo. Tuttavia, è importante tenere presente che quando si usa database interno di Windows per la farm, ogni server federativo della farm deve archiviare, gestire e mantenere le modifiche della replica per la copia locale del database di configurazione di ADFS, continuando contemporaneamente fornire la normale operazioni richieste dal servizio federativo.  

In confronto, i server federativi che vengono distribuiti in una farm che usa il database di SQL Server non contengono necessariamente un'istanza locale del database di configurazione di AD FS. Potrebbero quindi avere esigenze leggermente inferiori in termini di risorse hardware.  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Verifica della capacità dell'ambiente di produzione di supportare una distribuzione di ADFS  
Oltre ai server federativi che si desidera distribuire e a seconda di come configurare l'ambiente di produzione esistente, i seguenti server aggiuntivi potrebbe essere necessario fornire l'infrastruttura necessaria per supportare la nuova distribuzione di AD FS:  

-   Controller di dominio Active Directory  

-   Autorità di certificazione \(autorità di certificazione\)  

-   Server Web per ospitare i metadati federativi  

-   Bilanciamento carico di rete \(bilanciamento carico di rete\)  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
