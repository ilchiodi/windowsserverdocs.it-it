---
title: Virtualizzazione di ripristino dell'insieme di strutture Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 23317f55fdce18e78ac3e7e1490f6fc4937fd062
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858452"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualizzazione di ripristino di foreste Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Questo argomento descrive la funzionalità clonazione di controller di dominio virtualizzati in Windows Server 2016, 2012 R2 e 2012.  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Uso di clonazione del controller di dominio virtualizzati per accelerare ripristino della foresta

Clonazione del controller (DC) di dominio virtualizzati semplifica e velocizza il processo di installazione di altri controller di dominio virtualizzati in un dominio, soprattutto in posizioni centralizzate, ad esempio i Data Center in cui i vari controller di dominio eseguito in hypervisor. Dopo aver ripristinato un controller di dominio virtuale in ogni dominio da un backup, controller di dominio aggiuntivi in ogni dominio può essere rapidamente portata online da usando il controller di dominio virtualizzato processo di clonazione. È possibile preparare il controller di dominio virtualizzato prima che ripristina, arrestarlo e quindi copiare tale disco rigido virtuale come tante volte quante sono necessarie per creare clonato virtualizzare i controller di dominio per creare il dominio.  
  
I requisiti per la clonazione del controller di dominio virtualizzato sono:  
  
- L'hypervisor deve supportare la VM-GenerationID. Hyper-V in Windows Server 2016, 2012 e Windows 8 è riportato un esempio di un hypervisor che supporta VM-GenerationID. Se la VM-GenerationID è supportata, verificare con il fornitore dell'hypervisor.  
- Il controller di dominio virtualizzato che viene utilizzato come origine per la clonazione deve eseguire Windows Server 2016 o 2012 e da un membro del gruppo di controller di dominio clonabili. 
- L'emulatore PDC deve eseguire Windows Server 2016 o 2012. Se è virtualizzato, è possibile clonare emulatore PDC.  
  
Per istruzioni dettagliate su come eseguire virtualizzato la clonazione di controller di dominio, vedere [Introduction to Active Directory Domain Services (AD DS) Virtualization (Level 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Per informazioni dettagliate su come virtualizzato la clonazione di funzionamento del controller di dominio, vedere [virtualizzati sui Controller di dominio tecnica (livello 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). 

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
