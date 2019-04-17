---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Determinare la topologia di distribuzione AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinare la topologia di distribuzione AD FS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il primo passaggio nella pianificazione di una distribuzione di Active Directory Federation Services \(AD FS\) è per determinare la topologia di distribuzione appropriata per soddisfare il singolo \(SSO\) accesso-on esigenze dell'organizzazione. Gli argomenti in questa sezione descrivono le varie topologie di distribuzione che è possibile utilizzare con AD FS. Vengono inoltre descritti i vantaggi e limitazioni di ogni topologia di distribuzione in modo che è possibile selezionare la topologia più appropriata per le esigenze aziendali specifiche.  
  
Prima di leggere questo argomento sulla topologia di distribuzione, è consigliabile completare le attività nell'ordine indicato nella tabella seguente.  
  
|Attività consigliata|Descrizione|Riferimento|  
|--------------------|---------------|-------------|  
|Esaminare come dati di AD FS sono archiviati e replicati in altri server federativi in una server farm federativa.|Conoscere lo scopo e i metodi di replica che possono essere utilizzati per i dati sottostanti archiviati nel database di configurazione di ADFS. Questo argomento vengono illustrati i concetti del database di configurazione e descrive i due tipi di database: \(WID\) Database interno di Windows e Microsoft SQL Server.|[Il ruolo del Database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Selezionare il tipo di database di configurazione di ADFS che sarà distribuito nell'organizzazione.|Esaminare i diversi vantaggi e limitazioni di cui è associate all'utilizzo di WID o SQL Server come database di configurazione di ADFS, insieme ai vari scenari di applicazioni che supportano.|[Considerazioni sulla topologia di distribuzione ADFS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Per implementare l'opzione per ridimensionare \(if required\) il servizio federativo, il bilanciamento del carico e ridondanza di base, è consigliabile distribuire almeno due server federativi per server farm federativa per tutti gli ambienti di produzione, indipendentemente dal tipo di database che verrà utilizzato.  
  
Dopo avere esaminato il contenuto della tabella precedente, procedere con i seguenti argomenti in questa sezione:  
  
-   [Server federativo autonomo con database interno di Windows](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Server Farm federativa con database interno di Windows](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Server Farm federativa con WID e proxy](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Server Farm federativa con SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Dopo aver selezionato la topologia di distribuzione di ADFS, che è consigliabile consultare l'argomento [pianificazione della capacità del Server AD FS](Planning-for-AD-FS-Server-Capacity.md) per determinare il numero consigliato di server che è necessario distribuire per supportare questa topologia.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

