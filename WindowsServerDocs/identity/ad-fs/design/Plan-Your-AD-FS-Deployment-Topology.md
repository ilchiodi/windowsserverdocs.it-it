---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Pianificare la topologia di distribuzione AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Pianificare la topologia di distribuzione AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Il primo passaggio nella pianificazione di una distribuzione di Active Directory Federation Services \(AD FS\) consiste nel determinare la topologia di distribuzione appropriata per soddisfare le esigenze dell'organizzazione.  
  
Prima di leggere questo argomento, esaminare come dati di AD FS sono archiviati e replicati in altri server federativi in una server farm federativa e assicurarsi di comprendere lo scopo e i metodi di replica che possono essere utilizzati per i dati sottostanti archiviati nel database di configurazione di ADFS.  
  
Esistono due tipi di database che è possibile utilizzare per archiviare i dati di configurazione di ADFS: \(WID\) Database interno di Windows e Microsoft SQL Server. Per ulteriori informazioni, vedere [il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Esaminare i diversi vantaggi e limitazioni di cui è associate all'utilizzo di WID o SQL Server come database di configurazione di ADFS, insieme ai vari scenari di applicazioni che supportano e quindi effettuare una selezione.  
  
> [!IMPORTANT]  
> Per implementare l'opzione per ridimensionare \(if required\) il servizio federativo, il bilanciamento del carico e ridondanza di base, è consigliabile distribuire almeno due server federativi per server farm federativa per tutti gli ambienti di produzione, indipendentemente dal tipo di database che verrà utilizzato.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinazione del tipo di database di configurazione di ADFS per utilizzare  
ADFS utilizza un database per archiviare la configurazione e, in alcuni casi, i dati transazionali relativi al servizio federativo. È possibile utilizzare il software ADFS per selezionare il predefinita \(WID\) Database interno di Windows o Microsoft SQL Server 2008 o versione successiva per archiviare i dati nel servizio federativo.  
  
Per la maggior parte dei casi, i due tipi di database sono relativamente equivalenti. Tuttavia, esistono alcune differenze da tenere presenti prima di iniziare leggere altre informazioni sulle diverse topologie di distribuzione che è possibile utilizzare con AD FS. La tabella seguente descrive le funzionalità supportate le differenze tra un database interno di Windows e un database SQL Server.  
  
||Funzionalità|Database interno di Windows è supportata?|Supportata in SQL Server?
| --- | --- | --- |--- |
|Funzionalità di AD FS|Distribuzione di farm di server federativo|Sì. Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.</br></br>Una farm database interno di Windows non supporta la riproduzione token rilevamento o elemento risoluzione (parte del protocollo di sicurezza Assertion Markup Language (SAML)). |Sì. Non esiste alcun limite imposto per il numero di server federativi che è possibile distribuire in una singola farm.  
|Funzionalità di AD FS|Risoluzione artefatto SAML </br></br>**Nota:** questa funzionalità non è richiesta per scenari Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Sì  
|Funzionalità di AD FS|Rilevamento riproduzione token SAML\/WS-Federation|No|Sì  
|Funzionalità di database|Ridondanza del database di base con la replica pull in cui uno o più server che ospita una sola copia del database richiedono le modifiche vengono apportate in un server di origine che ospita una copia di lettura/scrittura del database|Sì|No 
|Funzionalità di database|Ridondanza del database con soluzioni a disponibilità elevata, ad esempio clustering di failover o mirroring \ (at only\ il livello di database) **Nota:** tutte le topologie di distribuzione di ADFS supportano il clustering a livello di servizio ADFS.|No|Sì  

  
## <a name="sql-server-considerations"></a>Considerazioni su SQL Server  
Se si seleziona SQL Server come database di configurazione per la distribuzione di ADFS, considerare i seguenti aspetti.  
  
-   **SAML funzionalità e i relativi effetti sulle dimensioni del database e la crescita**. Quando sono abilitate la risoluzione artefatto SAML o funzionalità di rilevamento riproduzione token SAML, ADFS archivia le informazioni nel database di configurazione SQL Server per ogni token di ADFS rilasciato. La crescita del database di SQL Server in seguito a questa attività non è considerata significativa e dipende dal periodo di memorizzazione riproduzione token configurato. Ogni record artefatto sono una dimensione di circa 30 kilobyte \(KB\).  
  
-   **Numero di server necessari per la distribuzione**. È necessario aggiungere almeno un server aggiuntivo \ (per il numero totale di server necessari per distribuire il infrastructure\ AD FS) che fungerà da un host dedicato dell'istanza di SQL Server. Se si prevede di usare clustering di failover o mirroring per fornire la tolleranza di errore e la scalabilità per il database di configurazione di SQL Server, è necessario un minimo di due server SQL.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Come il tipo di database di configurazione che si seleziona potrebbe influire sulle risorse hardware  
L'impatto sulle risorse hardware in un server federativo che viene distribuito in una farm con database interno di Windows anziché un server federativo che viene distribuito in una farm con database di SQL Server non è significativo. Tuttavia, è importante considerare che quando si usa database interno di Windows per la farm, ogni server federativo della farm deve archiviare, gestire e mantenere le modifiche della replica per la copia locale del database di configurazione di ADFS continuando a eseguire le normali operazioni che richiede il servizio federativo.  
  
In confronto, i server federativi che vengono distribuiti in una farm che utilizza il database di SQL Server non contengono necessariamente un'istanza locale del database di configurazione di ADFS. Pertanto, potrebbero rendono esigenze leggermente inferiori risorse hardware.  
  
## <a name="BKMK_1"></a>Dove posizionare un server federativo  
Come procedura di sicurezza meglio, posizionare i server federativi AD FS davanti a un firewall e collegarli alla tua rete aziendale per impedirne l'esposizione su Internet. Questo è importante perché i server federativi dispongano di autorizzazioni complete per concedere i token di sicurezza. Di conseguenza, devono avere la stessa protezione di un controller di dominio. Se viene compromesso un server federativo, un utente malintenzionato ha la possibilità di rilasciare token di accesso completo a tutte le applicazioni Web e per i server federativi che sono protetti da ADFS.  
  
> [!NOTE]  
> Come procedura di sicurezza consigliata, evitare che i server federativi direttamente accessibile su Internet. È consigliabile fornire i server federativi accesso diretto a Internet solo quando si impostano su un ambiente di testing o quando l'organizzazione non dispone di una rete perimetrale.  
  
Per le reti aziendali tipiche, viene stabilito un firewall con connessione intranet\ tra la rete aziendale e la rete perimetrale e spesso viene stabilito un firewall di con connessione Internet tra la rete perimetrale e Internet. In questo caso, il server federativo si trova all'interno della rete azienda e non è direttamente accessibile dai client Internet.  
  
> [!NOTE]  
> I computer client connessi alla rete aziendale possono comunicare direttamente con il server federativo tramite autenticazione integrata di Windows.  
  
Un proxy server federativo deve essere inserito nella rete perimetrale prima di configurare i server del firewall per l'utilizzo con AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologie di distribuzione supportate  
Gli argomenti seguenti descrivono le varie topologie di distribuzione che è possibile utilizzare con AD FS. Vengono inoltre descritti i vantaggi e limitazioni di ogni topologia di distribuzione in modo che è possibile selezionare la topologia più appropriata per le esigenze aziendali specifiche.  
  
-   [Server Farm federativa con database interno di Windows](Federation-Server-Farm-Using-WID.md)  
  
-   [Server Farm federativa con WID e proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Server Farm federativa con SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di ADFS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

