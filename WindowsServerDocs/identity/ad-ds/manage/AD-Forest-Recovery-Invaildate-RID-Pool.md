---
title: Ripristino della foresta di Active Directory-invalidamento del pool di RID
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 9e693f6f30fb721897eaaac89b3d146c57e0e63f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823924"
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
