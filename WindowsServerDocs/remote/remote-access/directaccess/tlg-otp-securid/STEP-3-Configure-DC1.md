---
title: PASSAGGIO 3 Configurare DC1
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7655208fb537e78839f2b459c8df0e24c0573aa7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367550"
---
# <a name="step-3-configure-dc1"></a>PASSAGGIO 3 Configurare DC1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

DC1 funge da controller di dominio, server DNS e server DHCP per il dominio corp.contoso.com. Configurare DC1 come segue:  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Verificare che user1 disponga di un nome dell'entità utente definito in DC1  
  
1.  In DC1 aprire Server Manager, quindi fare clic su **servizi di dominio Active Directory** nel riquadro sinistro. Fare clic con il pulsante destro del mouse su **DC1** e selezionare **Active Directory utenti e computer**. Nel riquadro sinistro espandere **Corp. contoso. com\Users**e fare doppio clic su user1.  
  
2.  Nella scheda **account** verificare che **nome di accesso utente** sia impostato su user1. In caso contrario, immettere **User1** nel campo **nome accesso utente** .  
  
3.  Fare clic su **OK**. Chiudere la console **Utenti e computer di Active Directory** .  
  


