---
title: Configurare AD FS IP vietato indirizzi
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2b518f92f80d06e4bd0854fde94013a412aae515
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407713"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>Indirizzi IP AD FS e bannato


Nel 2018 giugno AD FS in Windows Server 2016 ha introdotto **indirizzi IP vietati** con l'aggiornamento AD FS giugno 2018.  Questo aggiornamento consente di configurare un set di indirizzi IP a livello globale in AD FS, in modo che le richieste provenienti da tali indirizzi IP o che abbiano gli indirizzi IP nelle intestazioni **x-inoltred-for** o **x-ms-inoltred-client-IP** vengano bloccate da ad FS.

## <a name="adding-banned-ips"></a>Aggiunta di indirizzi IP esclusi
Per aggiungere indirizzi IP esclusi all'elenco globale, usare il cmdlet PowerShell seguente:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formati consentiti

1.  IPv4
2.  IPv6
3.  Formato CIDR con IPv4 o V6

È previsto un limite di 300 voci per gli indirizzi IP esclusi. È possibile utilizzare il formato CIDR o range per negare un grande blocco di voci con una singola voce.

## <a name="removing-banned-ips"></a>Rimozione degli indirizzi IP esclusi
Per rimuovere gli indirizzi IP esclusi dall'elenco globale, usare il cmdlet PowerShell seguente:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Leggi indirizzi IP esclusi
Per leggere il set corrente di indirizzi IP esclusi, usare il cmdlet PowerShell seguente:

``` powershell
PS C:\ >Get-AdfsProperties 
```

Output di esempio:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Altri riferimenti  
[Procedure consigliate per la protezione di Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
