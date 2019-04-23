---
title: Considerazioni sull'applicazione
description: Informazioni di compatibilità per le App in MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 400f87c09f1b2e897d67f94e9b7ac12ae0a1e799
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839832"
---
# <a name="application-considerations"></a>Considerazioni sull'applicazione
  
## <a name="application-compatibility"></a>Compatibilità dell'applicazione

Qualsiasi applicazione che si desidera eseguire in un sistema MultiPoint Services deve soddisfare i requisiti seguenti:
  
- Consigliabile installare ed eseguire in Windows Server 2016 
- Deve essere compatibile con sessione in modo che ogni utente può eseguire un'istanza dell'app in un sistema MultiPoint.
  
Se l'applicazione di specificare questo requisito è consigliabile provare a installare l'applicazione e usarlo in una sessione desktop remoto. 

## <a name="addressing-application-compatibility-problems"></a>Risoluzione dei problemi di compatibilità dell'applicazione  
Servizi multiPoint offre la possibilità di associare le stazioni istanze complete di edizioni di Windows 10 Enterprise in esecuzione quasi nello stesso computer host. Per le applicazioni critiche che non verranno eseguita più istanze per più utenti o non verranno installato in un sistema operativo a 64 bit, può essere una soluzione. Distribuzione di desktop in questo modo, è necessario usare la scheda di desktop virtuali in MultiPoint Manager per:  
  
-   Abilita desktop virtuali  
-   Creare un modello di desktop  
-   Personalizzare il modello con l'applicazione di problema  
-   Associare le stazioni con il modello personalizzato  

Ogni stazione viene avviata dallo stesso modello, in modo che tutte le modifiche vengono cancellate ogni volta che il computer viene avviato.  
  
>[!NOTE] 
>È importante verificare i requisiti di licenza per le applicazioni da eseguire su un MultiPoint. Anche se si installa una sola copia applicazioni potrebbero richiedere licenze per utente.  
  
