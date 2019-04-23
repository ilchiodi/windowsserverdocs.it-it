---
title: Ripristino della foresta Active Directory - il ripristino di un singolo dominio in una foresta con più domini
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: fae2cc40af0b43dd38d72c2622720a6bb17b0a66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863472"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Ripristino della foresta Active Directory - il ripristino di un singolo dominio in una foresta con più domini

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Talvolta potrebbe presentarsene quando è necessario ripristinare solo un singolo dominio all'interno di una foresta che abbia più domini, anziché un ripristino della foresta completo. In questo argomento illustra considerazioni per il ripristino di un singolo dominio e le possibili strategie di ripristino.  
  
Il ripristino di un singolo dominio costituisce una sfida singolare della ricostruzione dei server di catalogo globale (GC). Ad esempio, se il primo controller di dominio (DC) per il dominio viene ripristinato da un backup creato in precedenza una settimana, quindi tutti gli altri cataloghi globali nella foresta avranno più dati aggiornati per il dominio di controller di dominio ripristinato. Per ristabilire la coerenza dei dati di catalogo globale, sono disponibili due opzioni:  
  
- Unhost e quindi rehost partizione domini recuperati da tutti i cataloghi globali nella foresta, ad eccezione di quelli nel dominio ripristinato, nello stesso momento.  
- Seguire il processo di ripristino dell'insieme di strutture per ripristinare il dominio e quindi rimuovere oggetti residui da cataloghi globali in altri domini.  
  
Le sezioni seguenti offrono considerazioni generali per ogni opzione. Il set completo di passaggi che devono essere eseguite per il ripristino varia per diversi ambienti di Active Directory.  
  
## <a name="rehost-all-gcs"></a>Rehosting di tutti i cataloghi globali  

> [!WARNING]
> La password dell'account amministratore di dominio per tutti i domini deve essere pronta per l'uso nel caso in cui un problema impedisce l'accesso a un catalogo globale per l'accesso.  

Rehosting di tutti i cataloghi globali può essere eseguita mediante repadmin / unhost e i comandi repadmin /rehost (parte di repadmin /experthelp). Eseguire i comandi repadmin su ogni Garbage Collection in ogni dominio che non viene recuperato. Deve essere garantita, che tutti i cataloghi globali non contengono più una copia del dominio ripristinato. A tale scopo, unhost la partizione di dominio prima di tutto da tutti i controller di dominio in tutti i domini della foresta nessuno precedente al recupero prima di tutto. Dopo che tutti i cataloghi globali non hanno più la partizione, è possibile riallocare lo. Quando il rehosting, si consideri la struttura del sito e la replica dell'insieme di strutture, ad esempio, completare il rehosting di un controller di dominio per ogni sito prima della riallocazione altri controller di dominio di tale sito.  
  
Questa opzione può risultare utile per un'organizzazione di piccole dimensioni con solo pochi controller di dominio per ogni dominio. Tutti i cataloghi globali potrebbe essere ricreati in un venerdì sera e, se necessario, completare la replica per dominio di sola lettura esegue prima di lunedì mattina. Ma se è necessario ripristinare un dominio di grandi dimensioni che copre i siti in tutto il mondo, riallocazione della partizione di dominio di sola lettura in tutti i cataloghi globali per gli altri domini possono significativamente le operazioni di impatto e potenzialmente richiedono tempi di inattività.  
  
## <a name="remove-lingering-objects"></a>Rimuovere oggetti residui

Come per il processo di ripristino della foresta, è ripristinare un controller di dominio da un backup del dominio che è necessario ripristinare, eseguire la pulizia dei metadati di controller di dominio rimanenti e quindi installare nuovamente Active Directory Domain Services per compilare il dominio. Nel server di catalogo globale di tutti gli altri domini nella foresta, è rimuovere gli oggetti residui per la partizione di sola lettura del dominio ripristinato.  

L'origine per la pulizia di oggetti residui deve essere un controller di dominio nel dominio ripristinato. Per essere certi che il controller di dominio di origine non dispone di tutti gli oggetti residui per tutte le partizioni di dominio, è possibile rimuovere il catalogo globale, se si tratta di un catalogo globale.  

Rimozione di oggetti residui è vantaggiosa per le grandi organizzazioni che non è possibile rischio di tempi di inattività associato con le altre opzioni.  

Per altre informazioni, vedere [utilizzare Repadmin per rimuovere oggetti residui](https://technet.microsoft.com/library/cc785298.aspx).

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
