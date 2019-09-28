---
title: Ripristino della foresta di Active Directory-passaggi per il ripristino della foresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 07a043c4361f8eaae30b1dea665c604c0df42333
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390293"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Ripristino della foresta di Active Directory-passaggi per il ripristino della foresta

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In questa sezione viene fornita una panoramica del percorso consigliato per il ripristino di una foresta. I passaggi per il ripristino della foresta sono descritti in dettaglio più avanti.  
  
Nell'elenco seguente vengono riepilogati i passaggi di ripristino a un livello elevato:  
  
1. [Identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)  

   Collaborare con esso e supporto tecnico Microsoft per determinare l'ambito del problema e le possibili cause e valutare le possibili soluzioni con tutti gli stakeholder aziendali. In molti casi il ripristino della foresta totale deve essere l'ultima opzione.  
  
2. [Decidere come ripristinare la foresta](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   Dopo aver determinato che il ripristino della foresta è necessario, completare le procedure preliminari per prepararlo: determinare la struttura della foresta corrente, identificare le funzioni eseguite da ogni controller di dominio, decidere quale controller di dominio ripristinare per ogni dominio e assicurarsi che tutti i controller di dominio scrivibili vengono portati offline.  

3. [Eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  

   In isolamento, ripristinare un controller di dominio per ogni dominio, pulirli e riconnettere i domini. Reimpostare gli account con privilegi e correggere i problemi causati da violazioni della sicurezza in questa fase.  
  
4. [Ridistribuire i controller di dominio rimanenti](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   Ridistribuire la foresta per ripristinarne lo stato prima dell'errore. Questo passaggio deve essere adattato alla progettazione e ai requisiti specifici. La clonazione di controller di dominio virtualizzati consente di velocizzare questo processo.  

5. [Pulizia](AD-Forest-Recovery-Cleanup.md)  

   Una volta ripristinata la funzionalità, riconfigurare la risoluzione dei nomi in base alle esigenze e ottenere il funzionamento delle applicazioni LOB.  

I passaggi descritti in questa guida sono progettati per ridurre al minimo la possibilità di reintrodurre dati pericolosi nella foresta ripristinata. Potrebbe essere necessario modificare questi passaggi per tenere conto dei fattori seguenti:  
  
- Scalabilità  
- Gestibilità remota  
- Velocità di ripristino  
