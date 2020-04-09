---
title: Azioni Servizio integrità
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: a4ef08a4ca552211b64d11677153775d6b18b4fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827624"
---
# <a name="health-service-actions"></a>Azioni Servizio integrità

> Si applica a: Windows Server 2019, Windows Server 2016

Il Servizio integrità è una nuova funzionalità di Windows Server 2016 che migliora l'esperienza di monitoraggio e operatività quotidiana per i cluster che eseguono Spazi di archiviazione diretta.

## <a name="actions"></a>Azioni  

La sezione seguente descrive i flussi di lavoro che vengono automatizzati da Servizio integrità. Per verificare che un'azione venga effettivamente eseguita in modo autonomo o per tenere traccia dell’avanzamento o dei risultati, Servizio integrità genera "Azioni". A differenza dei log, le azioni scompaiano subito dopo esser state completate e il loro scopo è principalmente quello di offrire informazioni approfondite sulle attività in corso che possono influire sulle prestazioni o la capacità, ad esempio il ripristino della resilienza o il ribilanciamento di dati.  

### <a name="usage"></a>Utilizzo  

Un nuovo cmdlet di PowerShell consente di visualizzare tutte le azioni:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Coverage  

In Windows Server 2016, il cmdlet **Get-StorageHealthAction** può restituire le informazioni seguenti:  

-   Errore di disattivazione, perdita della connettività o mancata risposta del disco fisico  

-   Cambio di pool di archiviazione per usare il disco fisico di ricambio  

-   Ripristino della resilienza completa dei dati  

-   Ribilanciamento del pool di archiviazione  

## <a name="see-also"></a>Vedere anche

- [Servizio integrità in Windows Server 2016](health-service-overview.md)
- [Documentazione per gli sviluppatori, codice di esempio e informazioni di riferimento sulle API su MSDN](https://msdn.microsoft.com/windowshealthservice)
