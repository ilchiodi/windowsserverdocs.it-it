---
title: PASSAGGIO 9 configurare EDGE1
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ce86a75ac5b8d53874d2fc5c6743979506591680
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388233"
---
# <a name="step-9-configure-edge1"></a>PASSAGGIO 9 configurare EDGE1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Le seguenti procedure vengono eseguite nel server EDGE1:  
  
1. Configurare i server DNS in EDGE1. È necessario configurare il server DNS dal dominio corp2.corp.contoso.com in EDGE1.  
  
2. Configurare il routing tra subnet. Configurare il routing in EDGE1 per abilitare la comunicazione tra le subnet Corpnet e 2-Corpnet.  
  
## <a name="IPv6"></a>Configurare i server DNS in EDGE1  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **corpnet**, fare clic sul collegamento.  
  
2.  Nella finestra connessioni di rete fare clic con il pulsante destro del mouse su **corpnet**e quindi scegliere **Proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  In **server DNS alternativo**Digitare **10.2.0.1**. e quindi fare clic su **OK**.  
  
5.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
6.  In **server DNS alternativo**Digitare **2001: DB8:2:: 1** e quindi fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà corpnet** fare clic su **Chiudi**.  
  
8.  Chiudere la finestra **Connessioni di rete**.  
  
## <a name="ConfigRouting"></a>Configurare il routing tra subnet  
  
1.  Nella schermata **Start** Digitare**cmd. exe**, fare clic con il pulsante destro del mouse su **cmd**, scegliere **Avanzate**e quindi fare clic su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella finestra del prompt dei comandi immettere i comandi seguenti. Dopo l'immissione di ogni comando, premere INVIO.  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  Chiudere la finestra del prompt dei comandi.  
  


