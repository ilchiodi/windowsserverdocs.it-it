---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Determinare la topologia di distribuzione di ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b9128dded44e83acc63cef6785a1949e614cf6a7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408113"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinare la topologia di distribuzione di ADFS

Il primo passaggio nella pianificazione di una distribuzione di Active Directory Federation Services \(AD FS\) consiste nel determinare la topologia di distribuzione appropriata per soddisfare le esigenze del\-Single Sign-on \(SSO della propria organizzazione.\) Negli argomenti di questa sezione vengono descritte le diverse topologie di distribuzione che è possibile utilizzare con AD FS. Vengono descritti anche i vantaggi e le limitazioni di ogni topologia di distribuzione, in modo che si possa scegliere quella più appropriata per le esigenze aziendali specifiche.  
  
Prima di leggere questo argomento sulla topologia di distribuzione, è consigliabile completare le attività nell'ordine descritto nella tabella seguente.  
  
|Attività consigliata|Descrizione|Riferimento|  
|--------------------|---------------|-------------|  
|Esaminare il modo in cui i dati AD FS vengono archiviati e replicati in altri server federativi in una server farm federativa.|Conoscere lo scopo e i metodi di replica che si possono usare per i dati sottostanti archiviati nel database di configurazione di ADFS. In questo argomento vengono presentati i concetti del database di configurazione e vengono descritti i due tipi di database: database interno di Windows \(WID\) e Microsoft SQL Server.|[Ruolo del database di configurazione di AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Selezionare il tipo di database di configurazione di ADFS che sarà distribuito nell'organizzazione.|Esaminare i diversi vantaggi e le limitazioni associati all'uso del Database interno di Windows o di SQL Server come database di configurazione di ADFS, oltre ai vari scenari di applicazioni supportati.|[Considerazioni sulla topologia di distribuzione di AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Per implementare la ridondanza di base, il bilanciamento del carico e l'opzione per ridimensionare il Servizio federativo \(se necessario\), è consigliabile distribuire almeno due server federativi per server farm di federazione per tutti gli ambienti di produzione, indipendentemente dal tipo di database che si utilizzerà.  
  
Dopo avere esaminato il contenuto della tabella precedente, passare agli argomenti successivi di questa sezione:  
  
-   [Server federativo autonomo che usa Database interno di Windows](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Server farm federativa che usa Database interno di Windows](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Server farm federativa che usa Database interno di Windows e proxy](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Server farm federativa che usa SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Al termine della selezione della topologia di distribuzione di AD FS, è consigliabile consultare l'argomento [pianificazione della capacità del server ad FS](Planning-for-AD-FS-Server-Capacity.md) per determinare il numero consigliato di server che è necessario distribuire per supportare questa topologia.  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

