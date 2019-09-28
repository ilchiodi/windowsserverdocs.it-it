---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Pianificare la topologia di distribuzione di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9cd036e9dd0b249197fb475504c9cad532ead0ea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408038"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Pianificare la topologia di distribuzione di AD FS

Il primo passaggio nella pianificazione di una distribuzione di Active Directory Federation Services \(AD FS @ no__t-1 consiste nel determinare la topologia di distribuzione appropriata per soddisfare le esigenze dell'organizzazione.  
  
Prima di leggere questo argomento, esaminare il modo in cui i dati AD FS vengono archiviati e replicati in altri server federativi di una Federazione server farm e assicurarsi di comprendere lo scopo di e i metodi di replica che possono essere utilizzati per i dati sottostanti archiviati nella AD FS con database figuration.  
  
Per archiviare i dati di configurazione AD FS è possibile utilizzare due tipi di database: Database interno di Windows \(WID @ no__t-1 e Microsoft SQL Server. Per altre informazioni, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Esaminare i vari vantaggi e limitazioni associati all'uso di WID o SQL Server come database di configurazione AD FS, insieme ai vari scenari di applicazione supportati e quindi effettuare la selezione.  
  
> [!IMPORTANT]  
> Per implementare la ridondanza di base, il bilanciamento del carico e l'opzione per ridimensionare il Servizio federativo \(If richiesto @ no__t-1, è consigliabile distribuire almeno due server federativi per ogni server farm di federazione per tutti gli ambienti di produzione, indipendentemente dal tipo di database che si utilizzerà.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Determinazione del tipo di database di configurazione di ADFS da usare  
AD FS utilizza un database per archiviare la configurazione e, in alcuni casi, i dati transazionali correlati al Servizio federativo. È possibile utilizzare il software AD FS per selezionare il database interno di Windows built @ no__t-0cm \(WID @ no__t-2 oppure Microsoft SQL Server 2008 o una versione successiva per archiviare i dati nel Servizio federativo.  
  
I due tipi di database sono relativamente equivalenti per la maggior parte degli scopi. Tuttavia, esistono alcune differenze da tenere presenti prima di iniziare a leggere altre informazioni sulle diverse topologie di distribuzione che è possibile usare con AD FS. Nella tabella seguente sono descritte le differenze tra le funzionalità supportate in Database interno di Windows e nel database SQL Server.  
  
||Funzionalità|Supportata in Database interno di Windows|Supportata in SQL Server
| --- | --- | --- |--- |
|Funzionalità di AD FS|Distribuzione in una server farm federativa|Sì. Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.</br></br>Una farm WID non supporta il rilevamento della riproduzione dei token o la risoluzione dell'artefatto (parte del protocollo Security Assertion Markup Language (SAML)). |Sì. Non esiste un limite imposto per il numero di server federativi che si possono distribuire in una singola farm.  
|Funzionalità di AD FS|Risoluzione artefatto SAML </br></br>**Nota:** Questa funzionalità non è richiesta per scenari Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Yes  
|Funzionalità di AD FS|SAML\/WS\-rilevamento riproduzione token federazione|No|Yes  
|Funzionalità di database|Ridondanza di base del database tramite la replica pull, in cui uno o più server che ospitano una copia Read @ no__t-0only della richiesta del database modificano le modifiche apportate in un server di origine che ospita una copia di lettura @ no__t-1write del database|Yes|No 
|Funzionalità di database|Ridondanza del database con soluzioni High @ no__t-0availability, ad esempio il clustering di failover o il mirroring \(AT solo il livello del database @ no__t-2 **Nota:** Tutte le topologie di distribuzione di AD FS supportano il clustering a livello di servizio AD FS.|No|Yes  

  
## <a name="sql-server-considerations"></a>Considerazioni su SQL Server  
Se si sceglie SQL Server come database di configurazione per la distribuzione di ADFS, è consigliabile tenere presenti i seguenti aspetti.  
  
-   **Funzionalità SAML e relativo effetto sulle dimensioni e la crescita del database**. Quando è abilitata la funzionalità risoluzione artefatto SAML o rilevamento riproduzione token SAML, ADFS archivia le informazioni nel database di configurazione di SQL Server per ogni token di ADFS rilasciato. La crescita del database SQL Server a seguito di questa attività non è considerata significativa e dipende dal periodo di rilevamento riproduzione token configurato. Ogni record di artefatto ha una dimensione pari a circa 30 kilobyte \(KB @ no__t-1.  
  
-   **Numero di server richiesti per la distribuzione**. Sarà necessario aggiungere almeno un server aggiuntivo \(to il numero totale di server necessari per distribuire l'infrastruttura AD FS @ no__t-1 che fungerà da host dedicato dell'istanza di SQL Server. Se si prevede di usare clustering di failover o mirroring per garantire la tolleranza di errore e la scalabilità per il database di configurazione di SQL Server, sono necessari almeno due server SQL.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Possibile impatto del tipo di database di configurazione scelto sulle risorse hardware  
L'impatto sulle risorse hardware di un server federativo distribuito in una farm con Database interno di Windows rispetto a un server federativo distribuito in una farm con il database SQL Server non è significativo. È comunque importante considerare che quando si usa Database interno di Windows per la farm, ogni server federativo deve archiviare, gestire e mantenere le modifiche della replica per la copia locale del database di configurazione di ADFS, continuando contemporaneamente a eseguire le normali operazioni richieste dal Servizio federativo.  
  
Diversamente, i server federativi distribuiti in una farm che usa il database SQL Server non includono necessariamente un'istanza locale dal database di configurazione di ADFS. Potrebbero quindi avere esigenze leggermente inferiori in termini di risorse hardware.  
  
## <a name="BKMK_1"></a>Posizione in cui posizionare un server federativo  
Come procedura di sicurezza consigliata, posizionare AD FS server federativi davanti a un firewall e connetterli alla rete aziendale per impedire l'esposizione a Internet. Questo aspetto è importante perché i server federativi hanno l'autorizzazione completa per concedere i token di sicurezza. Di conseguenza, dovranno avere la stessa protezione di un controller di dominio. Se un server federativo è compromesso, un utente malintenzionato è in grado di rilasciare token di accesso completo a tutte le applicazioni Web e ai server federativi protetti da AD FS.  
  
> [!NOTE]  
> Come procedura di sicurezza consigliata, evitare che i server federativi siano direttamente accessibili su Internet. Si consiglia di concedere ai server federativi l'accesso diretto a Internet solo quando si configura un ambiente lab di test o quando l'organizzazione non dispone di una rete perimetrale.  
  
Per le reti aziendali tipiche, viene stabilito un firewall Intranet @ no__t-0facing tra la rete aziendale e la rete perimetrale e viene spesso stabilito un firewall Internet @ no__t-1facing tra la rete perimetrale e Internet. In questa situazione, il server federativo si trova all'interno della rete aziendale e non è direttamente accessibile dai client Internet.  
  
> [!NOTE]  
> I computer client connessi alla rete aziendale possono comunicare direttamente con il server federativo tramite l'autenticazione integrata di Windows.  
  
È necessario inserire un proxy server federativo nella rete perimetrale prima di configurare i server del firewall per l'uso con AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologie di distribuzione supportate  
Negli argomenti seguenti vengono descritte le diverse topologie di distribuzione che è possibile utilizzare con AD FS. Vengono descritti anche i vantaggi e le limitazioni di ogni topologia di distribuzione, in modo che si possa scegliere quella più appropriata per le esigenze aziendali specifiche.  
  
-   [Server farm federativa che usa Database interno di Windows](Federation-Server-Farm-Using-WID.md)  
  
-   [Server farm federativa che usa Database interno di Windows e proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Server farm federativa che usa SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

