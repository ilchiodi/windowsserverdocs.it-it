---
title: Ripristino della foresta di Active Directory - Identificare il problema
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: cb3cf45f778ff305c8fe5aad5e23f0257650d6f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865522"
---
# <a name="identify-the-problem"></a>Identificare il problema

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
  
Quando vengono visualizzati i sintomi di un errore a livello di foresta, ad esempio nei log eventi o altre soluzioni di monitoraggio, usare il supporto tecnico Microsoft per determinare la causa dell'errore e valutare le possibili soluzioni.  

## <a name="examples-of-forest-wide-failures"></a>Esempi di errori a livello di foresta

- Tutti i controller di dominio sono stati danneggiati logicamente o fisicamente danneggiate a un punto che è impossibile; la continuità aziendale ad esempio, tutte le applicazioni di business che dipendono da Active Directory Domain Services non funzioneranno.  
- Un amministratore non autorizzato sia compromessa l'ambiente Active Directory.  
- Un utente malintenzionato intenzionalmente, o un amministratore accidentalmente, ovvero esegue uno script che distribuisce il danneggiamento dei dati tra la foresta.  
- Un utente malintenzionato intenzionalmente, o un amministratore accidentalmente, si estende lo schema di Active Directory con le modifiche in conflitto o dannose.  
- Un utente malintenzionato è riuscito a installare il software dannoso nei controller di dominio e sono stati avvertiti dal supporto tecnico Microsoft per il ripristino della foresta da un backup.  
  
   > [!IMPORTANT]
   >  In questo documento non illustra le raccomandazioni di sicurezza su come ripristinare una foresta che è stata oggetto di un attacco o compromessa. In generale, è consigliabile per le strategie di attenuazione seguire Pass-the-Hash per la protezione avanzata dell'ambiente. Per altre informazioni, vedere [attacchi Mitigating Pass-the-Hash (PtH) e altre tecniche di furto delle credenziali](https://www.microsoft.com/download/details.aspx?id=36036).
  
- Nessuno dei controller di dominio può eseguire la replica con i partner di replica.  
- Non apportare modifiche al dominio di Active Directory in qualsiasi controller di dominio.  
- Nuovi controller di dominio non può essere installato in tutti i domini.  
  
## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta Active Directory - concepire un piano di ripristino personalizzata-foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta Active Directory - il ripristino di un singolo dominio all'interno di un Multidomain foresta](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta Active Directory - ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
