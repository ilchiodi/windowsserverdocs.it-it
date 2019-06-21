---
title: Ripristino della foresta Active Directory - ripristino non autorevole
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: 65e33e6507d2affc4d07cc0780a7baf91a170a09
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280582"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Esecuzione di un ripristino non autorevole di servizi di dominio Active Directory 

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Per eseguire un ripristino non autorevole, completare la procedura seguente.  
  
Le procedure seguenti usano il Wbadmin.exe eseguire un ripristino non autorevole di Active Directory o Active Directory Domain Services (AD DS). Se si usa una soluzione di backup diversa o se si prevede di completare il ripristino autorevole di SYSVOL in un secondo momento nel processo di ripristino di foreste, è possibile eseguire un ripristino autorevole di SYSVOL con questi metodi alternativi:  
  
- Se si usa servizio Replica File (FRS) per replicare SYSVOL, seguire i passaggi descritti in [articolo 290762](https://go.microsoft.com/fwlink/?LinkId=148443) della Microsoft Knowledge Base, usando la **BurFlags** chiave del Registro di sistema per reinizializzare replica FRS Imposta o, se necessario, articolo 315457 [315457](https://support.microsoft.com/kb/315457)per ricompilare l'albero SYSVOL. Per determinare se è la replica di SYSVOL dal servizio Replica file, vedere [cartella SYSVOL di determinare se un Controller di dominio vengono replicata per il servizio FRS o DFSR](https://msdn.microsoft.com/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
- Se si usa la replica del File System distribuito (DFS) per replicare SYSVOL, vedere [eseguire una sincronizzazione autorevole di DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  

## <a name="performing-a-nonauthoritative-restore"></a>Esecuzione di un ripristino non autorevole

Usare la procedura seguente per eseguire un ripristino non autorevole di dominio Active Directory e un ripristino autorevole di SYSVOL nello stesso momento usando wbadmin.exe in un controller di dominio che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. Il backup deve includere in modo esplicito dati di stato del sistema; un backup completo del server che viene usato per il ripristino completo del server non funzionerà. Per altre informazioni sulla creazione di un backup dello stato del sistema, vedere [backup dei dati dello stato del sistema](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Per eseguire un ripristino non autorevole di dominio Active Directory autorevole e ripristino di SYSVOL con wbadmin.exe  
  
- Includere il **- authsysvol** passare nel comando di ripristino, come illustrato nell'esempio seguente:  

   ```  
   wbadmin start systemstaterecovery <otheroptions> -authsysvol  
   ```  

   Ad esempio:  

   ```  
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
   ```  
  
![Ripristinare](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
