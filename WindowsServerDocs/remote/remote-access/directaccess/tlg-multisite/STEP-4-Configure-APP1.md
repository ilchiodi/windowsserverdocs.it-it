---
title: PASSAGGIO 4 configurare APP1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ccfac583f64d40012881a2d3f6a0897beb16c02a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867222"
---
# <a name="step-4-configure-app1"></a>PASSAGGIO 4 configurare APP1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Configurare impostazioni IPv6 statiche di indirizzamento e gateway per abilitare l'accesso di APP1 per la subnet Corpnet di 2.  
  
- Per configurare il gateway predefinito e il server DNS. La configurazione multisito utilizza il computer ROUTER1 come gateway predefinito. Configurare il gateway predefinito in APP1.  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>Per configurare il gateway predefinito e il server DNS  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi il **delle proprietà** area, accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nel **connessioni di rete** finestra, fare doppio clic su **connessione Ethernet cablata**, quindi fare clic su **proprietà**.  
  
3.  Nel **proprietà della connessione Ethernet cablata** finestra di dialogo, fare clic su **Internet Protocol versione 4 (TCP/IPv4)** e quindi fare clic su **proprietà**.  
  
4.  In **gateway predefinito**, digitare **10.0.0.254**e in **server DNS alternativo**, tipo **10.2.0.1**, quindi fare clic su **OK** .  
  
5.  Nel **proprietà della connessione Ethernet cablata** finestra di dialogo, fare clic su **Internet Protocol versione 6 (TCP/IPv6)** e quindi fare clic su **proprietà**.  
  
6.  Nelle **gateway predefinito**, digitare **2001:db8:1::fe**. Nelle **server DNS alternativo**, digitare **2001:db8:2::1**, quindi fare clic su **OK**.  
  
7.  Nel **proprietà di connessione Ethernet cablata** della finestra di dialogo fare clic su **chiudere**e quindi chiudere il **connessioni di rete** finestra.  
  


