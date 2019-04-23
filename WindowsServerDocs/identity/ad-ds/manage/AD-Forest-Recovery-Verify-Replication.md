---
title: Ripristino - della foresta Active Directory verifica della replica
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: fb05586f281460dc2c7a1afea4c0423493e3fc46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878312"
---
# <a name="resources-to-verify-replication-is-working"></a>Risorse per verificare il funzionamento della replica 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Dopo aver ripristinato o reinstallato tutti i controller di dominio, è possibile verificare che Active Directory Domain Services e SYSVOL siano ripristinati e la replica in modo corretto usando **repadmin /replsum.**, che viene eseguito in qualsiasi versione di Windows Server.  
  
> [!TIP]
> È anche possibile scaricare ed eseguire la [strumento di Active Directory replica lo stato](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), uno strumento gratuito che consente di monitorare lo stato di replica del controller di dominio e segnala gli errori. ADReplStatus richiede .NET Framework 4, che verrà installata se non è già presente.  

Controllare il log di replica DFS nel Visualizzatore eventi per eventi ID 4602 (o ID evento servizio Replica File 13516), che indica che è stato inizializzato SYSVOL.  

Se recuperare il primo controller di dominio registra l'evento ID 4614 ("il controller di dominio è in attesa di eseguire la replica iniziale. La cartella replicata rimarrà nello stato di sincronizzazione iniziale fino a quando non dispone di replica con il partner") nel log di replica DFS, quindi 4602 ID evento non viene visualizzata ed è necessario eseguire la procedura manuale seguente per ripristinare SYSVOL se replicato dal REPLICA DFS:  

1. Quando viene visualizzata 4612 eventi di replica DFS nel controller di dominio ripristinato prima eseguire un ripristino autorevole manuale come descritto in [2218556: Come forzare una sincronizzazione autorevole e non autorevole di DFSR-replicated SYSVOL (ad esempio, "D4/D2" per FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
2. Impostare **SysvolReady Flag** su 1 manualmente, come descritto in [947022 condivisione NETLOGON di non è presente dopo aver installato Servizi di dominio Active Directory in un nuovo controller di dominio completo o di sola lettura basato su Windows Server 2008 ](https://support.microsoft.com/kb/947022).  

È anche possibile creare un rapporto di diagnostica di replica DFS. Per altre informazioni, vedere [creare un Report di diagnostica per replica DFS](https://technet.microsoft.com/library/cc754227.aspx) e [Guida dettagliata DFS per Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Se il server è in esecuzione Windows Server 2008 R2, è possibile usare [opzione della riga di comando ReplicationState dfsrdiag.exe](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  

È anche possibile eseguire il test di repliche mediante dcdiag.exe verificare la presenza di errori di replica. Per altre informazioni, vedere l'articolo della Knowledge Base [articolo 249256](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
