---
title: Personalizzare il titolo "Risorse di lavoro" di Servizi Desktop remoto usando PowerShell in Windows Server
description: Descrive come modificare il nome predefinito dell'area di lavoro in Windows Server.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: ea7e1505f705c87e58145e7a306ed355ab3acf7f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387047"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personalizzare il titolo "Risorse di lavoro" di Servizi Desktop remoto usando PowerShell in Windows Server

Usando Windows Server per accedere a RemoteApp o desktop tramite Accesso Web Desktop remoto o la nuova app Desktop remoto, potresti aver notato che l'area di lavoro è denominata "Risorse di lavoro" per impostazione predefinita.  Puoi modificare facilmente questo titolo usando i cmdlet di PowerShell.

Per modificare il titolo, apri una nuova finestra di PowerShell nel server Gestore connessione e importa il modulo RemoteDesktop con il comando seguente.

```powershell
    Import-Module RemoteDesktop
```

Usa quindi il comando Set-RDWorkspace per modificare il nome dell'area di lavoro.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Ad esempio, puoi usare il comando seguente per modificare il nome dell'area di lavoro in "Contoso RemoteApps":

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Se esegui più gestori di connessione in modalità a disponibilità elevata, devi eseguire il comando sul gestore attivo. Puoi usare questo comando:

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Per altre informazioni sul cmdlet Set-RDWorkspace, vedi le informazioni di riferimento in [Set-RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps).