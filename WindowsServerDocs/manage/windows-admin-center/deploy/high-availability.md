---
title: Distribuire Windows Admin Center con la disponibilità elevata
description: Distribuire Windows Admin Center con la disponibilità elevata (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861062"
---
# <a name="deploy-windows-admin-center-with-high-availability"></a>Distribuire Windows Admin Center con la disponibilità elevata

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

È possibile distribuire Windows Admin Center in un cluster di failover per garantire un'elevata disponibilità per il servizio gateway di Windows Admin Center. La soluzione fornita è una soluzione di tipo attivo / passivo, in cui solo un'istanza di Windows Admin Center è attiva. Se uno dei nodi del cluster non riesce, Windows Admin Center normalmente esegue il failover a un altro nodo, consentendo di continuare a gestire senza problemi i server nell'ambiente in uso. 

[Informazioni sulle altre opzioni di distribuzione Windows Admin Center.](../plan/installation-options.md)

## <a name="prerequisites"></a>Prerequisiti

- Un cluster di failover di 2 o più nodi in Windows Server 2016 o 2019. [Altre informazioni sulla distribuzione di un Cluster di Failover](../../../failover-clustering/failover-clustering-overview.md).
- Un volume condiviso cluster (CSV) per Windows Admin Center archiviare i dati persistenti che sono accessibili da tutti i nodi del cluster. 10 GB sarà sufficiente per il volume condiviso cluster.
- Script di distribuzione a disponibilità elevata dal [file con estensione zip di Windows Admin Center a disponibilità elevata dello Script](https://aka.ms/WACHAScript). Scaricare il file con estensione zip che contiene lo script nel computer locale e quindi copiare lo script in base alle necessità in base alle istruzioni seguenti.
- Scelta consigliata ma facoltativa: un certificato firmato con estensione pfx e password. Non è necessario avere già installato il certificato nei nodi del cluster, lo script eseguirà che per l'utente. Se non specificato, lo script di installazione genera un certificato autofirmato, che scade dopo 60 giorni.

## <a name="install-windows-admin-center-on-a-failover-cluster"></a>Installare Windows Admin Center in un cluster di failover

1. Copia il ```Install-WindowsAdminCenterHA.ps1``` script a un nodo nel cluster. Scaricare o copiare il file MSI di Windows Admin Center allo stesso nodo.
2. Connettersi al nodo tramite RDP ed eseguire il ```Install-WindowsAdminCenterHA.ps1``` script dal nodo con i parametri seguenti:
    - `-clusterStorage`: il percorso locale del Volume condiviso Cluster per archiviare i dati di Windows Admin Center.
    - `-clientAccessPoint`: scegliere un nome che si userà per accedere a Windows Admin Center. Ad esempio, se si esegue lo script con il parametro `-clientAccessPoint contosoWindowsAdminCenter`, si accederà al servizio Windows Admin Center, visitare il sito `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Facoltativo. Uno o più indirizzi statici per il servizio generico del cluster. 
    - `-msiPath`: Il percorso del file con estensione msi di Windows Admin Center.
    - `-certPath`: Facoltativo. Il percorso per un file con estensione pfx del certificato.
    - `-certPassword`: Facoltativo. Password per il certificato con estensione pfx fornito in SecureString `-certPath`
    - `-generateSslCert`: Facoltativo. Se non si desidera fornire un certificato firmato, includere questo flag di parametro per generare un certificato autofirmato. Si noti che il certificato autofirmato scadrà 60 giorni.
    - `-portNumber`: Facoltativo. Se non si specifica una porta, il servizio gateway viene distribuito sulla porta 443 (HTTPS). Per usare una porta diversa specificare questo parametro. Si noti che se si usa una porta personalizzata (tutt'altro oltre alla 443), si accederà il Windows Admin Center, passare a https://\<clientAccessPoint\>:\<porta\>.

> [!NOTE]
> Il ```Install-WindowsAdminCenterHA.ps1``` lo script supporta ```-WhatIf ``` e ```-Verbose``` parametri

### <a name="examples"></a>Esempi

#### <a name="install-with-a-signed-certificate"></a>Per installare un certificato firmato:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### <a name="install-with-a-self-signed-certificate"></a>Per installare un certificato autofirmato:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## <a name="update-an-existing-high-availability-installation"></a>Aggiornare un'installazione esistente di disponibilità elevata

Usare lo stesso ```Install-WindowsAdminCenterHA.ps1``` script di aggiornamento della distribuzione a disponibilità elevata, senza perdere i dati di connessione.

### <a name="update-to-a-new-version-of-windows-admin-center"></a>Aggiornamento a una nuova versione di Windows Admin Center

Quando viene rilasciata una nuova versione di Windows Admin Center, è sufficiente eseguire il ```Install-WindowsAdminCenterHA.ps1``` nuovamente script con solo il ```msiPath``` parametro:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### <a name="update-the-certificate-used-by-windows-admin-center"></a>Aggiornare il certificato utilizzato da Windows Admin Center

È possibile aggiornare il certificato usato da una distribuzione a disponibilità elevata di Windows Admin Center in qualsiasi momento, fornendo i file con estensione pfx del certificato nuovo ed e la password.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Puoi anche aggiornare il certificato nello stesso momento che si aggiorna la piattaforma Windows Admin Center con un nuovo file con estensione msi.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## <a name="uninstall"></a>Disinstallazione

Per disinstallare la distribuzione a disponibilità elevata di Windows Admin Center dal cluster di failover, passare il ```-Uninstall``` parametro per il ```Install-WindowsAdminCenterHA.ps1``` script.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## <a name="troubleshooting"></a>Risoluzione dei problemi

I log vengono salvati nella cartella temp del volume condiviso cluster (ad esempio, C:\ClusterStorage\Volume1\temp).