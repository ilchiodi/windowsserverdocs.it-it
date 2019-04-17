---
title: Virtualizzazione di ripristino dell'insieme di strutture Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 08b0c4a4c5ed230f91241fcfb67db1749935c5e2
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualizzazione di ripristino di foresta di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Questo argomento descrive la funzionalità clonazione di controller di dominio virtualizzati in Windows Server 2016 e 2012 R2, 2012.  
 
## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Utilizzo di clonazione del controller di dominio virtualizzati per accelerare il ripristino della foresta  
 Clonazione del controller (DC) di dominio virtualizzati semplifica e consente di accelerare il processo per l'installazione di altri controller di dominio virtualizzati in un dominio, soprattutto in percorsi centralizzati, ad esempio centri dati diversi controller di dominio in cui viene eseguito hypervisor. Dopo aver ripristinato un controller di dominio virtuale in ogni dominio dal backup, i controller di dominio aggiuntivo in ogni dominio possono rapidamente portare in linea con il controller di dominio virtualizzati processo di clonazione. È possibile preparare il controller di dominio virtualizzati prima ripristinare, arrestarlo, e quindi copiare il disco rigido virtuale quante volte è necessario per creare clonato virtualizzato controller di dominio per realizzare il dominio.  
  
 I requisiti per la clonazione di controller di dominio virtualizzato sono:  
  
-   L'hypervisor deve supportare VM-GenerationID. Hyper-V in Windows Server 2016, 2012 e Windows 8 è un esempio di un hypervisor che supporta VM-GenerationID. Se è supportato VM-GenerationID, rivolgersi al fornitore dell'hypervisor.  
  
-   Il controller di dominio virtualizzati che viene utilizzato come origine per la clonazione deve eseguire Windows Server 2016 o 2012 ed essere un membro del gruppo di controller di dominio clonabili.  
  
-   L'emulatore PDC deve eseguire Windows Server 2016 o 2012. Se è virtualizzato, è possibile clonare emulatore PDC.  
  
 Per istruzioni dettagliate su come eseguire virtualizzato la clonazione del controller di dominio, vedere [Introduzione alla virtualizzazione di servizi di dominio Active Directory (AD DS) (livello 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Per informazioni dettagliate su come virtualizzato funziona la clonazione del controller di dominio, vedere [virtualizzato Controller di documentazione tecnica su dominio (livello 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md).  

-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 

  
