---
title: Ripristino della foresta Active Directory - passaggi per il ripristino della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 1712d3a636160f82495539afdd42ab2ee85ffae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861472"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Ripristino della foresta Active Directory - passaggi per il ripristino della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In questa sezione viene fornita una panoramica del percorso consigliato per il ripristino di un insieme di strutture. La procedura di ripristino della foresta è descritte in dettaglio in un secondo momento.  
  
Nell'elenco seguente vengono riepilogati i passaggi di ripristino a livello generale:  
  
1. [Identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)  

   Usare IT e il supporto tecnico Microsoft per determinare l'ambito del problema e le possibili cause e valutare possibili soluzioni con tutte le parti interessate dell'azienda. In molti casi ripristino della foresta totale deve essere l'ultima opzione.  
  
2. [Decidere come eseguire il ripristino della foresta](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   Dopo aver determinato che il ripristino della foresta è necessario, preliminary completa questa procedura per eseguire in fase di preparazione: determinare la struttura corrente della foresta, identificare le funzioni che ogni controller di dominio esegue, decidere quali controller di dominio per il ripristino per ogni dominio e verificare che tutti i controller di dominio scrivibile vengono portati offline.  

3. [Eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  

   In isolamento, ripristinare un controller di dominio per ogni dominio, pulirli e riconnettere i domini. Reimpostare gli account con privilegi e risolvere i problemi causati da violazioni della sicurezza in questa fase.  
  
4. [Ridistribuire rimanenti i controller di dominio](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   Ridistribuire l'insieme di strutture per ripristinare lo stato prima dell'errore. Questo passaggio dovrà essere adattato per i requisiti e progettazione specifica. Clonazione del controller di dominio virtualizzati consentono di accelerare il processo.  

5. [Pulizia](AD-Forest-Recovery-Cleanup.md)  

   Dopo il ripristino di funzionalità, riconfigurare la risoluzione dei nomi in base alle necessità e ottenere l'uso delle applicazioni LOB.  

I passaggi descritti in questa guida sono progettati per ridurre al minimo la possibilità di reintroduzione dati pericolosi nell'insieme di strutture ripristinato. Potrebbe essere necessario modificare questa procedura per tenere conto di fattori quali:  
  
- Scalabilità  
- Facilità di gestione remota  
- Velocità di recupero  
