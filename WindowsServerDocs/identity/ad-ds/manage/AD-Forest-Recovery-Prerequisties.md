---
title: Prerequisiti per la pianificazione di ripristino della foresta Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 672e1f4d0de9bfb2cbe291c5ed715814c8acacd0
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
#<a name="active-directory-forest-recovery-prerequisites"></a>Prerequisiti di ripristino di foresta di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In questo documento illustrati i prerequisiti che è necessario conoscere prima di definire un piano di ripristino della foresta o tentato un ripristino.

## <a name="assumptions-for-using-this-guide"></a>Presupposti per l'uso di questa Guida 

1.  Si è utilizzato con un professionista del supporto Microsoft e:

    - Determinare la causa dell'errore a livello di foresta. Questa Guida non suggerire una causa dell'errore o consigliamo di eventuali procedure per evitare l'errore.  
    - Valutato tutti i possibili rimedi.  
    - Concluso, in consultazione con supporto Microsoft, tale ripristinando intero insieme di strutture allo stato prima dell'errore è il modo migliore per ripristinare in caso di errore. In molti casi, ripristino della foresta deve essere l'ultima opzione.  </br></br>

2. Che siano state seguite le procedure consigliate Microsoft per l'uso di integrate in Active Directory del sistema DNS (Domain Name). In particolare, deve essere presente una zona DNS integrate in Active Directory per ogni dominio di Active Directory. Se non è in questo caso, è possibile utilizzare ancora i principi di base di questa guida per eseguire il ripristino dell'insieme di strutture. Tuttavia, è necessario prendere specifiche misure per il ripristino DNS in base al proprio ambiente. Per ulteriori informazioni sull'utilizzo di DNS integrate in Active Directory, vedere [creazione di una progettazione dell'infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).
3. Sebbene questa guida è da intendersi come una guida generica per il ripristino dell'insieme di strutture, non che sono coperti tutti i possibili scenari. Ad esempio, a partire da Windows Server 2008, esiste una versione Server Core, che è una versione completa di Windows Server, ma senza un'interfaccia utente grafica completa. Sebbene sia possibile ripristinare un insieme di strutture costituita solo i controller di dominio che eseguono Server Core, questa Guida non è istruzioni dettagliate. Tuttavia, in base alle linee guida per l'illustrata di seguito sarà possibile progettare le azioni necessarie della riga di comando manualmente.  
 
>![!NOTE]
> Anche se gli obiettivi di questa guida sono per ripristinare l'insieme di strutture e gestire o ripristinare le funzionalità complete DNS, ripristino può causare una configurazione DNS che è stata modificata dalla configurazione prima dell'errore. Dopo avere recuperata la foresta, è possibile ripristinare la configurazione DNS originale. I consigli forniti in questa guida viene descritto come configurare i server DNS per eseguire la risoluzione dei nomi di altre parti dello spazio dei nomi aziendali in cui sono presenti le zone DNS che non sono archiviate in Active Directory.  

## <a name="concepts-for-using-this-guide"></a>Concetti per l'uso di questa Guida
 Prima di iniziare la pianificazione del ripristino di una foresta di Active Directory, dovresti avere familiarità con le operazioni seguenti:  
  
-   Concetti fondamentali di Active Directory  
  
-   L'importanza dei ruoli di master operazioni (noto anche come FSMO o flexible single master Operation). Questi ruoli, tra cui:  
  
    -   Master schema  
  
    -   Master denominazione domini  
  
    -   Master ID relativo (RID)  
  
    -   Master di dominio primario (PDC) controller emulatore  
  
    -   Master infrastrutture  
  
 Inoltre, si dovrebbe eseguire il backup e ripristinato di dominio Active Directory e SYSVOL in un ambiente di testing su base regolare. Per ulteriori informazioni, vedere [eseguire il backup dello stato del sistema](AD-Forest-Recovery-Procedures.md) e [eseguire un ripristino non autorevole di servizi di dominio Active Directory](AD-Forest-Recovery-Procedures.md).

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
