---
title: Ripristino della foresta Active Directory - i passaggi per il ripristino della foresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 29988c262897649173e039cc052bb64f38a1a0ca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
#<a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Ripristino della foresta Active Directory - i passaggi per il ripristino della foresta 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In questa sezione viene fornita una panoramica del percorso consigliato per il ripristino di un insieme di strutture. Le operazioni di ripristino della foresta sono descritte in dettaglio più avanti.  
  
 Nell'elenco seguente vengono riepilogati i passaggi di ripristino a livello generale:  
  
1.  [Identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)  
  
     Lavorare con IT e il supporto Microsoft per determinare l'ambito del problema e le possibili cause e valutare le possibili soluzioni con tutte le parti interessate dell'azienda. In molti casi ripristino della foresta totale deve essere l'ultima opzione.  
  
2.  [Decidere come ripristinare l'insieme di strutture](AD-Forest-Recovery-Determine-how-to-Recover.md)  
  
     Dopo avere determinato che è necessario ripristino della foresta, preliminary completato i passaggi per preparare la: determinare la struttura della foresta corrente, identificare le funzioni che ogni controller di dominio esegue, decidere quali controller di dominio per il ripristino per ogni dominio e assicurarsi che tutti i controller di dominio scrivibile vengono disattivate.  
  
3.  [Eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
  
     Separatamente, ripristinare un controller di dominio per ogni dominio e i domini di riconnettersi eliminarli. Reimpostare l'account con privilegi e risolvere i problemi causati da violazioni della sicurezza in questa fase.  
  
4.  [Ridistribuire restanti controller di dominio](AD-Forest-Recovery-Restore-Additional-DCs.md)  
  
     Ridistribuire della foresta per tornare allo stato prima dell'errore. È necessario adattare la progettazione specifiche e i requisiti di questo passaggio. Clonazione del controller di dominio virtualizzati consente di accelerare il processo.  
  
5.  [Pulizia](AD-Forest-Recovery-Cleanup.md)  
  
     Dopo la funzionalità è stata ripristinata, riconfigurare la risoluzione dei nomi in base alle esigenze e ottenere applicazioni LOB funziona.  

  
 I passaggi descritti in questa guida sono progettati per ridurre al minimo il rischio di causare dati pericolosi nell'insieme di strutture ripristinato. Potrebbe essere necessario modificare questi passaggi per conto di fattori quali:  
  
-   Scalabilità  
-   Gestione remota  
-   Velocità di ripristino  

