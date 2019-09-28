---
title: 'Ripristino della foresta di Active Directory: verifica della replica'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: f65508bf8973721a09c779a52f708d6a258e2cb5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390235"
---
# <a name="resources-to-verify-replication-is-working"></a>Risorse per verificare il funzionamento della replica 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Una volta ripristinati o reinstallati tutti i controller di dominio, è possibile verificare che servizi di dominio Active Directory e SYSVOL vengano ripristinati e replicati correttamente usando **Repadmin/Replsum.** , che viene eseguito in qualsiasi versione di Windows Server.  
  
> [!TIP]
> È anche possibile scaricare ed eseguire lo [strumento stato replica di Active Directory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), uno strumento gratuito che monitora lo stato della replica dei controller di dominio e segnala gli errori. ADReplStatus richiede .NET Framework 4, che verrà installato se non è già presente.  

Controllare l'accesso Replica DFS Visualizzatore eventi per l'ID evento 4602 (o ID evento del servizio Replica file 13516), che indica che SYSVOL è stato inizializzato.  

Se il primo controller di dominio recuperato registra l'ID evento 4614 ("il controller di dominio è in attesa di eseguire la replica iniziale. La cartella replicata rimarrà nello stato di sincronizzazione iniziale fino a quando non viene replicata con il partner ") nel registro Replica DFS, quindi l'ID evento 4602 non viene visualizzato ed è necessario eseguire i passaggi manuali seguenti per ripristinare SYSVOL se è replicato da DFSR  

1. Quando viene visualizzato l'evento DFSR 4612 nel primo controller di dominio ripristinato, eseguire un ripristino autorevole manuale come descritto in [2218556: Come forzare una sincronizzazione autorevole e non autorevole per SYSVOL con replica DFSR (come "D4/D2" per FRS) ](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
2. Impostare il **flag SysvolReady** su 1 manualmente, come descritto in [947022 la condivisione Netlogon non è presente dopo l'installazione di Active Directory Domain Services in un nuovo controller di dominio basato su Windows Server 2008 completo o in sola lettura](https://support.microsoft.com/kb/947022).  

È anche possibile creare un report di diagnostica Replica DFS. Per ulteriori informazioni, vedere la pagina relativa alla [creazione di un report di diagnostica per replica DFS](https://technet.microsoft.com/library/cc754227.aspx) e alla [Guida dettagliata a DFS per Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Se nel server è in esecuzione Windows Server 2008 R2, è possibile utilizzare l' [opzione della riga di comando Dfsrdiag. exe ReplicationState](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  

È inoltre possibile eseguire il test delle repliche utilizzando Dcdiag. exe per verificare la presenza di errori di replica. Per ulteriori informazioni, vedere l'articolo della Knowledge base [249256](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
