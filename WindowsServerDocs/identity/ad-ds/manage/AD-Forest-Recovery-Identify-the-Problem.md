---
title: 'Active Directory ripristino della foresta: consente di identificare il problema'
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 1e9ede12dade0b5f1149eae784620520a8866e30
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="identify-the-problem"></a>Identificare il problema

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
  
 Quando vengono visualizzate sintomi di un errore a livello di foresta, ad esempio i registri eventi o altre soluzioni di monitoraggio, funziona con il supporto Microsoft per determinare la causa dell'errore e valutare tutti i possibili rimedi.  
 
## <a name="examples-of-forest-wide-failures"></a>Esempi di errori a livello di foresta 
  
-   Tutti i controller di dominio sono stati logicamente danneggiato o è danneggiato fisicamente a un punto che è impossibile; la continuità aziendale ad esempio, tutte le applicazioni aziendali che dipendono da servizi di dominio Active Directory non sono funzionante.  
  
-   Un amministratore autorizzato ha compromesso l'ambiente Active Directory.  
  
-   Un utente malintenzionato intenzionalmente, o un amministratore accidentalmente, esegue uno script che si estende il danneggiamento dei dati nell'insieme di strutture.  
  
-   Un utente malintenzionato intenzionalmente, o un amministratore accidentalmente, estende lo schema di Active Directory con le modifiche in conflitto o dannose.  
  
-   Un utente malintenzionato è riuscito a installare il software dannoso nei controller di dominio e siano stati informati dal supporto Microsoft per il ripristino della foresta da backup.  
  
    > [!IMPORTANT]
    >  Questo documento illustra suggerimenti relativi alla sicurezza su come recuperare un insieme di strutture che è stato violato o compromesso. In generale, si consiglia di tecniche di prevenzione seguire Pass-the-Hash per la protezione avanzata dell'ambiente. Per ulteriori informazioni, vedere [attacchi Mitigating Pass-the-Hash (PtH) e altre tecniche di furto delle credenziali](https://www.microsoft.com/download/details.aspx?id=36036).  
  
-   Nessuno dei controller di dominio può eseguire la replica con i partner di replica.  
  
-   Non apportare modifiche al dominio di Active Directory in qualsiasi controller di dominio.  
  
-   Impossibile installare nuovi controller di dominio in qualsiasi dominio.  
  
## <a name="next-steps"></a>Passaggi successivi
-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
