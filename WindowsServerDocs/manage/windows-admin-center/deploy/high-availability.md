---
title: Distribuire l'interfaccia di amministrazione di Windows con disponibilità elevata
description: Distribuire l'interfaccia di amministrazione di Windows con disponibilità elevata (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406947"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Distribuire l'interfaccia di amministrazione di Windows con disponibilità elevata

>Si applica a: Windows Admin Center, Windows Admin Center Preview

È possibile distribuire l'interfaccia di amministrazione di Windows in un cluster di failover per garantire una disponibilità elevata per il servizio gateway dell'interfaccia di amministrazione di Windows. La soluzione fornita è una soluzione attiva-passiva, in cui è attiva una sola istanza di centro di amministrazione di Windows. Se si verifica un errore in uno dei nodi del cluster, l'interfaccia di amministrazione di Windows esegue correttamente il failover su un altro nodo, consentendo di continuare a gestire i server nell'ambiente in modo uniforme. 

[Informazioni sulle altre opzioni di distribuzione di Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Prerequisiti

- Un cluster di failover di due o più nodi in Windows Server 2016 o 2019. Ulteriori informazioni [sulla distribuzione di un cluster di failover](../../../failover-clustering/failover-clustering-overview.md).
- Un volume condiviso cluster per l'interfaccia di amministrazione di Windows per archiviare i dati persistenti a cui è possibile accedere da tutti i nodi del cluster. 10 GB saranno sufficienti per il volume condiviso cluster.
- Script di distribuzione a disponibilità elevata dal [file zip del centro di amministrazione di Windows](https://aka.ms/WACHAScript). Scaricare il file zip contenente lo script nel computer locale, quindi copiare lo script in base alle indicazioni riportate di seguito.
- Consigliato, ma facoltativo: un certificato firmato. pfx & password. Non è necessario avere già installato il certificato nei nodi del cluster. lo script eseguirà questa operazione. Se non ne viene fornito uno, lo script di installazione genera un certificato autofirmato che scade dopo 60 giorni.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Installare l'interfaccia di amministrazione di Windows in un cluster di failover

1. Copiare lo script ```Install-WindowsAdminCenterHA.ps1``` in un nodo del cluster. Scaricare o copiare il file con estensione msi di Windows Admin Center nello stesso nodo.
2. Connettersi al nodo tramite RDP ed eseguire lo script ```Install-WindowsAdminCenterHA.ps1``` da tale nodo con i parametri seguenti:
    - `-clusterStorage`: percorso locale della Volume condiviso cluster per archiviare i dati dell'interfaccia di amministrazione di Windows.
    - `-clientAccessPoint`: scegliere un nome che si userà per accedere all'interfaccia di amministrazione di Windows. Se ad esempio si esegue lo script con il parametro `-clientAccessPoint contosoWindowsAdminCenter`, sarà possibile accedere al servizio Windows Admin Center visitando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Facoltativo. Uno o più indirizzi statici per il servizio generico cluster. 
    - `-msiPath`: Percorso del file con estensione msi di Windows Admin Center.
    - `-certPath`: Facoltativo. Percorso di un file con estensione pfx del certificato.
    - `-certPassword`: Facoltativo. Una password SecureString per il certificato. pfx fornito in `-certPath`
    - `-generateSslCert`: Facoltativo. Se non si desidera fornire un certificato firmato, includere questo flag di parametro per generare un certificato autofirmato. Si noti che il certificato autofirmato scadrà tra 60 giorni.
    - `-portNumber`: Facoltativo. Se non si specifica una porta, il servizio gateway viene distribuito sulla porta 443 (HTTPS). Per usare una porta diversa, specificare in questo parametro. Si noti che se si usa una porta personalizzata, ad eccezione di 443, sarà possibile accedere all'interfaccia di amministrazione di Windows passando a https://\<clientAccessPoint @ no__t-1: \<Port @ no__t-3.

> [!NOTE]
> Lo script ```Install-WindowsAdminCenterHA.ps1``` supporta i parametri ```-WhatIf ``` e ```-Verbose```

### <a name="examples"></a>Esempi

#### <a name="install-with-a-signed-certificate"></a>Eseguire l'installazione con un certificato firmato:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Eseguire l'installazione con un certificato autofirmato:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Aggiornare un'installazione a disponibilità elevata esistente

Usare lo stesso script ```Install-WindowsAdminCenterHA.ps1``` per aggiornare la distribuzione a disponibilità elevata, senza perdere i dati di connessione.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Eseguire l'aggiornamento a una nuova versione dell'interfaccia di amministrazione di Windows

Quando viene rilasciata una nuova versione dell'interfaccia di amministrazione di Windows, è sufficiente eseguire di nuovo lo script ```Install-WindowsAdminCenterHA.ps1``` con solo il parametro ```msiPath```:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Aggiornare il certificato usato dall'interfaccia di amministrazione di Windows

È possibile aggiornare il certificato usato da una distribuzione a disponibilità elevata dell'interfaccia di amministrazione di Windows in qualsiasi momento fornendo il file PFX e la password del nuovo certificato.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

È anche possibile aggiornare il certificato nello stesso momento in cui si aggiorna la piattaforma Windows Admin Center con un nuovo file con estensione msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Disinstallare

Per disinstallare la distribuzione a disponibilità elevata di Windows Admin Center dal cluster di failover, passare il parametro ```-Uninstall``` allo script ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Risoluzione dei problemi

I log vengono salvati nella cartella temporanea del volume condiviso cluster (ad esempio, C:\ClusterStorage\Volume1\temp).