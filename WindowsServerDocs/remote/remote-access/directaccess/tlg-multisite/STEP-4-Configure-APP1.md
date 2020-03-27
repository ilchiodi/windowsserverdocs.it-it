---
title: PASSAGGIO 4 configurare APP1
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 22775a3c453cf59a3dfd1d4b7e8a9522057790b4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308696"
---
# <a name="step-4-configure-app1"></a>PASSAGGIO 4 configurare APP1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Configurare gli indirizzi IPv6 statici e le impostazioni del gateway per abilitare l'accesso APP1 alla subnet 2-Corpnet.  
  
- Per configurare il gateway predefinito e il server DNS. La configurazione multisito usa il computer ROUTER1 come gateway predefinito. Configurare il gateway predefinito in APP1.  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>Per configurare il gateway predefinito e il server DNS  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nella finestra **connessioni di rete** fare clic con il pulsante destro del mouse su **connessione Ethernet cablata**e quindi scegliere **proprietà**.  
  
3.  Nella finestra di dialogo **Proprietà connessione Ethernet cablata** fare clic su **protocollo Internet versione 4 (TCP/IPv4)** , quindi fare clic su **proprietà**.  
  
4.  In **gateway predefinito**Digitare **10.0.0.254**e in **server DNS alternativo**digitare **10.2.0.1**e quindi fare clic su **OK**.  
  
5.  Nella finestra di dialogo **Proprietà connessione Ethernet cablata** fare clic su **protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **proprietà**.  
  
6.  In **gateway predefinito**Digitare **2001: DB8:1:: Fe**. In **server DNS alternativo**Digitare **2001: DB8:2:: 1**, quindi fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà connessione Ethernet cablata** fare clic su **Chiudi**e quindi chiudere la finestra **connessioni di rete** .  
  


