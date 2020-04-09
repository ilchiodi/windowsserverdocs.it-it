---
title: Unione segmenti ricevuti (RSC) nel commutatore virtuale
description: Receive Segment coalesation (RSC) in vSwitch è una funzionalità di Windows Server 2019 e Windows 10 ottobre 2018 Update che consente di ridurre l'utilizzo della CPU dell'host e di aumentare la velocità effettiva per i carichi di lavoro virtuali, costringendo più segmenti TCP in un minor numero, ma più grandi segmenti. L'elaborazione di un numero minore di segmenti di grandi dimensioni (con Unione) è più efficiente rispetto all'elaborazione di numerosi segmenti di piccole dimensioni.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.author: dacuo
author: dcuomo
ms.date: 09/07/2018
ms.openlocfilehash: 0ffb417728bbdb73d8fb462ff7783b17b511bcd3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814774"
---
# <a name="rsc-in-the-vswitch"></a>RSC in vSwitch
>Si applica a: Windows Server 2019

Receive Segment coalesation (RSC) in vSwitch è una funzionalità di Windows Server 2019 e Windows 10 ottobre 2018 Update che consente di ridurre l'utilizzo della CPU dell'host e di aumentare la velocità effettiva per i carichi di lavoro virtuali, costringendo più segmenti TCP in un minor numero, ma più grandi segmenti. L'elaborazione di un numero minore di segmenti di grandi dimensioni (con Unione) è più efficiente rispetto all'elaborazione di numerosi segmenti di piccole dimensioni.

Windows Server 2012 e versioni successive include una versione di offload solo hardware (implementata nella scheda di rete fisica) della tecnologia nota anche come Unione dei segmenti di ricezione. Questa versione con offload di RSC è ancora disponibile nelle versioni successive di Windows. Tuttavia, non è compatibile con i carichi di lavoro virtuali ed è stato disabilitato una volta collegata una scheda di rete fisica a un vSwitch. Per ulteriori informazioni sulla versione solo hardware di RSC, vedere la pagina relativa all'Unione dei [segmenti Receive (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Scenari che traggono vantaggio da RSC in vSwitch

I carichi di lavoro il cui percorso di DataPath attraversa un Commuter virtuale beneficiano di questa funzionalità.

Ad esempio,

-   NIC virtuali host che includono:

    -   Software Defined Networking

    -   Hyper-V Host

    -   Spazi di archiviazione diretta

-   NIC virtuali guest Hyper-V

-   Gateway GRE di Software Defined Networking

-   Contenitore

I carichi di lavoro che non sono compatibili con questa funzionalità includono:

-   Gateway IPSEC di Software Defined Networking

-   Schede NIC virtuali abilitate per SR-IOV

-   SMB diretto

## <a name="configure-rsc-in-the-vswitch"></a>Configurare RSC in vSwitch


Per impostazione predefinita, in vswitch esterno, RSC è abilitato.

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Abilitare o disabilitare RSC in vSwitch**


>[!IMPORTANT]
>Importante: RSC in vSwitch può essere abilitato e disabilitato in tempo reale senza conseguenze per le connessioni esistenti.


**Disabilitare RSC in vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Abilitare di nuovo RSC in vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Per ulteriori informazioni, vedere [set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
