---
title: Configurare AD FS da escludere indirizzi IP
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8ff87a1043b589e83faa875467ddced536291b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867512"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS e gli indirizzi IP da escludere

>Si applica a: Windows Server 2016

In giugno 2018, ADFS in Windows Server 2016 ha introdotto **gli indirizzi IP da escludere** con AD FS aggiornamento di giugno 2018.  Questo aggiornamento consente di configurare un set di indirizzi IP a livello globale in AD FS, in modo che le richieste provenienti da tali indirizzi IP, o che dispongono di tali indirizzi IP **x-forwarded-for** o **x-ms-inoltrati--indirizzo ip del client** le intestazioni, verranno bloccate da AD FS.

## <a name="adding-banned-ips"></a>Aggiunta da escludere gli indirizzi IP
Per aggiungere gli indirizzi IP da escludere per l'elenco globale, usare il cmdlet di Powershell seguente:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formati consentiti

1.  IPv4
2.  IPv6
3.  Formato CIDR con IPv4 o v6

È previsto un limite di 300 voci per gli indirizzi IP da escludere. È possibile utilizzare formato CIDR o intervallo di negare un blocco di grandi dimensioni delle voci con una singola voce.

## <a name="removing-banned-ips"></a>Rimozione da escludere gli indirizzi IP
Per rimuovere gli indirizzi IP da escludere dall'elenco globale, usare il cmdlet di Powershell seguente:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Lettura da escludere gli indirizzi IP
Per leggere il set di indirizzi IP da escludere corrente, usare il cmdlet di Powershell seguente:

``` powershell
PS C:\ >Get-AdfsProperties 
```

Output di esempio:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Altri riferimenti  
[Le procedure consigliate per la protezione di Active Directory Federation Services](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
