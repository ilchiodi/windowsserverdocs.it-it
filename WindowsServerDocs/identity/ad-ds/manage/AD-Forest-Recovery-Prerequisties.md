---
title: Prerequisiti per la pianificazione di ripristino della foresta Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c8945dd5ccccb27826dd96413b56a070a7452789
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842172"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Prerequisiti di ripristino di foreste Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Il documento seguente illustra i prerequisiti che è necessario conoscere prima di definire un piano di ripristino della foresta o tentare un ripristino.

## <a name="assumptions-for-using-this-guide"></a>Presupposti per l'uso di questa Guida

1. Chi ha familiarità con un professionista del supporto Microsoft e:
   - Determinare la causa dell'errore a livello di foresta. Questa Guida non una causa dell'errore suggeriresti tutte le procedure per impedire che un errore.
   - Valutare eventuali possibili soluzioni.  
   - Conclusione, in accordo con supporto tecnico Microsoft, che il ripristino dell'intera foresta al relativo stato prima che si è verificato l'errore è il modo migliore per correggere l'errore. In molti casi, ripristino della foresta deve essere l'ultima opzione.

2. Che siano state seguite le procedure consigliate Microsoft per l'utilizzo integrato di Active Directory del sistema DNS (Domain Name). In particolare, deve essere presente una zona di DNS integrate in Active Directory per ogni dominio di Active Directory. 
   - Se ciò non avviene, è possibile usare ancora i principi di base di questa guida per eseguire il ripristino dell'insieme di strutture. Tuttavia, è necessario adottare misure specifiche per il ripristino DNS in ambiente di base. Per altre informazioni sull'uso di DNS integrate in Active Directory, vedere [creazione di un progetto di infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

3. Sebbene questa guida è da intendersi come una guida generica per il ripristino dell'insieme di strutture, non tutte le possibili gli scenari. Ad esempio, a partire da Windows Server 2008, è presente una versione di Server Core, ovvero una versione completa di Windows Server ma senza un'interfaccia utente grafica completa. Sebbene sia certamente possibile ripristinare una foresta costituito semplicemente i controller di dominio che eseguono Server Core, questa Guida non ha nessuna istruzioni dettagliate. Tuttavia, le linee guida illustrate di seguito in base sarà in grado di progettare le azioni della riga di comando manualmente.  

> ![!NOTE]
> Sebbene gli obiettivi di questa Guida al ripristino della foresta e mantenere o ripristinare la funzionalità DNS completa, il ripristino può comportare una configurazione DNS che è stata modificata dalla configurazione prima dell'errore. Dopo avere recuperato l'insieme di strutture, è possibile ripristinare la configurazione DNS originale. I consigli riportati in questa guida viene descritto come configurare i server DNS per eseguire la risoluzione dei nomi delle altre parti dello spazio dei nomi aziendali in cui sono presenti le zone DNS che non sono archiviate in Active Directory Domain Services.  

## <a name="concepts-for-using-this-guide"></a>Concetti per l'uso di questa Guida

Prima di iniziare la pianificazione del ripristino di una foresta di Active Directory, è necessario avere familiarità con gli elementi seguenti:  
  
- Concetti fondamentali di Active Directory  
- L'importanza dei ruoli di master operazioni (noto anche come FSMO flexible single master operations). Questi ruoli includono quanto segue:  
   - Master schema
   - Master denominazione domini
   - Master ID relativo (RID)
   - Master di emulatore dominio primario (PDC) controller
   - Master infrastrutture

Inoltre, si deve avere eseguito il backup e ripristinare Active Directory e SYSVOL in un ambiente lab a intervalli regolari. Per altre informazioni, vedere [backup dei dati dello stato del sistema](AD-Forest-Recovery-Procedures.md) e [esegue un ripristino non autorevole di servizi di dominio Active Directory](AD-Forest-Recovery-Procedures.md).

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
