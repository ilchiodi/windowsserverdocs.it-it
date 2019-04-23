---
title: PASSAGGIO 9 configurare EDGE1
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
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f5d216934f0d09cdef97ce4405161862b112d632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864872"
---
# <a name="step-9-configure-edge1"></a>PASSAGGIO 9 configurare EDGE1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Le procedure seguenti vengono eseguite nel server EDGE1:  
  
1. Configurare i server DNS EDGE1. È necessario configurare il server DNS dal dominio corp2.corp.contoso.com su EDGE1.  
  
2. Configurare il routing tra subnet. Configurare il routing su EDGE1 per abilitare la comunicazione tra le subnet Corpnet e 2-Corpnet.  
  
## <a name="IPv6"></a>Configurare i server DNS EDGE1  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi la **proprietà** area, accanto a **Corpnet**, fare clic sul collegamento.  
  
2.  Nella finestra di connessioni di rete, fare doppio clic su **Corpnet**, quindi fare clic su **proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  Nelle **server DNS alternativo**, digitare **10.2.0.1**. e quindi fare clic su **OK**.  
  
5.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)**, quindi fare clic su **Proprietà**.  
  
6.  Nelle **server DNS alternativo**, digitare **2001:db8:2::1** e quindi fare clic su **OK**.  
  
7.  Nel **le proprietà della rete aziendale** finestra di dialogo, fare clic su **Chiudi**.  
  
8.  Chiudere la finestra **Connessioni di rete**.  
  
## <a name="ConfigRouting"></a>Configurare il routing tra subnet  
  
1.  Nel **avviare** digitare**cmd.exe**, fare doppio clic su **cmd**, fare clic su **avanzate**e quindi fare clic su **runas amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella finestra del prompt dei comandi, immettere i comandi seguenti. Dopo aver immesso tutti i comandi, premere INVIO.  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  Chiudere la finestra del prompt dei comandi.  
  


