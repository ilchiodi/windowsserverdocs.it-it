---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Appendice della documentazione tecnica sui controller di dominio virtualizzati
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9e3a5cc2c71455bb040f1311bdbfed1ac7e213fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832232"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Appendice della documentazione tecnica sui controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vengono trattati gli argomenti seguenti:  
  
-   [Terminologia](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminologia  
  
-   **Snapshot** -lo stato di una macchina virtuale in un determinato momento. È dipendente la catena di snapshot precedente, nell'hardware e la piattaforma di virtualizzazione.  
  
-   **Clone** - il completamento e separare copia di una macchina virtuale. Cui dipende l'hardware virtuale (hypervisor).  
  
-   **Clone completo** -un clone completo è una copia indipendente di una macchina virtuale che non condivide alcuna risorsa con la macchina virtuale padre dopo l'operazione di clonazione. Operazione in corso di un clone completo è completamente separato dalla macchina virtuale padre.  
  
-   **Disco differenze** -una copia di una macchina virtuale che condivide i dischi virtuali con la macchina virtuale padre in modo continuo. Questo in genere consente di conservare lo spazio su disco e consente più macchine virtuali da usare l'installazione del software stesso.  
  
-   **Copia macchina virtuale**: un file system copia di tutti i file correlati e le cartelle di una macchina virtuale.  
  
-   **Copia dei File di disco rigido virtuale** -una copia del disco rigido virtuale della macchina virtuale  
  
-   **ID di generazione VM** : un intero a 128 bit assegnato alla macchina virtuale dall'hypervisor. Questo ID viene archiviato in memoria e reimpostato ogni volta che viene applicato uno snapshot. La progettazione Usa un meccanismo indipendente dal hypervisor per esporre l'ID di generazione macchina virtuale nella macchina virtuale. L'implementazione di Hyper-V espone l'ID della tabella ACPI della macchina virtuale.  
  
-   **Importazione/esportazione** -funzionalità A Hyper-V che consente all'utente di salvare l'intera macchina virtuale (VM file, disco rigido virtuale e la configurazione della macchina). Consente agli utenti all'uso di tale set di file per portare il computer nuovamente nello stesso computer della VM stessa (ripristino), quindi in un computer diverso nella stessa macchina virtuale (spostare) o una nuova macchina virtuale (copia)  
  
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
  


