---
title: Risoluzione dei problemi di AD FS - rilevamento del ciclo
description: Questo documento descrive come risolvere i problemi di individuazione di loop
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc8eeb11e44da3b8f26b1ab94143c189bca9ed38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830912"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>Risoluzione dei problemi di AD FS - rilevamento del ciclo 
 
Cicli in AD FS, si verifica quando una relying party in modo continuo rifiuta un token di sicurezza valido e viene reindirizzato al AD FS.

## <a name="loop-detection-cookie"></a>Cookie di rilevamento di ciclo
Per evitare che ciò accada, AD FS è implementato il cosiddetto un cookie di rilevamento di ciclo. Per impostazione predefinita, ADFS scrive un cookie al client passivi web denominato **MSISLoopDetectionCookie**. Questo cookie include un valore di timestamp e un numero di token rilasciati valore.  In questo modo AD FS tenere traccia della frequenza e al modo in cui molte volte in cui un client ha visitato il servizio federativo all'interno di un intervallo di tempo specifico.

Se un client passivo visita al servizio federativo per un token da cinque (5) volte all'interno di 20 secondi, AD FS genera l'errore seguente:

**MSIS7042: La stessa sessione del browser client ha apportato '{0}'richieste negli ultimi'{1}' secondi. Per informazioni dettagliate, contattare l'amministratore.**

Immissione in cicli infiniti è spesso causato da un'applicazione relying party che non utilizza correttamente il token emesso da AD FS anomalo e l'applicazione invia al client passivo torna ad AD FS, più volte, per un nuovo token.  AD FS è will emettere client passivo un nuovo token ogni volta, fino a quando non superano 5 richieste entro 20 secondi. 

## <a name="adjusting-the-loop-detection-cookie"></a>Regolare il cookie di rilevamento di ciclo
È possibile usare PowerShell per modificare il numero di token rilasciati valore e il valore di intervallo di tempo.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
Il valore minimo per **LoopDetectionMaximumTokensIssuedInterval** è 1.

Il valore minimo per **LoopDetectionTimeIntervalInSeconds** è 5.

È anche possibile disabilitare l'individuazione di loop quando si esegue il test delle prestazioni.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>È consigliabile non per disabilitare in modo permanente il rilevamento di ciclo come questo impedisce agli utenti di immettere a stati di ciclo infinito.


## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)



