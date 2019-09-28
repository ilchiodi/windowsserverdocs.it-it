---
title: Controlli di autenticazione del dispositivo in AD FS
description: Questo documento descrive come abilitare l'autenticazione del dispositivo in AD FS per Windows Server 2016 e 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 87c011b18ad4a1d464072c1ea90b09a44e831378
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407363"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controlli di autenticazione del dispositivo in AD FS
Il documento seguente illustra come abilitare i controlli di autenticazione dei dispositivi in Windows Server 2016 e 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controlli di autenticazione del dispositivo in AD FS 2012 R2
Originariamente in AD FS 2012 R2 era presente una proprietà di autenticazione globale denominata `DeviceAuthenticationEnabled` che controllava l'autenticazione del dispositivo.

Per configurare l'impostazione, è stato usato il cmdlet `Set-AdfsGlobalAuthenticationPolicy`, come illustrato di seguito:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Per disabilitare l'autenticazione del dispositivo, è stato usato lo stesso cmdlet per impostare il valore su $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controlli di autenticazione del dispositivo in AD FS 2016
L'unico tipo di autenticazione del dispositivo supportato in 2012 R2 è clientTLS.  In AD FS 2016, oltre a clientTLS sono disponibili due nuovi tipi di autenticazione del dispositivo per l'autenticazione dei dispositivi moderni.  Questi sono:
- PKeyAuth
- PRT

Per controllare il nuovo comportamento, la proprietà `DeviceAuthenticationEnabled` viene utilizzata insieme a una nuova proprietà denominata `DeviceAuthenticationMethod`.  

Il metodo di autenticazione del dispositivo determina il tipo di autenticazione del dispositivo che verrà eseguita: PRT, PKeyAuth, clientTLS o una combinazione.
Presenta i valori seguenti:
 - SignedToken: Solo PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS PRT + clientTLS
 - All: Tutte le precedenti

Come si può notare, PRT fa parte di tutti i metodi di autenticazione del dispositivo, rendendolo effettivo il metodo predefinito che è sempre abilitato quando `DeviceAuthenticationEnabled` è impostato su `$true`.

Esempio: Per configurare il metodo o i metodi, usare il cmdlet DeviceAuthenticationEnabled come sopra, insieme alla nuova proprietà:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> In ADFS 2019, `DeviceAuthenticationMethod` può essere utilizzato con il comando `Set-AdfsRelyingPartyTrust`.

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> L'abilitazione dell'autenticazione del dispositivo (impostando `DeviceAuthenticationEnabled` su `$true`) significa che il `DeviceAuthenticationMethod` viene impostato in modo implicito su `SignedToken`, che equivale a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> Il metodo di autenticazione del dispositivo predefinito è `SignedToken`.  Gli altri valori sono **PKeyAuth,** <strong>ClientTLS</strong> e **All**.

Il significato dei valori `DeviceAuthenticationMethod` è leggermente cambiato rispetto a quando è stato rilasciato AD FS 2016.  Vedere la tabella seguente per il significato di ogni valore, a seconda del livello di aggiornamento:


|Versione AD FS|Valore DeviceAuthenticationMethod|Significa|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Tutte|PRT + PkeyAuth + clientTLS|
|2016 RTM + aggiornato con Windows Update|SignedToken (significato modificato)|PRT (solo)|
||PkeyAuth (nuovo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Tutte|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Vedere anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
