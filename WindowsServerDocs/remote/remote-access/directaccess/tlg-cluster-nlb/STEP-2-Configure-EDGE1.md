---
title: PASSAGGIO 2 configurare EDGE1
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: eea83bd9788ffd704b6169c140adc68465f9325d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310845"
---
# <a name="step-2-configure-edge1"></a>PASSAGGIO 2 configurare EDGE1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Nel server DirectAccess viene eseguita la procedura seguente:

## <a name="to-configure-directaccess-on-edge1"></a>Per configurare DirectAccess in EDGE1
  
1.  Nella schermata **Start** digitare**RAMgmtUI. exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
2.  Nel riquadro sinistro della console di gestione accesso remoto fare clic su **configurazione**.  
  
3.  Nel riquadro centrale della console, nell'area **passaggio 2 server di accesso remoto** , fare clic su **modifica**.  
  
4.  Nell'installazione guidata del **server di accesso remoto** fare clic su **configurazione prefisso**. Nella pagina **configurazione prefisso** , in **prefisso IPv6 assegnato ai computer client DirectAccess**, immettere **2001: DB8:1: 1000::/59**, quindi fare clic su **Avanti**.  
  
5.  Fare clic su **Fine**.  
  
6.  Nel riquadro centrale della console, fare clic su **Fine**.  
  
7.  Nel **controllare l'accesso remoto** la finestra di dialogo, rivedere le impostazioni di configurazione e quindi fare clic su **Applica**. Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.
