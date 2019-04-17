---
title: Ripristino della foresta Active Directory - sviluppo di un annuncio piano per il ripristino della foresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adfs
ms.openlocfilehash: 6754509ccac352895b9370e63adf1b69548953bf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Ripristino della foresta Active Directory - sviluppo di un annuncio piano per il ripristino della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

A seconda del tuo ambiente e i requisiti aziendali, si potrebbe o non potrebbe essere necessario eseguire tutti i passaggi descritti in questa guida per eseguire un ripristino ha esito positivo dell'insieme di strutture. Dato che questa guida viene utilizzato solo come un modello di ripristino della foresta, è essenziale che si tracciare un piano di ripristino personalizzato foresta che si adatta al proprio ambiente e soddisfi le esigenze aziendali.  
  
 Nel piano di ripristino di foresta, ad esempio, è una mappa topologica dettagliate dell'insieme di strutture. La mappa dovrebbe elencare tutte le informazioni sui controller di dominio, ad esempio i relativi nomi, i ruoli e lo stato del backup e le relazioni di trust tra di essi. Per uno strumento che è possibile utilizzare per creare una mappa della topologia, vedere [Microsoft Active Directory topologia Diagrammer](https://www.microsoft.com/download/details.aspx?id=13380).  
  
 È consigliabile esercitarsi il piano di ripristino della foresta almeno una volta all'anno. Inoltre, è consigliabile eseguire un drill ripristino foresta quando viene modificato l'appartenenza al gruppo Enterprise Admins o Domain Admins. Questo contribuisce a garantire che il personale IT (IT) comprende appieno il piano di ripristino della foresta.

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
