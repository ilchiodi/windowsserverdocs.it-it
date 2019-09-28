---
title: Virtualizzazione del ripristino della foresta di Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c055445c2d3aecd8c6d92e94799f556962c977bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390129"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualizzazione del ripristino della foresta Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Questo argomento descrive la funzionalità di clonazione di controller di dominio virtualizzati in Windows Server 2016, 2012 R2 e 2012.  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Uso della clonazione del controller di dominio virtualizzato per velocizzare il ripristino della foresta

La clonazione del controller di dominio virtualizzato semplifica e velocizza il processo di installazione di controller di dominio virtualizzati aggiuntivi in un dominio, soprattutto in posizioni centralizzate quali i Data Center in cui diversi controller di dominio vengono eseguiti su hypervisor. Dopo il ripristino di un controller di dominio virtuale in ogni dominio dal backup, i controller di dominio aggiuntivi in ogni dominio possono essere portati rapidamente online utilizzando il processo di clonazione del controller di dominio virtualizzato. È possibile preparare il primo controller di dominio virtualizzato recuperato, arrestarlo e quindi copiare il disco rigido virtuale tutte le volte che è necessario per creare controller di dominio virtualizzati clonati per compilare il dominio.  
  
I requisiti per la clonazione del controller di dominio virtualizzato sono:  
  
- L'hypervisor deve supportare VM-generazione. Hyper-V in Windows Server 2016, 2012 e Windows 8 è un esempio di hypervisor che supporta VM-generazione. Verificare con il fornitore dell'hypervisor se VM-generazione è supportato.  
- Il controller di dominio virtualizzato utilizzato come origine per la clonazione deve eseguire Windows Server 2016 o 2012 ed essere membro del gruppo controller di dominio clonabili. 
- L'emulatore PDC deve eseguire Windows Server 2016 o 2012. È possibile clonare l'emulatore PDC se è virtualizzato.  
  
Per istruzioni dettagliate su come eseguire la clonazione del controller di dominio virtualizzato, vedere [Introduzione alla virtualizzazione di Active Directory Domain Services (ad DS) (livello 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Per informazioni dettagliate sul funzionamento della clonazione del controller di dominio virtualizzato, vedere [riferimento tecnico per i controller di dominio virtualizzati (livello 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). 

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
