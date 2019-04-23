---
title: Unione segmenti ricevuti (RSC) nel commutatore virtuale
description: Ricezione segmento Coalescing (RSC) nel commutatore virtuale è una funzionalità di Windows Server 2019 e Windows 10 ottobre 2018 Update che contribuisce a ridurre utilizzo CPU dell'host e la velocità effettiva aumenta per carichi di lavoro virtuali dall'unione di più segmenti TCP in un numero inferiore, ma di dimensioni maggiori segmenti. L'elaborazione di segmenti di un numero inferiore di grandi dimensioni (Uniti) è più efficiente rispetto all'elaborazione numerosi, segmenti di piccole dimensioni.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.author: dacuo
author: shortpatti
ms.date: 09/07/2018
ms.openlocfilehash: 667e795e398443cadd4c966cc31e65eeee4962f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827782"
---
# <a name="rsc-in-the-vswitch"></a>RSC nel commutatore virtuale
>Si applica a: Windows Server 2019

Ricezione segmento Coalescing (RSC) nel commutatore virtuale è una funzionalità di Windows Server 2019 e Windows 10 ottobre 2018 Update che contribuisce a ridurre utilizzo CPU dell'host e la velocità effettiva aumenta per carichi di lavoro virtuali dall'unione di più segmenti TCP in un numero inferiore, ma di dimensioni maggiori segmenti. L'elaborazione di segmenti di un numero inferiore di grandi dimensioni (Uniti) è più efficiente rispetto all'elaborazione numerosi, segmenti di piccole dimensioni.

Windows Server 2012 e versioni successive è inclusa una versione di solo hardware offload (implementata nella scheda di rete fisica) della tecnologia nota anche come ricevere unione segmenti. Questa versione ODX di RSC è ancora disponibile nelle versioni successive di Windows. Tuttavia, non è compatibile con i carichi di lavoro virtuale ed è stata disabilitata una volta che una scheda di rete fisica collegata a un commutatore virtuale. Per altre informazioni sulla versione di RSC solo hardware, vedere [Receive segmento Coalescing (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Scenari che beneficiano RSC nel commutatore virtuale

I carichi di lavoro il cui percorso dati attraversa un commutatore virtuale trae vantaggio da questa funzionalità.

Ad esempio: 

-   NIC virtuali host tra cui:

    -   Software Defined Networking

    -   Host Hyper-V

    -   Spazi di archiviazione diretta

-   NIC virtuali Guest Hyper-V

-   Software Defined Networking GRE gateway

-   Contenitore

I carichi di lavoro che non sono compatibili con questa funzionalità includono:

-   Software Defined Networking IPSEC gateway

-   NIC virtuali abilitato SR-IOV

-   SMB diretto

## <a name="configure-rsc-in-the-vswitch"></a>Configurare RSC nel commutatore virtuale


Per impostazione predefinita, nei commutatori virtuali esterni, la funzionalità RSC è abilitata.

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Abilitare o disabilitare RSC nel commutatore virtuale**


>[!IMPORTANT]
>Importante: RSC nel commutatore virtuale può essere abilitata e disabilitata in tempo reale senza alcun impatto per le connessioni esistenti.


**Disabilitare RSC nel commutatore virtuale**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Riabilitare RSC nel commutatore virtuale**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Per altre informazioni, vedere [Set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
