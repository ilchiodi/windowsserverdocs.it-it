---
title: Considerazioni sull'applicazione
description: Informazioni compatibilità per le app in Servizi MultiPoint
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 04449db18febdb2e37ac5ef7f5eee4dfc02ba94d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859364"
---
# <a name="application-considerations"></a>Considerazioni sull'applicazione
  
## <a name="application-compatibility"></a>Compatibilità dell'applicazione

Qualsiasi applicazione che si vuole eseguire in un sistema MultiPoint Services deve soddisfare i requisiti seguenti:
  
- Deve essere installato ed eseguito in Windows Server 2016 
- Deve essere compatibile con la sessione in modo che ogni utente possa eseguire un'istanza dell'app in un sistema MultiPoint.
  
Se l'applicazione specifica questo requisito, è consigliabile provare a installare l'applicazione e usarla in una sessione Desktop remoto. 

## <a name="addressing-application-compatibility-problems"></a>Risoluzione dei problemi di compatibilità delle applicazioni  
MultiPoint Services offre la possibilità di associare le stazioni a istanze complete di Windows 10 Enterprise Edition in esecuzione praticamente nello stesso computer host. Per le applicazioni critiche che non eseguono più istanze per più utenti o che non verranno installate in un sistema operativo a 64 bit, può trattarsi di una soluzione. La distribuzione di desktop in questo modo richiede l'uso della scheda desktop virtuali in Gestione MultiPoint per:  
  
-   Abilita desktop virtuali  
-   Creare un modello di desktop  
-   Personalizzare il modello con l'applicazione problema  
-   Associare le stazioni al modello personalizzato  

Ogni stazione inizia dallo stesso modello, quindi tutte le modifiche vengono cancellate ogni volta che il computer viene avviato.  
  
>[!NOTE] 
>È importante verificare i requisiti di licenza per le applicazioni che si vuole eseguire in un MultiPoint. Sebbene si stia installando una copia, le applicazioni potrebbero richiedere licenze per singolo utente.  
  
