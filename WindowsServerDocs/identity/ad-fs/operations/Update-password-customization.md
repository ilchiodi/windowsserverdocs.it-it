---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Aggiornare la personalizzazione di password
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e67c04c98a53f4f1db36e6586fa77bcf181a8d5a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188971"
---
# <a name="update-password-customization"></a>Aggiornare la personalizzazione di password 


In alcuni casi gli utenti potrebbero non riuscire a connettersi alla rete aziendale per modificare la password del proprio account. Questo fattore può essere un problema specialmente per i dipendenti remoti che risiedono lontano dall'ufficio aziendale più vicino. Per questi casi specifici è possibile usare la pagina di aggiornamento della password solo connettendosi a Internet.  
  
La pagina di aggiornamento della password può essere personalizzata fornendo una propria descrizione della pagina.  
  
> Per abilitare la pagina di aggiornamento della password, passare a Gestione ADFS in Endpoint. L'endpoint per l'aggiornamento della password si trova in basso, in Altro - /adfs/portal/updatepassword/. Dopo avere abilitato l'endpoint, è necessario riavviare il servizio ADFS. Questa operazione deve essere eseguita manualmente. È quindi possibile accedere a https://<fqdn>/adfs/portal/updatepassword/ su un dispositivo aggiunto all'area di lavoro. Dovrebbe essere visualizzata la pagina di aggiornamento della password.  
  
![aggiornamento](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalizzare la descrizione della pagina di aggiornamento della password  
Per personalizzare la descrizione della pagina password aggiornamento, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Altri riferimenti 
[AD FS Sign-in personalizzazione dell'utente](AD-FS-user-sign-in-customization.md)  
