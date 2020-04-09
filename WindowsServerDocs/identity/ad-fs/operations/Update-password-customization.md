---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Aggiornare la personalizzazione di password
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cb5fd0ff432e441900e379d3fe798dbe6aef855f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816104"
---
# <a name="update-password-customization"></a>Aggiornare la personalizzazione di password 


In alcuni casi gli utenti potrebbero non riuscire a connettersi alla rete aziendale per modificare la password del proprio account. Questo fattore può essere un problema specialmente per i dipendenti remoti che risiedono lontano dall'ufficio aziendale più vicino. Per questi casi specifici è possibile usare la pagina di aggiornamento della password solo connettendosi a Internet.  
  
La pagina di aggiornamento della password può essere personalizzata fornendo una propria descrizione della pagina.  
  
> Per abilitare la pagina di aggiornamento della password, passare a Gestione ADFS in Endpoint. L'endpoint per l'aggiornamento della password si trova in basso, in Altro - /adfs/portal/updatepassword/. Dopo avere abilitato l'endpoint, è necessario riavviare il servizio ADFS. Questa operazione deve essere eseguita manualmente. È quindi possibile accedere a https://<fqdn>/adfs/portal/updatepassword/ su un dispositivo aggiunto all'area di lavoro. Dovrebbe essere visualizzata la pagina di aggiornamento della password.  
  
![update](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalizzare la descrizione della pagina di aggiornamento della password  
Per personalizzare la descrizione della pagina password aggiornamento, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Altri riferimenti 
[Personalizzazione dell'accesso utente AD FS](AD-FS-user-sign-in-customization.md)  
