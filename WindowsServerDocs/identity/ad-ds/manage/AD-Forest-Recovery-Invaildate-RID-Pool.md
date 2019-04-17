---
title: Ripristino della foresta Active Directory - invalidare il Pool di RID
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adfs
ms.openlocfilehash: cb024356ae5f872e93448d73ea54b271fe3fae4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Ripristino della foresta Active Directory - invalidare il pool RID corrente  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Utilizzare la procedura seguente per noi Windows PowerShell per invalidare il pool RID corrente in un controller di dominio. Windows PowerShell è abilitata per impostazione predefinita in Windows Server 2012 e Windows Server 2008 R2, ma non Windows Server 2008 in cui deve essere installato utilizzando **Aggiungi funzionalità**. Può essere [scaricato](https://www.microsoft.com/download/details.aspx?id=20020) per l'esecuzione in Windows Server 2003.  
  
 Per verificare il comando completato correttamente, cercare l'ID evento 16654 (l'origine è Directory-Services-SAM) nel Registro di sistema nel Visualizzatore eventi in Windows Server 2012. Versioni precedenti di Windows non registrare questo evento.  
  
> [!NOTE]
>  Dopo che si invalida il pool di RID, si riceverà un errore quando si tenta di creare entità di sicurezza (utente, computer o gruppo). Il tentativo di creare un oggetto attiva una richiesta per un nuovo pool di RID. Nuovo tentativo dell'operazione ha esito positivo poiché verrà allocato il nuovo pool di RID.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Per invalidare il pool RID corrente  
  
1.  Aprire una sessione di Windows PowerShell con privilegi elevata, eseguire il comando seguente e premere INVIO:  
  
    ```  
    $Domain = New-Object System.DirectoryServices.DirectoryEntry  
    $DomainSid = $Domain.objectSid  
    $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
    $RootDSE.UsePropertyCache = $false  
    $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
    $RootDSE.SetInfo()  
    ```  
  
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
