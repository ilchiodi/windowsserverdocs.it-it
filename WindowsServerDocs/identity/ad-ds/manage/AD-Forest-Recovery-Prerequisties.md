---
title: Prerequisiti per la pianificazione del ripristino della foresta Active Directory
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 6dcd1185ba4d4c847cfe212f78ccc9661fd2aead
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823784"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Prerequisiti per il ripristino della foresta Active Directory

> Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Il documento seguente illustra i prerequisiti che è necessario conoscere prima di definire un piano di ripristino della foresta o tentare un ripristino.

## <a name="assumptions-for-using-this-guide"></a>Presupposti per l'uso di questa guida

1. Si è lavorato con una supporto tecnico Microsoft Professional e:
   - Determinazione della cause dell'errore a livello di foresta. In questa guida non viene suggerita la presenza di un errore o una procedura consigliata per evitare l'errore.
   - Sono state valutate le possibili soluzioni.  
   - Conclusa, in consultazione con supporto tecnico Microsoft, che il ripristino dell'intera foresta allo stato precedente all'errore è il modo migliore per risolvere il problema. In molti casi, il ripristino della foresta deve essere l'ultima opzione.

1. È stata seguita la procedura consigliata Microsoft per l'uso di Active Directory-Integrated Domain Name System (DNS). In particolare, deve essere presente una zona DNS integrata Active Directory per ogni dominio di Active Directory.
   - In caso contrario, è comunque possibile utilizzare i principi di base di questa guida per eseguire il ripristino della foresta. Tuttavia, sarà necessario adottare misure specifiche per il ripristino DNS in base al proprio ambiente. Per altre informazioni sull'uso di Active Directory DNS integrato, vedere [creazione di un progetto di infrastruttura DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

1. Sebbene questa guida sia destinata a una guida generica per il ripristino della foresta, non tutti gli scenari possibili sono trattati. Ad esempio, a partire da Windows Server 2008, esiste una versione Server Core, ovvero una versione completa di Windows Server, ma senza un'interfaccia utente grafica completa. Sebbene sia certamente possibile ripristinare una foresta costituita da solo controller di dominio che eseguono Server Core, in questa guida non sono disponibili istruzioni dettagliate. Tuttavia, in base alle linee guida illustrate in questo articolo, sarà possibile progettare le azioni da riga di comando necessarie.  

> [!NOTE]
> Sebbene gli obiettivi di questa guida siano il ripristino della foresta e la gestione o il ripristino della funzionalità DNS completa, il ripristino può comportare una configurazione DNS modificata rispetto alla configurazione prima dell'errore. Dopo il ripristino della foresta, è possibile ripristinare la configurazione DNS originale. I consigli riportati in questa guida non descrivono come configurare i server DNS per eseguire la risoluzione dei nomi di altre parti dello spazio dei nomi aziendale in cui sono presenti zone DNS che non sono archiviate in servizi di dominio Active Directory.  

## <a name="concepts-for-using-this-guide"></a>Concetti relativi all'uso di questa guida

Prima di iniziare a pianificare il ripristino di una foresta di Active Directory, è necessario avere familiarità con quanto segue:  
  
- Concetti fondamentali di Active Directory  
- Importanza dei ruoli di master operazioni, noti anche come FSMO (Flexible Single Master Operation). Questi ruoli includono i seguenti:  
  - Master schema
  - Master per la denominazione dei domini
  - Master di ID relativo (RID)
  - Master emulatore controller di dominio primario (PDC)
  - Master infrastrutture

Inoltre, è necessario aver eseguito il backup e il ripristino di servizi di dominio Active Directory e SYSVOL in un ambiente lab a intervalli regolari. Per ulteriori informazioni, vedere [backup dei dati sullo stato del sistema](AD-Forest-Recovery-Procedures.md) ed [esecuzione di un ripristino non autorevole di Active Directory Domain Services](AD-Forest-Recovery-Procedures.md).

## <a name="next-steps"></a>Passaggi successivi

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta personalizzato](AD-Forest-Recovery-Devising-a-Plan.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory-identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory-determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory-Domande frequenti](AD-Forest-Recovery-FAQ.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory-ripristino di un singolo dominio in una foresta con più domini](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)

> [!div class="nextstepaction"]
> [Ripristino della foresta di Active Directory-ripristino della foresta con i controller di dominio Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
