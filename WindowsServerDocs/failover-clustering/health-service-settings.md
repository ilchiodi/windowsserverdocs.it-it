---
title: Impostazioni Servizio integrità
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: a8262567abdd18847e99026c43d722351a00d3f2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720543"
---
# <a name="health-service-settings"></a>Impostazioni Servizio integrità

> Si applica a: Windows Server 2019, Windows Server 2016

Il Servizio integrità è una nuova funzionalità di Windows Server 2016 che migliora l'esperienza di monitoraggio e operatività quotidiana per i cluster che eseguono Spazi di archiviazione diretta.

Molti dei parametri che governano il comportamento del Servizio integrità vengono esposti come impostazioni. È possibile modificarli per ottimizzare l'aggressività di errori o azioni, attivare/disattivare determinati comportamenti e altro ancora.

Usare il cmdlet di PowerShell seguente per impostare o modificare le impostazioni.

### <a name="usage"></a>Uso

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name <SettingName> -Value <Value>  
```

#### <a name="example"></a>Esempio

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.Volume.CapacityThreshold.Warning" -Value 70
```

### <a name="common-settings"></a>Impostazioni comuni

Di seguito sono elencate alcune impostazioni modificate di frequente, insieme ai relativi valori predefiniti.

#### <a name="volume-capacity-threshold"></a>Soglia di capacità del volume

```
"System.Storage.Volume.CapacityThreshold.Enabled"  = True
"System.Storage.Volume.CapacityThreshold.Warning"  = 80
"System.Storage.Volume.CapacityThreshold.Critical" = 90
```

#### <a name="pool-reserve-capacity-threshold"></a>Soglia di capacità di riserva pool

```
"System.Storage.StoragePool.CheckPoolReserveCapacity.Enabled" = True
```

#### <a name="physical-disk-lifecycle"></a>Ciclo di vita del disco fisico

```
"System.Storage.PhysicalDisk.AutoPool.Enabled"                             = True
"System.Storage.PhysicalDisk.AutoRetire.OnLostCommunication.Enabled"       = True
"System.Storage.PhysicalDisk.AutoRetire.OnUnresponsive.Enabled"            = True
"System.Storage.PhysicalDisk.AutoRetire.DelayMs"                           = 900000 (i.e. 15 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountResetIntervalSeconds" = 360 (i.e. 60 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountAllowed"              = 3
```

#### <a name="supported-components-document"></a>Documento componenti supportati

Vedere la sezione precedente.

#### <a name="firmware-rollout"></a>Implementazione del firmware

```
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.SingleDrive.Enabled"       = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled"           = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelaySeconds"  = 604800 (i.e. 7 days)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.ShortDelaySeconds" = 86400 (i.e. 1 day)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelayCount"    = 1
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.FailureTolerance"  = 3
```

#### <a name="platform--quiescence"></a>Piattaforma/quiescenza

```
"Platform.Quiescence.MinDelaySeconds" = 120 (i.e. 2 minutes)
"Platform.Quiescence.MaxDelaySeconds" = 420 (i.e. 7 minutes)
```

#### <a name="metrics"></a>Metriche

```
"System.Reports.ReportingPeriodSeconds" = 1
```

#### <a name="debugging"></a>Debug

```
"System.LogLevel" = 4
```

## <a name="see-also"></a>Vedere anche

- [Servizio integrità in Windows Server 2016](health-service-overview.md)
- [Spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
