---
title: Gestire il controllo degli accessi in base al ruolo con Windows PowerShell
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11dd417be4720b09851fc03f111edaaf06b55e59
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282127"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gestire il controllo degli accessi in base al ruolo con Windows PowerShell

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per imparare a usare Gestione indirizzi IP per gestire controllo degli accessi in base al ruolo con Windows PowerShell.  
  
>[!NOTE]
>Per i riferimenti ai comandi di gestione indirizzi IP Windows PowerShell, vedere la [IpamServer cmdlet in Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps).  
  
I nuovi comandi di gestione indirizzi IP di Windows PowerShell offrono la possibilità di recuperare e modificare gli ambiti di accesso di oggetti DNS e DHCP. Nella tabella seguente viene illustrato il comando corretto da utilizzare per ogni oggetto di gestione indirizzi IP.  
  
|Oggetto di gestione indirizzi IP|Comando|Descrizione|  
|---------------|-----------|---------------|  
|Server DNS|Get-IpamDnsServer|Questo cmdlet restituisce l'oggetto server DNS in Gestione indirizzi IP|  
|Zona DNS|Get-IpamDnsZone|Questo cmdlet restituisce l'oggetto di zona DNS in Gestione indirizzi IP|  
|Record di risorse DNS|Get-IpamResourceRecord|Questo cmdlet restituisce l'oggetto di record risorsa DNS in Gestione indirizzi IP|  
|Server d'inoltro condizionale DNS|Get-IpamDnsConditionalForwarder|Questo cmdlet restituisce l'oggetto server d'inoltro condizionale DNS in Gestione indirizzi IP|  
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
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Impostare gli ambiti di accesso su oggetti di gestione indirizzi IP  
È possibile impostare gli ambiti di accesso per gli oggetti di gestione indirizzi IP usando il `Set-IpamAccessScope` comando. È possibile usare questo comando per impostare l'ambito di accesso a un valore specifico per un oggetto o per fare in modo gli oggetti da cui ereditare ambito di accesso a oggetti padre. Di seguito sono gli oggetti che è possibile configurare con questo comando.  
  
-   Ambito DHCP  
  
-   Server DHCP  
  
-   Ambito esteso DHCP  
  
-   Server d'inoltro condizionale DNS  
  
-   Record risorsa DNS  
  
-   Server DNS  
  
-   Zona DNS  
  
-   Blocco di indirizzi IP  
  
-   Intervallo di indirizzi IP  
  
-   Spazio di indirizzi IP  
  
-   Subnet di indirizzi IP  
  
Seguito è riportata la sintassi per il `Set-IpamAccessScope` comando.  
  
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
  
Nell'esempio seguente, l'ambito di accesso della zona DNS **dublin.contoso.com** viene modificato da **Dublino** al **Europa**.  
  
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
  


