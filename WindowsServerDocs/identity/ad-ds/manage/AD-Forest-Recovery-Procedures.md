---
title: Ripristino della foresta Active Directory - procedure
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adfs
ms.openlocfilehash: 91e3954c05fe3cd12d35b5db91afd29fb3a31e00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/13/2017
---
# <a name="ad-forest-recovery---procedures"></a>Ripristino della foresta Active Directory - procedure


>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

This section contains procedures related to the forest recovery process. The procedures are applicable for Windows Server 2016, 2012 R2, 2012 and are also applicable to Windows Server 2008 R2 and 2008 with some minor exceptions. 

Procedures that include steps that vary for Windows Server 2003 are found in [Forest Recovery with Windows Server 2003 Domain Controllers](AD-Forest-Recovery-Windows-Server-2003.md).  

The following is a list of procedures that are used in backing up and restoring domain controllers and Active Directory.
  
-   [Backing up a full server](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
-   [Eseguire il backup dello stato del sistema](AD-Forest-Recovery-Backing-up-System-State.md)  
-   [Performing a full server recovery](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
-   [Performing an authoritative synch of DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
-   [Eseguire un ripristino non autorevole di servizi di dominio Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md)  
  
     These steps explain how to perform an authoritative restore of SYSVOL at the same time.  
-   [Configuring the DNS Server service](AD-Forest-Recovery-Configure-DNS.md)  
-   [Removing the global catalog](AD-Forest-Recovery-Remove-GC.md)  
-   [Raising the value of available RID pools](AD-Forest-Recovery-Raise-RID-Pool.md)  
-   [Invalidating the current RID pool](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
-   [Seizing an operations master role](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
-   [Cleaning up after a restore](AD-Forest-Recovery-Cleanup.md)
-   [Cleaning metadata of removed writable domain controllers](AD-Forest-Recovery-Cleaning-Metadata.md)  
-   [Resetting the computer account password of the domain controller](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
-   [Resetting the krbtgt password](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
-   [Reimpostare una password di trust su un lato della relazione di trust](AD-Forest-Recovery-Reset-Trust.md)  
-   [Adding the global catalog](AD-Forest-Recovery-Add-GC.md)  
-   [Risorse per verificare il funzionamento della replica](AD-Forest-Recovery-Verify-Replication.md)  
  
  
## <a name="next-steps"></a>Passaggi successivi
-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
