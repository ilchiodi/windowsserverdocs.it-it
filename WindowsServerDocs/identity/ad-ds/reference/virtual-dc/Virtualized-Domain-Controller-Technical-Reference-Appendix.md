---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Appendice di documentazione tecnica su Controller di dominio virtualizzati
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e7f264a098b6f67d98c9aa47ec5794374b8920d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Appendice di documentazione tecnica su Controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento si applica:  
  
-   [Terminologia](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminologia  
  
-   **Snapshot** -lo stato di una macchina virtuale in un determinato momento. Sia dipendente nella catena di snapshot precedente, hardware e della piattaforma di virtualizzazione.  
  
-   **Clone** - il completamento e separare copia di una macchina virtuale. Sia dipendente l'hardware virtuale (hypervisor).  
  
-   **Completa Clone** -un clone completo è una copia di una macchina virtuale che non condivide alcun risorse con la macchina virtuale padre dopo l'operazione di clonazione indipendente. Operazione in corso di un clone completo è completamente separate dalla macchina virtuale padre.  
  
-   **Disco differenze** -una copia di una macchina virtuale che condivide i dischi virtuali con la macchina virtuale padre in modo continuo. Questo in genere lo spazio su disco consente di risparmiare spazio e consente a più macchine virtuali possono utilizzare l'installazione del software stesso.  
  
-   **Copia macchina virtuale**- il file system copia di tutti i file correlati e le cartelle di una macchina virtuale.  
  
-   **Copia dei File disco rigido virtuale** -una copia del disco rigido virtuale di una macchina virtuale  
  
-   **ID di generazione VM** - un intero a 128 bit assegnato alla macchina virtuale dall'hypervisor. Questo ID è archiviato nella memoria e reimpostato ogni volta che viene applicato uno snapshot. La progettazione utilizza un meccanismo indipendente dall'hypervisor per superfici l'ID di generazione macchina virtuale nella macchina virtuale. L'implementazione di Hyper-V espone l'ID della tabella ACPI della macchina virtuale.  
  
-   **Importazione/esportazione** -funzionalità A Hyper-V che consente all'utente di salvare l'intera macchina virtuale (VM file, disco rigido virtuale e la configurazione del computer). Consente agli utenti all'uso di tale set di file per portare il computer torna nello stesso computer della stessa macchina virtuale (ripristino), quindi in un altro computer della stessa macchina virtuale (spostamento) o una nuova macchina virtuale (copia)  
  
## <a name="BKMK_FixPDCPerms"></a>FixVDCPermissions.ps1  
  
```  
# Unsigned script, requires use of set-executionpolicy remotesigned -force  
# You must run the Windows PowerShell console as an elevated administrator  
  
# Load Active Directory Windows PowerShell Module and switch to AD DS drive  
import-module activedirectory  
cd ad:  
  
## Get Domain NC  
$domainNC = get-addomain  
  
## Get groups and obtain their SIDs   
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
  
$sid1 = (get-adgroup $dcgroup).sid  
  
## Get the DACL of the domain  
$acl = get-acl $domainNC  
  
## The following object specific ACE grants extended right 'Allow a DC to create a clone of itself' for the CDC group to the Domain NC  
## 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e is the schemaIDGuid for 'DS-Clone-Domain-Controller"  
  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
  
## Add the ACE in the ACL and set the ACL on the object   
  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
write-host "Done writing new VDC permissions."  
cd c:   
```  
  


