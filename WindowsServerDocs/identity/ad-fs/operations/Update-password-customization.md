---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Aggiornare la personalizzazione di password
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 78d8839ccafa9530784cb9e38c3127ed2c215423
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2018
---
# <a name="update-password-customization"></a>Aggiornare la personalizzazione di password 

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In alcuni casi, gli utenti potrebbero non essere in grado di connettersi alla rete aziendale per modificare la password dell'account. Questo fattore può essere un problema specialmente per i dipendenti remoti che risiedono lontano dal più vicino sede. Per questi casi specifici, è possibile utilizzare la pagina di aggiornamento della password solo connettendosi a Internet.  
  
È possibile personalizzare la pagina password aggiornamento, fornendo una propria descrizione della pagina.  
  
> Per abilitare la pagina di aggiornamento della password, Vai alla gestione di ADFS in endpoint. L'endpoint per l'aggiornamento della password si trova nella parte inferiore in altro - / adfs//ADFS/Portal/UpdatePassword/. Dopo aver abilitato l'endpoint, è necessario riavviare il servizio ADFS. Questa operazione deve essere eseguita manualmente. È quindi possibile accedere con https://<fqdn>/adfs//ADFS/Portal/UpdatePassword/ su una rete aziendale appartenenti a un dispositivo, viene visualizzata la pagina di aggiornamento della password.  
  
![aggiornamento](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalizzare la descrizione della pagina di aggiornamento della Password  
Per personalizzare la descrizione della pagina password aggiornamento, utilizzare la sintassi e i cmdlet Windows PowerShell seguente.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Riferimenti aggiuntivi 
[AD FS Sign-personalizzazione utente](AD-FS-user-sign-in-customization.md)  
