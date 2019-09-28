---
title: Ripristino della foresta di Active Directory-ripristino non autorevole
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: d7792cd739931d758125c8946606beb043ce19dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369087"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Esecuzione di un ripristino non autorevole di Active Directory Domain Services 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Per eseguire un ripristino non autorevole, completare la procedura riportata di seguito.  
  
Nelle procedure riportate di seguito viene utilizzato il comando Wbadmin. exe per eseguire un ripristino non autorevole di Active Directory o Active Directory Domain Services (AD DS). Se si usa una soluzione di backup diversa o si intende completare il ripristino autorevole di SYSVOL in un secondo momento nel processo di ripristino della foresta, è possibile eseguire un ripristino autorevole di SYSVOL usando questi metodi alternativi:  
  
- Se si usa il servizio Replica file per replicare SYSVOL, attenersi alla procedura descritta nell' [articolo 290762](https://go.microsoft.com/fwlink/?LinkId=148443) della Microsoft Knowledge base, usando la chiave del registro di sistema **BurFlags** per reinizializzare i set di repliche FRS o, se necessario, l'articolo 315457 [ 315457](https://support.microsoft.com/kb/315457)per ricompilare l'albero SYSVOL. Per determinare se SYSVOL è replicato da FRS, vedere [determinare se la cartella SYSVOL di un controller di dominio viene replicata da DFSR o FRS](https://msdn.microsoft.com/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
- Se si usa la replica file system distribuito (DFS) per replicare SYSVOL, vedere [eseguire una sincronizzazione autorevole di SYSVOL con replica DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  

## <a name="performing-a-nonauthoritative-restore"></a>Esecuzione di un ripristino non autorevole

Utilizzare la procedura seguente per eseguire un ripristino non autorevole di servizi di dominio Active Directory e un ripristino autorevole di SYSVOL contemporaneamente utilizzando WBADMIN. exe in un controller di dominio che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Il backup deve includere in modo esplicito i dati sullo stato del sistema; un backup completo del server utilizzato per il ripristino completo del server non funzionerà. Per ulteriori informazioni sulla creazione di un backup dello stato del sistema, vedere backup [dei dati sullo stato del sistema](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Per eseguire un ripristino non autorevole di servizi di dominio Active Directory e ripristino autorevole di SYSVOL utilizzando WBADMIN. exe  
  
- Includere l'opzione **-authsysvol** nel comando di ripristino, come illustrato nell'esempio seguente:  

   ```  
   wbadmin start systemstaterecovery <otheroptions> -authsysvol  
   ```  

   Esempio:  

   ```  
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
   ```  
  
![Ripristinare](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
