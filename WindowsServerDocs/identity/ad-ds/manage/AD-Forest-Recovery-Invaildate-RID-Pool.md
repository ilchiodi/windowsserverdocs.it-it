---
title: Ripristino della foresta di Active Directory-invalidamento del pool di RID
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: c3c477e21a455e5e5777da00b064ca7a02672571
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390407"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Ripristino della foresta di Active Directory-invalidamento del pool di RID corrente  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Usare la procedura seguente per Microsoft PowerShell per invalidare il pool RID corrente in un controller di dominio. Windows PowerShell è abilitato per impostazione predefinita in Windows Server 2012 e Windows Server 2008 R2, ma non in Windows Server 2008, in cui deve essere installato tramite l' **aggiunta di funzionalità**. Può essere [scaricata](https://www.microsoft.com/download/details.aspx?id=20020) per l'esecuzione in Windows Server 2003.  

Per verificare che il comando sia stato completato correttamente, cercare l'ID evento 16654 (source is Directory-Services-SAM) nel registro di sistema Visualizzatore eventi in Windows Server 2012. Le versioni precedenti di Windows non registrano questo evento.  
  
> [!NOTE]
> Dopo aver invalidato il pool di RID, verrà visualizzato un errore quando si tenta di creare l'entità di sicurezza (utente, computer o gruppo). Il tentativo di creare un oggetto attiva una richiesta per un nuovo pool di RID. Il tentativo di esecuzione dell'operazione riesce perché verrà allocato il nuovo pool di RID.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Per invalidare il pool RID corrente  
  
- Aprire una sessione di Windows PowerShell con privilegi elevati, eseguire il comando seguente e premere INVIO:  

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry  
   $DomainSid = $Domain.objectSid  
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
   $RootDSE.UsePropertyCache = $false  
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
   $RootDSE.SetInfo()  
   ```  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
