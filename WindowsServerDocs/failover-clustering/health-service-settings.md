---
title: "Impostazioni del servizio integrità"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 569cf7ba30fd3f993394efd3735a56d116c067e0
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-settings"></a>Impostazioni del servizio integrità
> Si applica a Windows Server 2016

Il servizio integrità è una nuova funzionalità di Windows Server 2016 che consente di migliorare il monitoraggio e l'esperienza per i cluster che eseguono spazi di archiviazione diretta.

Molti dei parametri che controllano il comportamento del servizio integrità vengono esposti come impostazioni. È possibile modificare questi per ottimizzare l'aggressività errori o azioni, attivare determinati comportamenti attivato/disattivato e altro ancora.

Utilizzare il seguente cmdlet PowerShell per impostare o modificare le impostazioni.

### <a name="usage"></a>Utilizzo

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name <SettingName> -Value <Value>  
```

#### <a name="example"></a>Esempio

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.Volume.CapacityThreshold.Warning" -Value 70
```

### <a name="common-settings"></a>Impostazioni comuni

Alcune impostazioni modificate comunemente sono elencati di seguito, insieme ai relativi valori predefiniti.

#### <a name="volume-capacity-threshold"></a>Soglia volume di capacità

```
"System.Storage.Volume.CapacityThreshold.Enabled"  = True
"System.Storage.Volume.CapacityThreshold.Warning"  = 80
"System.Storage.Volume.CapacityThreshold.Critical" = 90
```

#### <a name="pool-reserve-capacity-threshold"></a>Soglia capacità di riserva pool

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

#### <a name="platform--quiescence"></a>Piattaforma / Quiescence

```
"Platform.Quiescence.MinDelaySeconds" = 120 (i.e. 2 minutes)
"Platform.Quiescence.MaxDelaySeconds" = 420 (i.e. 7 minutes)
```

#### <a name="metrics"></a>Metriche

```
"System.Reports.ReportingPeriodSeconds" = 1
```

#### <a name="debugging"></a>Il debug

```
"System.LogLevel" = 4
```

## <a name="see-also"></a>Vedere anche

- [Servizio di integrità in Windows Server 2016](health-service-overview.md)
- [Spazi di archiviazione diretta in Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
