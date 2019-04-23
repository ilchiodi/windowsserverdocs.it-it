---
title: Personalizzare il titolo "Risorse di lavoro" di Servizi Desktop remoto usando PowerShell in Windows Server
description: Fornisce la descrizione di come modificare il nome dell'area di lavoro rispetto all'impostazione predefinita in Windows Server.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826712"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personalizzare il titolo "Risorse di lavoro" di Servizi Desktop remoto usando PowerShell in Windows Server

Quando si usa Windows Server per accedere a RemoteApp o desktop tramite accesso Web desktop remoto o la nuova app di Desktop remoto, si è notato che l'area di lavoro è denominato "Risorse di lavoro" per impostazione predefinita.  È possibile modificare facilmente il titolo usando i cmdlet di PowerShell.

Per modificare il titolo, aprire una nuova finestra di PowerShell nel server Gestore connessione e importare il modulo RemoteDesktop con il comando seguente.

```powershell
    Import-Module RemoteDesktop
```

Successivamente, usare il comando Set-RDWorkspace per modificare il nome dell'area di lavoro.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Ad esempio, è possibile usare il comando seguente per modificare il nome area di lavoro in "Contoso RemoteApps":

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Se si eseguono più gestori di connessione in modalità a disponibilità elevata, è necessario eseguire Service broker attivo. È possibile usare questo comando:

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Per altre informazioni sul cmdlet Set-RDWorkspace, vedere la [Set-RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps) riferimento.