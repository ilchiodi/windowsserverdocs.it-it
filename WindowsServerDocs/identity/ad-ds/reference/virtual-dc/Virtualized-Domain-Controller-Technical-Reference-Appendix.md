---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Appendice della documentazione tecnica sui controller di dominio virtualizzati
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ee5a46781a61b8546fef113763c0d8ef9ca9f6cc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853984"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Appendice della documentazione tecnica sui controller di dominio virtualizzati

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vengono trattati gli argomenti seguenti:  
  
-   [Terminologia](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions. ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="terminology"></a><a name="BKMK_Terms"></a>Terminologia  
  
-   **Snapshot** : stato di una macchina virtuale in un determinato momento. Dipende dalla catena di snapshot precedenti acquisita, dall'hardware e dalla piattaforma di virtualizzazione.  
  
-   **Clone** : copia completa e separata di una macchina virtuale. Dipende dall'hardware virtuale (hypervisor).  
  
-   **Clone completo** : un clone completo è una copia indipendente di una macchina virtuale che non condivide risorse con la macchina virtuale padre dopo l'operazione di clonazione. Il funzionamento continuo di un clone completo è interamente separato dalla macchina virtuale padre.  
  
-   **Disco differenze** : copia di una macchina virtuale che condivide i dischi virtuali con la macchina virtuale padre in modo continuo. Questo in genere consente di mantenere lo spazio su disco e consente a più macchine virtuali di usare la stessa installazione software.  
  
-   **Copia**della macchina virtuale: una file System copia di tutti i file e le cartelle correlati di una macchina virtuale.  
  
-   **Copia del file VHD** : copia del disco rigido virtuale di una macchina virtuale  
  
-   **ID di generazione VM** : intero a 128 bit assegnato alla macchina virtuale dall'hypervisor. Questo ID viene archiviato in memoria e reimpostato ogni volta che viene applicato uno snapshot. La progettazione usa un meccanismo indipendente dall'hypervisor per l'emersione dell'ID di generazione VM nella macchina virtuale. L'implementazione di Hyper-V espone l'ID nella tabella ACPI della macchina virtuale.  
  
-   **Importazione/esportazione** : funzionalità Hyper-V che consente all'utente di salvare l'intera macchina virtuale (file VM, VHD e configurazione macchina). Consente quindi agli utenti di usare tale set di file per riportare il computer nello stesso computer della stessa VM (ripristino), in un computer diverso come la stessa VM (spostamento) o una nuova macchina virtuale (copia)  
  
## <a name="fixvdcpermissionsps1"></a><a name="BKMK_FixPDCPerms"></a>FixVDCPermissions. ps1  
  
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
  


