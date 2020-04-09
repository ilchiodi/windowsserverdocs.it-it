---
title: Ripristino della foresta di Active Directory - Identificare il problema
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 73b7ef8dc6093ae28c4b5076e7b332b704090b4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823994"
---
# <a name="identify-the-problem"></a>Identificare il problema

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
  
Quando vengono visualizzati i sintomi di un errore a livello di foresta, ad esempio nei registri eventi o in altre soluzioni di monitoraggio, utilizzare supporto tecnico Microsoft per determinare la cause dell'errore e valutare eventuali rimedi possibili.  

## <a name="examples-of-forest-wide-failures"></a>Esempi di errori a livello di foresta

- Tutti i controller di dominio sono stati danneggiati logicamente o fisicamente danneggiati a un punto in cui la continuità aziendale è impossibile. tutte le applicazioni aziendali che dipendono da servizi di dominio Active Directory, ad esempio, non funzionano.  
- Un amministratore non autorizzato ha compromesso l'ambiente Active Directory.  
- Un utente malintenzionato, o un amministratore accidentalmente, esegue uno script che suddivide il danneggiamento dei dati nell'insieme di strutture.  
- Un utente malintenzionato, o un amministratore accidentalmente, estende lo schema Active Directory con modifiche dannose o in conflitto.  
- Un utente malintenzionato ha gestito l'installazione di software dannoso nei controller di dominio ed è stata consigliata da supporto tecnico Microsoft per ripristinare la foresta dal backup.  
  
   > [!IMPORTANT]
   >  In questo documento non vengono illustrate le raccomandazioni sulla sicurezza per il ripristino di una foresta che è stata violata o compromessa. In generale, è consigliabile seguire le tecniche di mitigazione pass-the-hash per rafforzare l'ambiente. Per altre informazioni, vedere [mitigazione degli attacchi pass-the-hash (PtH) e altre tecniche di furto delle credenziali](https://www.microsoft.com/download/details.aspx?id=36036).
  
- Nessuno dei controller di dominio può essere replicato con i partner di replica.  
- Non è possibile apportare modifiche ai servizi di dominio Active Directory in un controller di dominio.  
- Non è possibile installare nuovi controller di dominio in nessun dominio.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta personalizzato](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta di Active Directory-identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta di Active Directory-determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta di Active Directory-Domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta di Active Directory-ripristino di un singolo dominio in una foresta con più domini](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta di Active Directory-ripristino della foresta con i controller di dominio Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
