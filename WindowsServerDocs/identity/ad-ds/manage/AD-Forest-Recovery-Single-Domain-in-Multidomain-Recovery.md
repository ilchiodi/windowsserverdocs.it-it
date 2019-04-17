---
title: Ripristino della foresta Active Directory - ripristino di un singolo dominio in una foresta con più domini
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adfs
ms.openlocfilehash: 3a1c9d0671a732eee83aa707e061afdbbf106fa5
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2018
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Ripristino della foresta Active Directory - ripristino di un singolo dominio in una foresta con più domini

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Possono esserci orari in cui è necessario ripristinare un solo dominio all'interno di una foresta con più domini, invece di un ripristino completo dell'insieme di strutture. In questo argomento illustra considerazioni sul ripristino di un unico dominio e le possibili strategie per il ripristino.  
  
 Il ripristino di un singolo dominio costituisce una sfida unica della ricostruzione dei server di catalogo globale (GC). Ad esempio, se il primo controller di dominio (DC) per il dominio viene ripristinato da un backup creato in precedenza una settimana, quindi tutti gli altri cataloghi globali nell'insieme di strutture avrà più i dati aggiornati per tale dominio rispetto a controller di dominio ripristinato. Per ristabilire la coerenza dei dati di catalogo globale, sono disponibili due opzioni:  
  
-   Unhost e quindi rehost la partizione di domini ripristinati da tutti i cataloghi globali nell'insieme di strutture, ad eccezione di quelli nel dominio ripristinato, nello stesso momento.  
  
-   Seguire il processo di ripristino per ripristinare il dominio dell'insieme di strutture e quindi rimuovere oggetti residui da cataloghi globali in altri domini.  
  
 Le sezioni seguenti forniscono le considerazioni generali per ogni opzione. L'intero set di passaggi che devono essere eseguite per il ripristino variano per gli ambienti Active Directory diversi.  
  
## <a name="rehost-all-gcs"></a>Tutti i cataloghi globali rehost  

> [!WARNING]
>  La password dell'account amministratore di dominio per tutti i domini deve essere pronta per l'utilizzo nel caso in cui un problema impedisce l'accesso a un catalogo globale per l'accesso.  

 Caso di riallocazione di tutti i cataloghi globali può essere eseguita mediante repadmin//unhost e repadmin /rehost comandi (parte di repadmin /experthelp). Eseguire i comandi repadmin in ogni catalogo globale in ogni dominio che non viene risolto. Deve essere garantita, che tutti i cataloghi globali non hanno più una copia del dominio ripristinato. A tale scopo, unhost la partizione di dominio prima di tutti i controller di dominio in tutti i domini della foresta ripristinato di nessuno prima. Dopo che tutti i cataloghi globali non contengono più la partizione, è possibile rehost. In caso di riallocazione, prendere in considerazione la struttura del sito e la replica dell'insieme di strutture, ad esempio, completare la rehost di un controller di dominio per ogni sito prima di caso di riallocazione di altri controller di dominio di tale sito.  
  
 Questa opzione può risultare vantaggiosa per un'organizzazione di piccole dimensioni con solo pochi controller di dominio per ogni dominio. Tutti i server di catalogo globale potranno essere ripristinati in una notte venerdì e, se necessario, completare la replica tutti dominio di sola partizioni prima di lunedì mattina. Ma se è necessario ripristinare un dominio di grandi dimensioni che copre siti in tutto il mondo, caso la partizione di dominio di sola lettura in tutti i cataloghi globali di riallocazione per gli altri domini possono in modo significativo le operazioni di impatto e richiedere tempi di inattività.  
  
## <a name="remove-lingering-objects"></a>Rimuovere oggetti residui  
 Simile al processo di ripristino della foresta, ripristinare un controller di dominio da un backup del dominio che è necessario ripristinare, eseguire la pulizia dei metadati di controller di dominio rimanenti e quindi installare nuovamente il dominio Active Directory per realizzare il dominio. Nel server di catalogo globale di tutti gli altri domini nella foresta, rimuovere gli oggetti residui per la partizione di sola lettura del dominio ripristinato.  
  
 L'origine per la pulizia dell'oggetto residui deve essere un controller di dominio nel dominio ripristinato. Per essere certi che il controller di dominio di origine non tutti gli oggetti residui per tutte le partizioni di dominio, è possibile rimuovere il catalogo globale, se è stato un catalogo globale.  
  
 Rimozione di oggetti residui risulta vantaggiosa per le organizzazioni più grandi che non possono rischiare i tempi di inattività associato le altre opzioni.  
  
 Per ulteriori informazioni, vedere [utilizzare Repadmin per rimuovere oggetti residui](https://technet.microsoft.com/library/cc785298.aspx).

## <a name="next-steps"></a>Passaggi successivi
-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
-   [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
