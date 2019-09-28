---
title: AD FS risoluzione dei problemi-rilevamento del ciclo
description: Questo documento descrive come risolvere i problemi di rilevamento del ciclo
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2f8842dc53756cc4f65b6d6794a8c4952e111c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385339"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>AD FS risoluzione dei problemi-rilevamento del ciclo 
 
Il ciclo in AD FS si verifica quando un relying party rifiuta continuamente un token di sicurezza valido ed esegue il reindirizzamento a AD FS.

## <a name="loop-detection-cookie"></a>Cookie di rilevamento del ciclo
Per evitare che ciò accada, AD FS ha implementato un cookie di rilevamento del ciclo. Per impostazione predefinita, AD FS scrive un cookie nei client passivi Web denominati **MSISLoopDetectionCookie**. Questo cookie include un valore timestamp e un numero di token emessi.  Ciò consente AD FS di tenere traccia della frequenza e del numero di volte in cui un client ha visitato il Servizio federativo all'interno di un intervallo di tempo specifico.

Se un client passivo visita il Servizio federativo per un token cinque (5) volte entro 20 secondi, AD FS genera l'errore seguente:

**MSIS7042: La stessa sessione del browser client ha eseguito le richieste ' {0}' negli ultimi ' {1}' secondi. Per informazioni dettagliate, contattare l'amministratore.**

L'immissione in cicli infiniti è spesso causata da un comportamento errato relying party applicazione che non utilizza correttamente il token emesso da AD FS e che l'applicazione invia nuovamente il client passivo a AD FS ripetutamente per un nuovo token.  AD FS viene emesso ogni volta il client passivo per un nuovo token, purché non superino 5 richieste entro 20 secondi. 

## <a name="adjusting-the-loop-detection-cookie"></a>Regolazione del cookie di rilevamento del ciclo
È possibile usare PowerShell per modificare il numero di token emessi e il valore TimeSpan.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
Il valore minimo per **LoopDetectionMaximumTokensIssuedInterval** è 1.

Il valore minimo per **LoopDetectionTimeIntervalInSeconds** è 5.

È anche possibile disabilitare il rilevamento del ciclo quando si eseguono test delle prestazioni.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>Non è consigliabile disabilitare in modo permanente il rilevamento del ciclo perché impedisce agli utenti di entrare in Stati di ciclo infinito.


## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)



