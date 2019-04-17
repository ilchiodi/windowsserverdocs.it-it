---
title: Ripristino - della foresta Active Directory verificare la replica
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adfs
ms.openlocfilehash: d85336a10e808755677a99777af8ecf5c07b05ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="resources-to-verify-replication-is-working"></a>Risorse per verificare il funzionamento della replica 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Dopo aver ripristinato o reinstallato tutti i controller di dominio, è possibile verificare che servizi di dominio Active Directory e SYSVOL siano ripristinati e la replica correttamente utilizzando **repadmin /replsum.**, che viene eseguita in qualsiasi versione di Windows Server.  
  
> [!TIP]
>  È anche possibile scaricare ed eseguire il [Active Directory replica lo stato strumento](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), uno strumento gratuito che consente di monitorare lo stato della replica del controller di dominio e segnala gli errori. ADReplStatus richiede .NET Framework 4, verrà installato se non è già presente.  
  
 Controllare il Registro di replica DFS nel Visualizzatore eventi per l'evento ID 4602 (o l'ID evento 13516 servizio Replica File), che indica che è stato inizializzato SYSVOL.  
  
 Se ripristinato il primo controller di dominio registra l'evento ID 4614 ("il controller di dominio è in attesa di eseguire la replica iniziale. La cartella replicata rimarrà in stato di sincronizzazione iniziale fino a quando non replicato con il partner") nel Registro di replica DFS, quindi l'ID evento 4602 non viene visualizzato ed è necessario eseguire la seguente procedura manuale per ripristinare SYSVOL se vengono replicati da replica DFS:  
  
1.  Quando viene visualizzato il primo controller di dominio ripristinato DFSR evento 4612 eseguire un ripristino autorevole manuale come descritto in [2218556: come forzare una sincronizzazione autorevole e non autorevole per SYSVOL con replica DFS (come "D4/D2" per il servizio Replica file)](https://support.microsoft.com/kb/2218556) (n. https://support.microsoft.com/kb/2218556).  
  
2.  Impostare **SysvolReady Flag** su 1 manualmente, come descritto in [947022 condivisione NETLOGON di non è presente dopo l'installazione di servizi di dominio Active Directory in un nuovo controller di dominio completo o di sola lettura basato su Windows Server 2008](https://support.microsoft.com/kb/947022).  
  
 È inoltre possibile creare un rapporto diagnostica di replica DFS. Per ulteriori informazioni, vedere [creare un rapporto di diagnostica per replica DFS](https://technet.microsoft.com/library/cc754227.aspx) e [DFS Step-by-Step Guida di Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Se il server è in esecuzione Windows Server 2008 R2, è possibile utilizzare [opzione della riga di comando dfsrdiag.exe ReplicationState](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  
  
 È anche possibile eseguire il test di repliche con dcdiag.exe per controllare gli errori di replica. Per ulteriori informazioni, vedere l'articolo della Knowledge Base [articolo 249256](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
