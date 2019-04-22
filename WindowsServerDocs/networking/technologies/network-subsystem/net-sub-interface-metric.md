---
title: Configurare l'ordine delle interfacce di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete di Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 18bc9a268b4e69e4b87b6b1e310f1162473adb10
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821772"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurare l'ordine delle interfacce di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In Windows Server 2016 e Windows 10, è possibile usare la metrica interfaccia per configurare l'ordine delle interfacce di rete.

Ciò è diverso da quello in versioni precedenti di Windows e Windows Server, che consentiva di configurare l'ordine di binding di schede di rete usando l'interfaccia utente o i comandi **INetCfgComponentBindings::MoveBefore**e **INetCfgComponentBindings::MoveAfter**. Questi due metodi per l'ordinamento di interfacce di rete non sono disponibili in Windows Server 2016 e Windows 10.

In alternativa, è possibile utilizzare il nuovo metodo per impostare l'ordine enumerato delle schede di rete configurando la metrica di interfaccia di ogni scheda. È possibile configurare la metrica interfaccia usando il [Set-NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) comando Windows PowerShell.

Quando vengono scelte le route del traffico di rete ed è stata configurata la **InterfaceMetric** parametro delle **Set-NetIPInterface** comando, la metrica generale che consente di determinare l'interfaccia preferenza è la somma della metrica di interfaccia e la metrica della route. In genere, la metrica interfaccia accorda priorità a una determinata interfaccia, come l'utilizzo di reti cablate se sia cablato e wireless sono disponibili.

Nell'esempio di comando di Windows PowerShell seguente illustra l'uso di questo parametro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

L'ordine in cui vengono visualizzati gli adapter in un elenco è determinata dalla metrica interfaccia IPv4 o IPv6.  Per altre informazioni, vedere [GetAdaptersAddresses funzione](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Per collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni di rete sottosistema](net-sub-performance-top.md).
