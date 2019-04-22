---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Pianificare la topologia di distribuzione di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821712"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Pianificare la topologia di distribuzione di AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Il primo passaggio nella pianificazione di una distribuzione di Active Directory Federation Services \(ADFS\) consiste nel determinare la topologia di distribuzione appropriata per soddisfare le esigenze della propria organizzazione.  
  
Prima di leggere questo argomento, esaminare le modalità di archiviazione e replicati in altri server federativi in una server farm federativa in dati di AD FS e assicurarsi di comprendere lo scopo e i metodi di replica che possono essere utilizzati per i dati sottostanti che vengono archiviati in AD FS con database di configurazione.  
  
Esistono due tipi di database che è possibile usare per archiviare i dati di configurazione di AD FS: Database interno di Windows \(WID\) e Microsoft SQL Server. Per altre informazioni, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Esaminare i diversi vantaggi e limitazioni associate tramite WID o SQL Server come database di configurazione di ADFS, insieme ai vari scenari di applicazioni che supportano e quindi effettuare la selezione.  
  
> [!IMPORTANT]  
> Per implementare la ridondanza di base, il bilanciamento del carico e la possibilità di ridimensionare il servizio federativo \(se necessario\), è consigliabile distribuire almeno due server federativi per server farm federativa per tutti gli ambienti di produzione, indipendentemente dal tipo di database che si userà.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Determinazione del tipo di database di configurazione di ADFS da usare  
ADFS utilizza un database per archiviare la configurazione e, in alcuni casi, i dati transazionali relativi al servizio federativo. È possibile usare il software ADFS per selezionare il compilato\-nel Database interno di Windows \(WID\) o Microsoft SQL Server 2008 o versione successiva per archiviare i dati nel servizio federativo.  
  
I due tipi di database sono relativamente equivalenti per la maggior parte degli scopi. Tuttavia, esistono alcune differenze da tenere presenti prima di iniziare la lettura di più sulle tecnologie di distribuzione diversi che è possibile utilizzare con ADFS. Nella tabella seguente sono descritte le differenze tra le funzionalità supportate in Database interno di Windows e nel database SQL Server.  
  
||Funzionalità|Supportata in Database interno di Windows|Supportata in SQL Server
| --- | --- | --- |--- |
|Funzionalità di AD FS|Distribuzione in una server farm federativa|Sì. Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.</br></br>Una farm database interno di Windows non supporta riproduzione token rilevamento o elemento risoluzione (parte del protocollo Security Assertion Markup Language (SAML)). |Sì. Non esiste un limite imposto per il numero di server federativi che si possono distribuire in una singola farm.  
|Funzionalità di AD FS|Risoluzione artefatto SAML </br></br>**Nota:** Questa funzionalità non è richiesta per scenari Microsoft Online Services, Microsoft Office 365, Microsoft Exchange o Microsoft Office SharePoint.|No|Yes  
|Funzionalità di AD FS|SAML\/WS\-rilevamento riproduzione token federazione|No|Yes  
|Funzionalità di database|Tramite la ridondanza dei database Basic effettuare il pull della replica, in cui uno o più server che ospita un'operazione di lettura\-copia solo delle modifiche di richiesta del database che vengono eseguite su un server di origine che ospita una lettura\/scrivere copia del database|Yes|No 
|Funzionalità di database|Ridondanza del database con elevata\-soluzioni di disponibilità, ad esempio clustering di failover o mirroring \(a livello di database solo\) **Nota:** Tutte le topologie di distribuzione di AD FS supportano il clustering a livello del servizio AD FS.|No|Yes  

  
## <a name="sql-server-considerations"></a>Considerazioni su SQL Server  
Se si sceglie SQL Server come database di configurazione per la distribuzione di ADFS, è consigliabile tenere presenti i seguenti aspetti.  
  
-   **Funzionalità SAML e relativo effetto sulle dimensioni e la crescita del database**. Quando è abilitata la funzionalità risoluzione artefatto SAML o rilevamento riproduzione token SAML, ADFS archivia le informazioni nel database di configurazione di SQL Server per ogni token di ADFS rilasciato. La crescita del database SQL Server a seguito di questa attività non è considerata significativa e dipende dal periodo di rilevamento riproduzione token configurato. Ogni record artefatto sono una dimensione pari a circa 30 kilobyte \(KB\).  
  
-   **Numero di server richiesti per la distribuzione**. È necessario aggiungere almeno un server aggiuntivo \(e il numero totale di server necessari per distribuire l'infrastruttura AD FS\) che fungerà da un host dedicato dell'istanza di SQL Server. Se si prevede di usare clustering di failover o mirroring per garantire la tolleranza di errore e la scalabilità per il database di configurazione di SQL Server, sono necessari almeno due server SQL.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Possibile impatto del tipo di database di configurazione scelto sulle risorse hardware  
L'impatto sulle risorse hardware di un server federativo distribuito in una farm con Database interno di Windows rispetto a un server federativo distribuito in una farm con il database SQL Server non è significativo. È comunque importante considerare che quando si usa Database interno di Windows per la farm, ogni server federativo deve archiviare, gestire e mantenere le modifiche della replica per la copia locale del database di configurazione di ADFS, continuando contemporaneamente a eseguire le normali operazioni richieste dal Servizio federativo.  
  
Diversamente, i server federativi distribuiti in una farm che usa il database SQL Server non includono necessariamente un'istanza locale dal database di configurazione di ADFS. Potrebbero quindi avere esigenze leggermente inferiori in termini di risorse hardware.  
  
## <a name="BKMK_1"></a>Dove posizionare un server federativo  
Come procedura di sicurezza consigliata, posizionare i server federativi di ADFS usato un firewall e connetterle alla rete aziendale per impedire l'esposizione su Internet. Questo è importante perché i server federativi hanno autorizzazioni complete per concedere i token di sicurezza. Di conseguenza, dovranno avere la stessa protezione di un controller di dominio. Se viene compromesso un server federativo, un utente malintenzionato ha la possibilità di rilasciare token di accesso completo a tutte le applicazioni Web e per i server federativi che sono protetti da AD FS.  
  
> [!NOTE]  
> Come procedura di sicurezza consigliata, evitare che i server federativi direttamente accessibili su Internet. È consigliabile assegnare i server federativi accesso diretto a Internet solo durante l'impostazione di un ambiente lab di test o quando l'organizzazione non dispone di una rete perimetrale.  
  
Per le reti aziendali tipiche, una rete intranet\-rivolta firewall viene stabilita tra la rete aziendale e la rete perimetrale e Internet\-rivolta firewall spesso viene stabilito tra la rete perimetrale e il Internet. In questo caso, si trova il server federativo nella rete aziendale e non è direttamente accessibile dai client Internet.  
  
> [!NOTE]  
> I computer client connessi alla rete aziendale possono comunicare direttamente con il server di federazione tramite l'autenticazione integrata di Windows.  
  
Un proxy server federativo deve essere posizionato nella rete perimetrale prima di configurare il server del firewall per l'uso con AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologie di distribuzione supportate  
Gli argomenti seguenti descrivono le diverse topologie di distribuzione che è possibile utilizzare con ADFS. Vengono descritti anche i vantaggi e le limitazioni di ogni topologia di distribuzione, in modo che si possa scegliere quella più appropriata per le esigenze aziendali specifiche.  
  
-   [Server Farm federativa con WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Server Farm federativa con WID e proxy](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Server Farm federativa con SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

