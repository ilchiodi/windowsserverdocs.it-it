---
title: Ripristino della foresta Active Directory - concepire un annuncio piano di ripristino della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: edfc874fe030c6394bc8bda95c49e61951e78f43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881782"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Ripristino della foresta Active Directory - concepire un annuncio piano di ripristino della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

A seconda dell'ambiente e i requisiti aziendali, si può o potrebbe non essere necessario eseguire tutti i passaggi descritti in questa guida per eseguire un ripristino della foresta ha esito positivo. Dato che questa guida viene utilizzato solo come modello per il ripristino della foresta, è fondamentale che è definire un piano di ripristino dell'insieme di strutture personalizzato che soddisfi l'ambiente e le esigenze aziendali.  
  
Ad esempio, nel piano di ripristino di foreste, è necessario per una mappa della topologia dettagliata delle foreste di. La mappa dovrebbe elencare tutte le informazioni sul controller di dominio, quali i relativi nomi, i relativi ruoli e lo stato del backup e le relazioni di trust tra di essi. Per uno strumento che è possibile usare per creare una mappa della topologia, vedere [Microsoft Active Directory topologia Diagrammer](https://www.microsoft.com/download/details.aspx?id=13380).  
  
Sarà necessario applicare il piano di ripristino della foresta almeno una volta all'anno. Inoltre, è consigliabile eseguire un'esercitazione sul ripristino di foreste quando sono presenti modifiche all'appartenenza al gruppo Enterprise Admins o Domain Admins. Ciò garantisce che il personale di information technology (IT) a fondo il piano di ripristino della foresta.

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
