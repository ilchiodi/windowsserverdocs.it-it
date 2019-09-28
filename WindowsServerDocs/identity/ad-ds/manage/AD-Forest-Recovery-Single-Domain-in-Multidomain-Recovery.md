---
title: 'Ripristino della foresta di Active Directory: ripristino di un singolo dominio in una foresta con più domini'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: 2582ffacb169b59692c0510469131b58be09d5f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368928"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Ripristino della foresta di Active Directory: ripristino di un singolo dominio in una foresta con più domini

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

In alcuni casi è necessario recuperare un solo dominio all'interno di una foresta con più domini, anziché un ripristino completo della foresta. In questo argomento vengono illustrate le considerazioni per il ripristino di un singolo dominio e le possibili strategie per il ripristino.  
  
Un singolo ripristino del dominio presenta una sfida univoca per la ricompilazione dei server di catalogo globale (GC). Se, ad esempio, il primo controller di dominio per il dominio viene ripristinato da un backup creato una settimana prima, tutti gli altri cataloghi globali nella foresta avranno dati più aggiornati per quel dominio rispetto al controller di dominio ripristinato. Per ristabilire la coerenza dei dati GC, sono disponibili due opzioni:  
  
- Deallocare e quindi riospitare la partizione domini ripristinati da tutti i cataloghi globali nella foresta, ad eccezione di quelli nel dominio recuperato, nello stesso momento.  
- Seguire il processo di ripristino della foresta per ripristinare il dominio, quindi rimuovere gli oggetti residui da cataloghi globali in altri domini.  
  
Le sezioni seguenti forniscono considerazioni generali per ogni opzione. Il set completo di passaggi che è necessario eseguire per il ripristino può variare per ambienti Active Directory diversi.  
  
## <a name="rehost-all-gcs"></a>Riospitare tutti i cataloghi globali  

> [!WARNING]
> La password dell'account amministratore di dominio per tutti i domini deve essere pronta per l'uso in caso di problemi che impedisce l'accesso a un catalogo globale per l'accesso.  

La riallocazione di tutti i cataloghi globali può essere eseguita usando i comandi repadmin/Unhost e repadmin/rehost (parte di repadmin/experthelp). Si eseguiranno i comandi repadmin in ogni GC in ogni dominio non recuperato. È necessario assicurarsi che tutti i cataloghi globali non contengano più una copia del dominio recuperato. A tale scopo, è necessario prima di tutto deallocare la partizione di dominio da tutti i controller di dominio in tutti i domini ripristinati nessuno della foresta. Dopo che tutti i cataloghi globali non contengono più la partizione, è possibile riospitarla. Quando si rialloca, prendere in considerazione la struttura del sito e della replica della foresta, ad esempio, completare il riallocatore di un controller di dominio per sito prima di riallocare gli altri controller di dominio del sito.  
  
Questa opzione può risultare vantaggiosa per un'organizzazione di piccole dimensioni con pochi controller di dominio per ogni dominio. Tutti i cataloghi globali possono essere ricompilati in una notte di venerdì e, se necessario, completare la replica per tutte le partizioni di dominio di sola lettura prima del lunedì mattina. Tuttavia, se è necessario ripristinare un dominio di grandi dimensioni che copre i siti di tutto il mondo, la riallocazione della partizione di dominio di sola lettura in tutti i cataloghi globali per altri domini può influire in modo significativo sulle operazioni e potenzialmente richiedere tempi di inattività.  
  
## <a name="remove-lingering-objects"></a>Rimuovi oggetti persistenti

In modo analogo al processo di ripristino della foresta, viene ripristinato un controller di dominio dal backup nel dominio che è necessario ripristinare, viene eseguita la pulizia dei metadati rimanenti e quindi si reinstalla servizi di dominio Active Directory per compilare il dominio. Nei cataloghi globali di tutti gli altri domini della foresta, è possibile rimuovere gli oggetti residui per la partizione di sola lettura del dominio recuperato.  

L'origine per la pulizia dell'oggetto residuo deve essere un controller di dominio nel dominio recuperato. Per essere certi che il controller di dominio di origine non disponga di oggetti residui per le partizioni di dominio, è possibile rimuovere il catalogo globale se si tratta di un GC.  

La rimozione di oggetti residui è vantaggiosa per le organizzazioni di grandi dimensioni che non possono rischiare il tempo di inattività associato ad altre opzioni.  

Per ulteriori informazioni, vedere [utilizzare Repadmin per rimuovere gli oggetti](https://technet.microsoft.com/library/cc785298.aspx)residui.

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
