---
title: Distribuire Windows Admin Center con disponibilità elevata
description: Distribuire Windows Admin Center con disponibilità elevata (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ae7bd9ed7aee5835ac1f53b9e10879ad8824f52
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406947"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Distribuire Windows Admin Center con disponibilità elevata

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Puoi distribuire Windows Admin Center in un cluster di failover per garantire disponibilità elevata per il servizio gateway di Windows Admin Center. La soluzione fornita è una soluzione attiva-passiva, in cui è attiva una sola istanza di Windows Admin Center. Se si verifica un errore in uno dei nodi del cluster, Windows Admin Center esegue normalmente il failover su un altro nodo, consentendoti di continuare a gestire i server nell'ambiente senza interruzioni. 

[Ottieni informazioni sulle altre opzioni di distribuzione di Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Prerequisiti

- Un cluster di failover di due o più nodi in Windows Server 2016 o 2019. [Ottieni altre informazioni sulla distribuzione di un cluster di failover](../../../failover-clustering/failover-clustering-overview.md).
- Un volume condiviso cluster per Windows Admin Center per archiviare i dati permanenti a cui è possibile accedere da tutti i nodi del cluster. 10 GB saranno sufficienti per il volume condiviso cluster.
- Script di distribuzione a disponibilità elevata disponibile nel [file zip dello script di disponibilità elevata di Windows Admin Center](https://aka.ms/WACHAScript). Scarica nel computer locale il file zip contenente lo script e quindi copia lo script in base alle tue esigenze seguendo le indicazioni riportate di seguito.
- Consigliato, ma facoltativo: un certificato firmato con estensione pfx e una password. Non è necessario che il certificato sia già installato nei nodi del cluster. Lo script eseguirà questa operazione. Se non fornisci un certificato firmato, lo script di installazione genera un certificato autofirmato che scade dopo 60 giorni.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Installare Windows Admin Center in un cluster di failover

1. Copia lo script ```Install-WindowsAdminCenterHA.ps1``` in un nodo del cluster. Scarica o copia il file con estensione msi di Windows Admin Center nello stesso nodo.
2. Connettiti al nodo tramite RDP ed esegui lo script ```Install-WindowsAdminCenterHA.ps1``` da tale nodo con i parametri seguenti:
    - `-clusterStorage`: percorso locale del volume condiviso cluster per archiviare i dati di Windows Admin Center.
    - `-clientAccessPoint`: scegli un nome che userai per accedere a Windows Admin Center. Se ad esempio esegui lo script con il parametro `-clientAccessPoint contosoWindowsAdminCenter`, potrai accedere al servizio Windows Admin Center visitando `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Facoltativo. Uno o più indirizzi statici per il servizio cluster generico. 
    - `-msiPath`: percorso del file msi di Windows Admin Center.
    - `-certPath`: Facoltativo. Percorso di un file pfx di certificato.
    - `-certPassword`: Facoltativo. Password SecureString per il file pfx di certificato fornito in `-certPath`
    - `-generateSslCert`: Facoltativo. Se non vuoi fornire un certificato firmato, includi questo flag di parametro per generare un certificato autofirmato. Tieni presente che il certificato autofirmato scadrà dopo 60 giorni.
    - `-portNumber`: Facoltativo. Se non specifichi una porta, il servizio gateway viene distribuito sulla porta 443 (HTTPS). Per usare una porta diversa, specifica questo parametro. Tieni presente che se usi una porta personalizzata (una qualsiasi ad eccezione della porta 443), potrai accedere a Windows Admin Center da https://\<puntoAccessoClient\>:\<porta\>.

> [!NOTE]
> Lo script ```Install-WindowsAdminCenterHA.ps1``` supporta i parametri ```-WhatIf ``` e ```-Verbose```

### <a name="examples"></a>Esempi

#### <a name="install-with-a-signed-certificate"></a>Installazione con un certificato firmato:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Installazione con un certificato autofirmato:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Aggiornare un'installazione a disponibilità elevata esistente

Usa lo stesso script ```Install-WindowsAdminCenterHA.ps1``` per aggiornare la distribuzione a disponibilità elevata, senza perdere i dati di connessione.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Eseguire l'aggiornamento a una nuova versione di Windows Admin Center

Quando viene rilasciata una nuova versione di Windows Admin Center, è sufficiente eseguire di nuovo lo script ```Install-WindowsAdminCenterHA.ps1``` con il parametro ```msiPath```:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Aggiornare il certificato usato da Windows Admin Center

Puoi aggiornare il certificato usato da una distribuzione a disponibilità elevata di Windows Admin Center in qualsiasi momento fornendo il file pfx e la password del nuovo certificato.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Puoi aggiornare il certificato anche mentre aggiorni la piattaforma Windows Admin Center con un nuovo file msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Disinstallazione

Per disinstallare dal cluster di failover la distribuzione a disponibilità elevata di Windows Admin Center, passa il parametro ```-Uninstall``` allo script ```Install-WindowsAdminCenterHA.ps1```.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Risoluzione dei problemi

I log vengono salvati nella cartella temporanea del volume condiviso cluster, ad esempio C:\ClusterStorage\Volume1\temp.