---
title: Sviluppo per Nano Server
description: Comunicazione remota di PowerShell e sessioni CIM
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 57079470-a1c1-4fdc-af15-1950d3381860
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: bc1930b681621d4d34c85414dbc2f97df257af20
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082272"
---
# <a name="developing-for-nano-server"></a>Sviluppo per Nano Server

>Si applica a: Windows Server 2016

> [!IMPORTANT]
> A partire da Windows Server, versione 1709, Nano Server sarà disponibile solo come [immagine del sistema operativo di base del contenitore](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Per scoprire ciò cosa implica, vedi [Modifiche a Nano Server](nano-in-semi-annual-channel.md). 

Questi argomenti illustrano importanti differenze relative a PowerShell in Nano Server e inoltre forniscono linee guida per lo sviluppo di cmdlet PowerShell da usare in Nano Server.

- [PowerShell in Nano Server](PowerShell-on-Nano-Server.md)
- [Sviluppo di cmdlet di PowerShell per Nano Server](Developing-PowerShell-Cmdlets-for-Nano-Server.md)

## <a name="using-windows-powershell-remoting"></a>Uso della comunicazione remota di Windows PowerShell  
Per gestire Nano Server con la comunicazione remota di Windows PowerShell, è necessario aggiungere l'indirizzo IP di Nano Server all'elenco dei computer di gestione degli host attendibili, aggiungere l'account in uso agli amministratori di Nano Server e abilitare CredSSP, se si prevede di usare tale funzionalità.  

 >[!NOTE]  
    > Se l'istanza di Nano Server di destinazione e il computer di gestione sono nella stessa foresta di Active Directory Domain Services (o in foreste con una relazione di trust), non è necessario aggiungere l'istanza di Nano Server all'elenco di host attendibili. È possibile connettersi a Nano Server usando il nome di dominio completo, ad esempio: PS C:\> Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)
  
  
Per aggiungere Nano Server all'elenco di host attendibili, eseguire questo comando a un prompt di Windows PowerShell con privilegi elevati:  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
Per avviare la sessione remota di Windows PowerShell, avviare una sessione di Windows PowerShell locale con privilegi elevati e quindi eseguire questi comandi:  
  
  
```  
$ip = "\<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
È ora possibile eseguire i comandi di Windows PowerShell in Nano Server come di consueto.  
  
> [!NOTE]  
> Non tutti i comandi di Windows PowerShell sono disponibili in questa versione di Nano Server. Per vedere quali sono disponibili, eseguire `Get-Command -CommandType Cmdlet`  
  
Arrestare la sessione remota con il comando `Exit-PSSession`  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>Uso di sessioni di Windows PowerShell CIM su WinRM  
È possibile usare sessioni CIM e istanze di Windows PowerShell per eseguire i comandi WMI tramite Gestione remota Windows (WinRM).  
  
Avviare la sessione CIM eseguendo questi comandi in un prompt di Windows PowerShell:  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
Con la sessione stabilita, è possibile eseguire vari comandi WMI, ad esempio:  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  