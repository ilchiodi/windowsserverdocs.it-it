---
title: PASSAGGIO 9 configurare EDGE1
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a3f0b291890e294389b611527bd34d2ec8afa832
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861534"
---
# <a name="step-9-configure-edge1"></a>PASSAGGIO 9 configurare EDGE1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Le seguenti procedure vengono eseguite nel server EDGE1:  
  
1. Configurare i server DNS in EDGE1. È necessario configurare il server DNS dal dominio corp2.corp.contoso.com in EDGE1.  
  
2. Configurare il routing tra subnet. Configurare il routing in EDGE1 per abilitare la comunicazione tra le subnet Corpnet e 2-Corpnet.  
  
## <a name="configure-the-dns-servers-on-edge1"></a><a name="IPv6"></a>Configurare i server DNS in EDGE1  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **corpnet**, fare clic sul collegamento.  
  
2.  Nella finestra connessioni di rete fare clic con il pulsante destro del mouse su **corpnet**e quindi scegliere **Proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  In **server DNS alternativo**Digitare **10.2.0.1**. e quindi fare clic su **OK**.  
  
5.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
6.  In **server DNS alternativo**Digitare **2001: DB8:2:: 1** e quindi fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà corpnet** fare clic su **Chiudi**.  
  
8.  Chiudere la finestra **Connessioni di rete**.  
  
## <a name="configure-routing-between-subnets"></a><a name="ConfigRouting"></a>Configurare il routing tra subnet  
  
1.  Nella schermata **Start** Digitare**cmd. exe**, fare clic con il pulsante destro del mouse su **cmd**, scegliere **Avanzate**e quindi fare clic su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Nella finestra del prompt dei comandi immettere i comandi seguenti. Dopo l'immissione di ogni comando, premere INVIO.  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  Chiudere la finestra del prompt dei comandi.  
  


