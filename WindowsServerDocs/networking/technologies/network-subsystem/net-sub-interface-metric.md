---
title: Configurare l'ordine delle interfacce di rete
description: Questo argomento fa parte della Guida di ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2edf9d312e50a0fd8485e0032dee8413675367cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurare l'ordine delle interfacce di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In Windows Server 2016 e Windows 10, è possibile utilizzare la metrica di interfaccia per configurare l'ordine delle interfacce di rete.

Questo è diverso da quello nelle versioni precedenti di Windows e Windows Server, che consentono di configurare l'ordine di binding delle schede di rete tramite l'interfaccia utente o i comandi **INetCfgComponentBindings::MoveBefore** e **INetCfgComponentBindings::MoveAfter**. Questi due metodi per ordinare le interfacce di rete non sono disponibili in Windows Server 2016 e Windows 10.

Al contrario, è possibile utilizzare il nuovo metodo per impostare l'ordine enumerato delle schede di rete configurando la metrica di interfaccia di ogni scheda. È possibile configurare la metrica interfaccia tramite la [Set-NetIPInterface](https://docs.microsoft.com/en-us/powershell/module/nettcpip/set-netipinterface) comando di Windows PowerShell.

Quando vengono scelti route del traffico di rete e aver configurato il **InterfaceMetric** parametro del **Set-NetIPInterface** comando, la metrica generale che viene utilizzata per determinare le preferenze dell'interfaccia è la somma della metrica di route e la metrica di interfaccia. La metrica interfaccia offre in genere, la preferenza per una particolare interfaccia, ad esempio usando cablate se entrambi cablate e wireless sono disponibili.

L'esempio di comando di Windows PowerShell riportato di seguito viene illustrato l'utilizzo di questo parametro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

L'ordine in cui le schede visualizzate in un elenco è determinato dalla metrica interfaccia IPv4 o IPv6.  Per ulteriori informazioni, vedere [GetAdaptersAddresses funzione](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Per i collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).
