---
title: Gestire in base al ruolo controllo di accesso con Windows PowerShell
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df6fa423a4ec891f1ad3faefad6c6054519542c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gestire in base al ruolo controllo di accesso con Windows PowerShell

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come utilizzare Gestione indirizzi IP per gestire il controllo degli accessi in base al ruolo con Windows PowerShell.  
  
>[!NOTE]
>Per riferimento ai comandi di Windows PowerShell di gestione indirizzi IP, vedere [cmdlet di Server di gestione indirizzi IP (IPAM) in Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
I comandi di gestione indirizzi IP di Windows PowerShell nuovo offrono la possibilità di recuperare e modificare gli ambiti di accesso di oggetti DNS e DHCP. Nella tabella seguente illustra il comando corretto da utilizzare per ogni oggetto di gestione indirizzi IP.  
  
|Oggetto di gestione indirizzi IP|Comando|Descrizione|  
|---------------|-----------|---------------|  
|Server DNS|Get-IpamDnsServer|Questo cmdlet restituisce l'oggetto server DNS in Gestione indirizzi IP|  
|Zona DNS|Get-IpamDnsZone|Questo cmdlet restituisce l'oggetto di zona DNS in Gestione indirizzi IP|  
|Record di risorse DNS|Get-IpamResourceRecord|Questo cmdlet restituisce l'oggetto di record di risorse DNS in Gestione indirizzi IP|  
|Server d'inoltro condizionale DNS|Get-IpamDnsConditionalForwarder|Questo cmdlet restituisce l'oggetto server d'inoltro condizionali DNS in Gestione indirizzi IP|  
|Server DHCP|Get-IpamDhcpServer|Questo cmdlet restituisce l'oggetto server DHCP in Gestione indirizzi IP|  
|Ambito esteso DHCP|Get-IpamDhcpSuperscope|Questo cmdlet restituisce l'oggetto di ambito esteso DHCP in Gestione indirizzi IP|  
|Ambito DHCP|Get-IpamDhcpScope|Questo cmdlet restituisce l'oggetto di ambito DHCP in Gestione indirizzi IP|  
  
Nell'esempio seguente di output del comando, il `Get-IpamDnsZone` cmdlet recupera le **dublin.contoso.com** zona DNS.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Impostazione gli ambiti di accesso per gli oggetti di gestione indirizzi IP  
È possibile impostare gli ambiti di accesso per gli oggetti di gestione indirizzi IP utilizzando la `Set-IpamAccessScope` comando. È possibile utilizzare questo comando per impostare l'ambito di accesso su un valore specifico per un oggetto o per gli oggetti da cui ereditare ambito di accesso a oggetti padre. Di seguito sono gli oggetti che è possibile configurare con questo comando.  
  
-   Ambito DHCP  
  
-   Server DHCP  
  
-   Ambito esteso DHCP  
  
-   Server d'inoltro condizionale DNS  
  
-   Record di risorse DNS  
  
-   Server DNS  
  
-   Zona DNS  
  
-   Blocco di indirizzi IP  
  
-   Intervallo di indirizzi IP  
  
-   Spazio di indirizzi IP  
  
-   Subnet di indirizzi IP  
  
Ecco la sintassi per la `Set-IpamAccessScope` comando.  
  
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
  
Nell'esempio seguente, l'ambito di accesso della zona DNS **dublin.contoso.com** viene modificata da **Dublin** a **Europa**.  
  
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
  


