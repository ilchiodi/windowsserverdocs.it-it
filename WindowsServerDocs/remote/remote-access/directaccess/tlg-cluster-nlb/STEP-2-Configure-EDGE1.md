---
title: PASSAGGIO 2 configurare EDGE1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c31e65fc7210849564bd0541085322a7a6c284e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838582"
---
# <a name="step-2-configure-edge1"></a>PASSAGGIO 2 configurare EDGE1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

La procedura seguente viene eseguita nel server DirectAccess:

## <a name="to-configure-directaccess-on-edge1"></a>Per configurare DirectAccess in EDGE1
  
1.  Nel **avviare** digitare**RAMgmtUI.exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione accesso remoto, nel riquadro sinistro, fare clic su **configurazione**.  
  
3.  Nel riquadro centrale della console, nelle **passaggio 2 Server di accesso remoto** area, fare clic su **modificare**.  
  
4.  Nel **configurazione Server di accesso remoto** procedura guidata, fare clic su **configurazione prefissi**. Nel **configurazione prefissi** nella pagina **prefisso IPv6 assegnato ai computer client DirectAccess**, immettere **2001:db8:1:1000:: 59**, quindi fare clic su **successivo** .  
  
5.  Scegliere **Fine**.  
  
6.  Nel riquadro centrale della console, fare clic su **Fine**.  
  
7.  Nel **controllare l'accesso remoto** la finestra di dialogo, rivedere le impostazioni di configurazione e quindi fare clic su **Applica**. Nella finestra di dialogo **Applicazione delle impostazioni della Configurazione guidata Accesso remoto** fare clic su **Chiudi**.
