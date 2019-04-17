---
title: "Azioni del servizio integrità"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-actions"></a>Azioni del servizio integrità

> Si applica a Windows Server 2016

Il servizio integrità è una nuova funzionalità di Windows Server 2016 che consente di migliorare il monitoraggio e l'esperienza per i cluster che eseguono spazi di archiviazione diretta.

## <a name="actions"></a>Azioni  

La sezione seguente descrive i flussi di lavoro che vengono automatizzati da servizio integrità. Per verificare che un'azione venga effettivamente eseguita in modo autonomo o per tenere traccia dell'avanzamento o dei risultati, il servizio integrità genera "Azioni". A differenza dei log, azioni scompaiano subito dopo aver completato e sono destinati principalmente per fornire informazioni sulle attività in corso che possono influire sulle prestazioni o capacità (ad esempio, ripristino della resilienza o il ribilanciamento di dati).  

### <a name="usage"></a>Utilizzo  

Un nuovo cmdlet PowerShell Visualizza tutte le azioni:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Copertura  

In Windows Server 2016, il **Get-StorageHealthAction** cmdlet può restituire le informazioni seguenti:  

-   Disattivazione non riuscita, perdita della connettività o mancata risposta del disco fisico  

-   Cambio di pool di archiviazione da usare disco fisico di ricambio  

-   Ripristino della resilienza completa ai dati  

-   Ribilanciamento del pool di archiviazione  

## <a name="see-also"></a>Vedere anche

- [Servizio di integrità in Windows Server 2016](health-service-overview.md)
- [Documentazione per sviluppatori, il codice di esempio e di riferimento all'API su MSDN](https://msdn.microsoft.com/windowshealthservice)
