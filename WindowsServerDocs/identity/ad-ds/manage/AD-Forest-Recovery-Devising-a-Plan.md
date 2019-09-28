---
title: Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: 0ef0fbc19f1b3ba5a46fe09f66da6721f2e84712
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369155"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

A seconda dell'ambiente e dei requisiti aziendali, è possibile che non sia necessario eseguire tutti i passaggi descritti in questa guida per eseguire correttamente il ripristino della foresta. Dato che questa guida funge solo da modello per il ripristino della foresta, è essenziale definire un piano di ripristino della foresta personalizzato adatto all'ambiente in uso e soddisfare le esigenze aziendali.  
  
Nel piano di ripristino della foresta, ad esempio, è necessario disporre di una mappa topologica dettagliata delle foreste. La mappa dovrebbe elencare tutte le informazioni sui controller di dominio, ad esempio i nomi, i ruoli e lo stato di backup e le relazioni di trust tra di essi. Per uno strumento che è possibile usare per creare una mappa della topologia, vedere [Microsoft Active Directory topologia Diagrammer](https://www.microsoft.com/download/details.aspx?id=13380).  
  
È consigliabile provare il piano di ripristino della foresta almeno una volta all'anno. Inoltre, è consigliabile eseguire un'esercitazione sul ripristino della foresta quando sono presenti modifiche di appartenenza al gruppo Enterprise Admins o Domain Admins. Ciò consente di garantire che il personale IT (Information Technology) conosca pienamente il piano di ripristino della foresta.

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
