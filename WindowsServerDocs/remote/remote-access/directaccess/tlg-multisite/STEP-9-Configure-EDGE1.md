---
title: PASSAGGIO 9 configurare EDGE1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0a47a436bbd11c795caa8b402054ae0d2c3282f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281370"
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
  
5.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
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
  


