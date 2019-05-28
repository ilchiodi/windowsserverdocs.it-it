---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Determinare la topologia di distribuzione di ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 06cc4bd37905f6bb7afbc513ffce216104654aba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191477"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinare la topologia di distribuzione di ADFS

Il primo passaggio nella pianificazione di una distribuzione di Active Directory Federation Services \(ADFS\) consiste nel determinare la topologia di distribuzione appropriata per soddisfare l'accesso single sign\-sul \(SSO\) necessita del organizzazione. In questa sezione vengono illustrate le diverse topologie di distribuzione che è possibile utilizzare con ADFS. Vengono descritti anche i vantaggi e le limitazioni di ogni topologia di distribuzione, in modo che si possa scegliere quella più appropriata per le esigenze aziendali specifiche.  
  
Prima di leggere questo argomento sulla topologia di distribuzione, è consigliabile completare le attività nell'ordine descritto nella tabella seguente.  
  
|Attività consigliata|Descrizione|Riferimenti|  
|--------------------|---------------|-------------|  
|Esaminare le modalità di archiviazione e replicati in altri server federativi in una server farm federativa in dati di AD FS.|Conoscere lo scopo e i metodi di replica che si possono usare per i dati sottostanti archiviati nel database di configurazione di ADFS. Questo argomento introduce i concetti sul database di configurazione e descrive i due tipi di database: Database interno di Windows \(WID\) e Microsoft SQL Server.|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Selezionare il tipo di database di configurazione di ADFS che sarà distribuito nell'organizzazione.|Esaminare i diversi vantaggi e le limitazioni associati all'uso del Database interno di Windows o di SQL Server come database di configurazione di ADFS, oltre ai vari scenari di applicazioni supportati.|[Considerazioni sulla topologia di distribuzione di AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Per implementare la ridondanza di base, il bilanciamento del carico e la possibilità di ridimensionare il servizio federativo \(se necessario\), è consigliabile distribuire almeno due server federativi per server farm federativa per tutti gli ambienti di produzione, indipendentemente dal tipo di database che si userà.  
  
Dopo avere esaminato il contenuto della tabella precedente, passare agli argomenti successivi di questa sezione:  
  
-   [Server federativo autonomo che usa Database interno di Windows](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Server farm federativa che usa Database interno di Windows](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Server farm federativa che usa Database interno di Windows e proxy](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Server farm federativa che usa SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Dopo aver selezionato la topologia di distribuzione di AD FS, è consigliabile rivedere l'argomento [pianificazione della capacità dei Server AD FS](Planning-for-AD-FS-Server-Capacity.md) per determinare il numero consigliato di server che è necessario distribuire per supportare questa topologia.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

