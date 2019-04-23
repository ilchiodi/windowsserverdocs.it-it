---
title: Ripristino della foresta Active Directory - invalidando il Pool di RID
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 46115991e48da301a8a739009bac27415ebe73df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842512"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Ripristino della foresta Active Directory - invalidando il pool RID corrente  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Usare la procedura seguente per Microsoft Windows PowerShell per invalidare il pool RID corrente in un controller di dominio. Windows PowerShell sia abilitato per impostazione predefinita in Windows Server 2012 e Windows Server 2008 R2, ma non Windows Server 2008 in cui deve essere installato usando **Aggiungi funzionalità**. Può essere [scaricati](https://www.microsoft.com/download/details.aspx?id=20020) per l'esecuzione in Windows Server 2003.  

Per verificare il comando completato correttamente, verificare la presenza di ID evento 16654 (l'origine è Directory-Services-SAM) nel Registro di sistema nel Visualizzatore eventi in Windows Server 2012. Le versioni precedenti di Windows non registrare questo evento.  
  
> [!NOTE]
> Dopo che si invalida il pool di RID, si riceverà un errore quando si tenta innanzitutto di creare entità di sicurezza (utente, computer o gruppo). Il tentativo di creare un oggetto attiva una richiesta di un nuovo pool di RID. Ripetizione dei tentativi dell'operazione ha esito positivo perché il nuovo pool di RID verrà allocato.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Per invalidare il pool RID corrente  
  
- Aprire una sessione di Windows PowerShell con privilegi elevata, eseguire il comando seguente e premere INVIO:  

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry  
   $DomainSid = $Domain.objectSid  
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
   $RootDSE.UsePropertyCache = $false  
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
   $RootDSE.SetInfo()  
   ```  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
