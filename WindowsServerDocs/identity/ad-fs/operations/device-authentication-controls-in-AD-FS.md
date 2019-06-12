---
title: Controlli di autenticazione dispositivo in AD FS
description: Questo documento descrive come abilitare l'autenticazione del dispositivo in AD FS per Windows Server 2016 e 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f52d3d237573e4ed0028e228ff80273862a0aaf2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444638"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controlli di autenticazione dispositivo in AD FS
Il documento seguente illustra come abilitare i controlli di autenticazione dispositivo in Windows Server 2016 e 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controlli di autenticazione dispositivo in AD FS 2012 R2
Originariamente in AD FS 2012 R2 era presente una proprietà di autenticazione globali chiamata `DeviceAuthenticationEnabled` che l'autenticazione dei dispositivi controllate.

Per configurare l'impostazione di `Set-AdfsGlobalAuthenticationPolicy` cmdlet è stato usato come illustrato di seguito:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Per disabilitare l'autenticazione del dispositivo, è stato usato lo stesso cmdlet per impostare il valore su $false false per.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controlli di autenticazione dispositivo in AD FS 2016
L'unico tipo di autenticazione dei dispositivi supportati in 2012 R2 era clientTLS.  In AD FS 2016, oltre a clientTLS esistono due nuovi tipi di autenticazione del dispositivo per l'autenticazione i dispositivi moderni.  Questi sono:
- PKeyAuth
- PRT

Per controllare il comportamento di nuovo, il `DeviceAuthenticationEnabled` proprietà viene utilizzata in combinazione con una nuova proprietà denominata `DeviceAuthenticationMethod`.  

Il metodo di autenticazione dispositivo determina il tipo di autenticazione del dispositivo che verrà completato: PRT PKeyAuth, clientTLS o una combinazione.
Include i valori seguenti:
 - SignedToken: Solo PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS
 - All: Tutti gli elementi elencati sopra

Come può notare, PRT fa parte di tutti i metodi di autenticazione dispositivo, rendendolo attivo il metodo predefinito che è sempre abilitata quando `DeviceAuthenticationEnabled` è impostata su `$true`.

Esempio: Per configurare i metodi, usare il cmdlet DeviceAuthenticationEnabled come sopra, insieme a proprietà nuove:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> In ad FS 2019, `DeviceAuthenticationMethod` può essere usato con il `Set-AdfsRelyingPartyTrust` comando.

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> Abilitare l'autenticazione del dispositivo (impostazione `DeviceAuthenticationEnabled` al `$true`) significa che il `DeviceAuthenticationMethod` in modo implicito è impostata su `SignedToken`, che equivale a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> Il metodo di autenticazione del dispositivo predefinita è `SignedToken`.  Altri valori sono **PKeyAuth,** <strong>ClientTLS,</strong> e **tutti**.

I significati del `DeviceAuthenticationMethod` valori sono stati modificati leggermente dopo il rilascio di AD FS 2016.  Vedere la tabella seguente per il significato di ogni valore, a seconda del livello di aggiornamento:


|Versione di AD FS|Valore DeviceAuthenticationMethod|Significa che|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Tutte|PRT + PkeyAuth clientTLS|
|2016 RTM + fino alla data con Windows Update|SignedToken (ovvero modificato)|PRT (solo)|
||PkeyAuth (nuovo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Tutte|PRT + PkeyAuth clientTLS|

## <a name="see-also"></a>Vedere anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
