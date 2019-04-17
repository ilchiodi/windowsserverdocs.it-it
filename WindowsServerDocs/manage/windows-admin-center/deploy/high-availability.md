---
title: Distribuire Windows Admin Center con una disponibilità elevata
description: Distribuire Windows Admin Center con una disponibilità elevata (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a0062230dd3d9e9c52aa317f87e06b0e84507dc4
ms.sourcegitcommit: 802a7bd537cab22893abb7e6657c4be90346ef88
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2019
ms.locfileid: "9025033"
---
# Distribuire Windows Admin Center con una disponibilità elevata

>Si applica a: Windows Admin Center, Windows Admin Center Preview

È possibile distribuire Windows Admin Center in un cluster di failover per fornire una disponibilità elevata per il servizio gateway Windows Admin Center. La soluzione fornita è una soluzione attivi passivo, in cui è attiva solo un'istanza di Windows Admin Center. Se uno dei nodi del cluster non riesce, Windows Admin Center normalmente failover in un altro nodo, consentendoti di continuare a gestire con facilità i server nel tuo ambiente. 

[Informazioni sulle altre opzioni di distribuzione di Windows Admin Center.](../plan/installation-options.md)

## Prerequisiti

- Un cluster di failover di 2 o più nodi in Windows Server 2016 o 2019. [Ulteriori informazioni sulla distribuzione di un Cluster di Failover](../../../failover-clustering/failover-clustering-overview.md).
- Un cluster shared volume (CSV) per Windows Admin Center archiviare i dati permanenti che sono accessibili da tutti i nodi del cluster. 10 GB sarà sufficiente per la tua CSV.
- Script di distribuzione a disponibilità elevata da [file zip Windows Admin Center HA Script](https://aka.ms/WACHAScript). Scarica il file con estensione zip che contiene lo script nel computer locale e quindi copiare lo script in base alle esigenze in base alle indicazioni seguenti.
- Consigliata, ma facoltativo: una password di certificato PFX &. Non è necessario avere già installato il certificato nei nodi del cluster: lo script farà che per te. Se non viene fornita una, lo script di installazione genera un certificato autofirmato, che scade dopo 60 giorni.

## Installare Windows Admin Center in un cluster di failover

1. Copia il ```Install-WindowsAdminCenterHA.ps1``` script a un nodo del cluster. Scaricare o copiare il file MSI Windows Admin Center allo stesso nodo.
2. Connettersi al nodo tramite RDP ed eseguire il ```Install-WindowsAdminCenterHA.ps1``` script dal nodo con i parametri seguenti:
    - `-clusterStorage`: percorso locale del Volume condiviso Cluster per archiviare i dati di Windows Admin Center.
    - `-clientAccessPoint`: Scegli un nome che userai per accedere a Windows Admin Center. Ad esempio, se si esegue lo script con il parametro `-clientAccessPoint contosoWindowsAdminCenter`, accederanno al servizio Windows Admin Center, visitando la pagina `https://contosoWindowsAdminCenter.<domain>.com`
    - `-staticAddress`: Facoltativo. Uno o più indirizzi statici per il servizio generico di cluster. 
    - `-msiPath`: Il percorso per il file MSI di Windows Admin Center.
    - `-certPath`: Facoltativo. Il percorso di un file di certificato PFX.
    - `-certPassword`: Facoltativo. Una password SecureString per PFX il certificato fornito in `-certPath`
    - `-generateSslCert`: Facoltativo. Se non vuoi fornire un certificato, Includi questo flag di parametro per generare un certificato autofirmato. Tieni presente che il certificato autofirmato scadrà entro 60 giorni.
    - `-portNumber`: Facoltativo. Se non specifichi una porta, il servizio gateway viene distribuito sulla porta 443 (HTTPS). Per usare una porta diversa specifica in questo parametro. Tieni presente che se si utilizza una porta personalizzata (alcuna oltre 443), è possibile accedere accedendo a https://\<clientAccessPoint\>:\<port\> Windows Admin Center.

> [!NOTE]
> Il ```Install-WindowsAdminCenterHA.ps1``` script supporta ```-WhatIf ``` e ```-Verbose``` parametri

### Esempi

#### Installa con un certificato firmato:

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

#### Installa con un certificato autofirmato:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -clusterStorage "C:\ClusterStorage\Volume1" -clientAccessPoint "contoso-ha-gateway" -msiPath ".\WindowsAdminCenter.msi" -generateSslCert -Verbose
```

## Aggiornare un'installazione esistente di una disponibilità elevata

Usare lo stesso ```Install-WindowsAdminCenterHA.ps1``` script per aggiornare la distribuzione, la disponibilità elevata senza perdere i dati di connessione.

### Aggiornamento a una nuova versione di Windows Admin Center

Quando viene rilasciata una nuova versione di Windows Admin Center, Esegui semplicemente il ```Install-WindowsAdminCenterHA.ps1``` script di nuovo con solo il ```msiPath``` parametro:

```powershell
.\Install-WindowsAdminCenterHA.ps1 -msiPath '.\WindowsAdminCenter.msi' -Verbose
```

### Aggiornare il certificato utilizzato da Windows Admin Center

È possibile aggiornare il certificato utilizzato per una disponibilità elevata distribuzione di Windows Admin Center in qualsiasi momento, fornendo file con estensione pfx del nuovo certificato ed e la password.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -certPath "cert.pfx" -certPassword $certPassword -Verbose
```

Si può anche aggiornare il certificato nello stesso momento che aggiornare la piattaforma Windows Admin Center con un nuovo file MSI.

```powershell
$certPassword = Read-Host -AsSecureString
.\Install-WindowsAdminCenterHA.ps1 -msiPath ".\WindowsAdminCenter.msi" -certPath "cert.pfx" -certPassword $certPassword -Verbose
``` 

## Disinstallare

Per disinstallare la disponibilità elevata distribuzione di Windows Admin Center dal cluster di failover, passare il ```-Uninstall``` parametro per la ```Install-WindowsAdminCenterHA.ps1``` script.

```powershell
.\Install-WindowsAdminCenterHA.ps1 -Uninstall -Verbose
```

## Risoluzione dei problemi

I log vengono salvati nella cartella temporanea del file CSV (ad esempio, C:\ClusterStorage\Volume1\temp).