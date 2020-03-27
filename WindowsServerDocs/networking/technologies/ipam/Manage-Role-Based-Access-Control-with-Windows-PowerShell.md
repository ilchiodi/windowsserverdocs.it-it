---
title: Gestire il controllo degli accessi in base al ruolo con Windows PowerShell
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a5cd347b849948052f4f7caa7fa8a863808e8c26
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309531"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gestire il controllo degli accessi in base al ruolo con Windows PowerShell

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni su come usare Gestione indirizzi IP per gestire il controllo degli accessi in base al ruolo con Windows PowerShell.  
  
>[!NOTE]
>Per informazioni di riferimento sui comandi di Windows PowerShell per gestione indirizzi IP, vedere i [cmdlet di IpamServer in Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps).  
  
I nuovi comandi di gestione indirizzi IP di Windows PowerShell offrono la possibilità di recuperare e modificare gli ambiti di accesso degli oggetti DNS e DHCP. La tabella seguente illustra il comando corretto da usare per ogni oggetto di gestione indirizzi IP.  
  
|Oggetto IPAM|Comando|Descrizione|  
|---------------|-----------|---------------|  
|Server DNS|Get-IpamDnsServer|Questo cmdlet restituisce l'oggetto server DNS in gestione indirizzi IP|  
|Zona DNS|Get-IpamDnsZone|Questo cmdlet restituisce l'oggetto zona DNS in gestione indirizzi IP|  
|Record di risorse DNS|Get-IpamResourceRecord|Questo cmdlet restituisce l'oggetto record di risorse DNS in gestione indirizzi IP|  
|Server di trasmissione condizionale DNS|Get-IpamDnsConditionalForwarder|Questo cmdlet restituisce l'oggetto server di trasmissione condizionale DNS in gestione indirizzi IP|  
|Server DHCP|Get-IpamDhcpServer|Questo cmdlet restituisce l'oggetto server DHCP in gestione indirizzi IP|  
|Ambito esteso DHCP|Get-IpamDhcpSuperscope|Questo cmdlet restituisce l'oggetto ambito esteso DHCP in gestione indirizzi IP|  
|Ambito DHCP|Get-IpamDhcpScope|Questo cmdlet restituisce l'oggetto ambito DHCP in gestione indirizzi IP|  
  
Nell'esempio seguente di output del comando, il cmdlet `Get-IpamDnsZone` recupera la zona DNS **Dublin.contoso.com** .  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Impostazione degli ambiti di accesso sugli oggetti IPAM  
È possibile impostare gli ambiti di accesso sugli oggetti di gestione indirizzi IP usando il comando `Set-IpamAccessScope`. È possibile utilizzare questo comando per impostare l'ambito di accesso su un valore specifico per un oggetto o per fare in modo che gli oggetti ereditino l'ambito di accesso dagli oggetti padre. Di seguito sono riportati gli oggetti che è possibile configurare con questo comando.  
  
-   Ambito DHCP  
  
-   Server DHCP  
  
-   Ambito esteso DHCP  
  
-   Server di trasmissione condizionale DNS  
  
-   Record di risorse DNS  
  
-   Server DNS  
  
-   Zona DNS  
  
-   Blocco di indirizzi IP  
  
-   Intervallo di indirizzi IP  
  
-   Spazio degli indirizzi IP  
  
-   Subnet indirizzi IP  
  
Di seguito è riportata la sintassi per il comando `Set-IpamAccessScope`.  
  
```  
NAME  
    Set-IpamAccessScope  
  
SYNTAX  
    Set-IpamAccessScope [-IpamRange] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpSuperscope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpScope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsConditionalForwarder] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsResourceRecord] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsZone] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamAddressSpace] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamSubnet] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamBlock] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
```  
  
Nell'esempio seguente, l'ambito di accesso della zona DNS **Dublin.contoso.com** viene modificato da **Dublino** ad **Europa**.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
  
PS C:\Users\Administrator.CONTOSO> $a = Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
PS C:\Users\Administrator.CONTOSO> Set-IpamAccessScope -IpamDnsZone -InputObject $a -AccessScopePath \Global\Europe -PassThru  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Europe  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  


