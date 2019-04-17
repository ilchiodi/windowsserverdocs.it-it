---
title: Ripristino della foresta Active Directory - ripristino non autorevole
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adfs
ms.openlocfilehash: 684672491a0574b12e9b117a8e3c9c1f0936f357
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Eseguire un ripristino non autorevole di servizi di dominio Active Directory 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
 Per eseguire un ripristino non autorevole, completare la procedura seguente.  
  
 Le procedure seguenti usano le Wbadmin.exe per eseguire un ripristino non autorevole di Active Directory o servizi di dominio Active Directory (AD DS). Se si utilizza una soluzione di backup diversa o se si prevede di completare il ripristino autorevole di SYSVOL in un secondo momento nel processo di ripristino di foresta, è possibile eseguire un ripristino autorevole di SYSVOL usando questi metodi alternativi:  
  
-   Se si utilizza il servizio di replica File (FRS) per replicare SYSVOL, seguire i passaggi descritti in [articolo 290762](https://go.microsoft.com/fwlink/?LinkId=148443) della Microsoft Knowledge Base, utilizzando il **BurFlags** chiave del Registro di sistema per reinizializzare i set di repliche FRS o, se necessario, articolo 315457 [315457](https://support.microsoft.com/kb/315457)per ricostruire la struttura ad albero SYSVOL. Per determinare se è la replica di SYSVOL dal servizio Replica file, vedere [cartella SYSVOL di determinare se un Controller di dominio è replicato tramite replica DFS o replica file](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
  
-   Se si utilizza la replica del File System distribuito (DFS) per replicare SYSVOL, vedere [eseguire una sincronizzazione autorevole di SYSVOL con replica DFS](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
 
## <a name="performing-a-nonauthoritative-restore"></a>Eseguire un ripristino non autorevole  
 Utilizzare la procedura seguente per eseguire un ripristino non autorevole di dominio Active Directory e un ripristino autorevole di SYSVOL nello stesso momento utilizzando wbadmin.exe su un controller di dominio che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Il backup deve includere in modo esplicito i dati di stato del sistema; un backup completo del server che viene utilizzato per il ripristino completo del server non funzionerà. Per ulteriori informazioni sulla creazione di un backup dello stato del sistema, vedere [eseguire il backup dello stato del sistema](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Per eseguire un ripristino non autorevole di dominio Active Directory e il ripristino autorevole di SYSVOL utilizzando wbadmin.exe  
  
-   Includere il **- authsysvol** passare nel comando ripristino, come illustrato nell'esempio seguente:  
  
    ```  
    wbadmin start systemstaterecovery <otheroptions> -authsysvol  
    ```  
  
     Per esempio:  
  
    ```  
    wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
    ```  
  
 ![Ripristino](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
