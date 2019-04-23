---
title: Azioni del servizio integrità
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843022"
---
# <a name="health-service-actions"></a>Azioni del servizio integrità

> Si applica a Windows Server 2016

Il servizio integrità è una nuova funzionalità di Windows Server 2016 che consente di migliorare il monitoraggio quotidiano e l'esperienza operativa per i cluster che esegue spazi di archiviazione diretta.

## <a name="actions"></a>Azioni  

La sezione seguente descrive i flussi di lavoro che vengono automatizzati da Servizio integrità. Per verificare che un'azione venga effettivamente eseguita in modo autonomo o per tenere traccia dell’avanzamento o dei risultati, Servizio integrità genera "Azioni". A differenza dei log, le azioni scompaiano subito dopo esser state completate e il loro scopo è principalmente quello di offrire informazioni approfondite sulle attività in corso che possono influire sulle prestazioni o la capacità, ad esempio il ripristino della resilienza o il ribilanciamento di dati.  

### <a name="usage"></a>Uso  

Un nuovo cmdlet di PowerShell consente di visualizzare tutte le azioni:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Ambito del servizio  

In Windows Server 2016, il **Get-StorageHealthAction** cmdlet può restituire le informazioni seguenti:  

-   Errore di disattivazione, perdita della connettività o mancata risposta del disco fisico  

-   Cambio di pool di archiviazione per usare il disco fisico di ricambio  

-   Ripristino della resilienza completa dei dati  

-   Ribilanciamento del pool di archiviazione  

## <a name="see-also"></a>Vedere anche

- [Integrità dei servizi in Windows Server 2016](health-service-overview.md)
- [Documentazione per sviluppatori, codice di esempio e riferimento API su MSDN](https://msdn.microsoft.com/windowshealthservice)
