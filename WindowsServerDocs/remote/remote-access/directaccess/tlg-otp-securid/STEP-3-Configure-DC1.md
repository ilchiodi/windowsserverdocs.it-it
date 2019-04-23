---
title: PASSAGGIO 3 configurare DC1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess con autenticazione OTP e SecurID RSA per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 836a2a08-3d22-48d2-873e-80d7e57ebbd6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b1762fe6e5a98529956208c4c807dfeb39c439cd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875742"
---
# <a name="step-3-configure-dc1"></a>PASSAGGIO 3 configurare DC1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

DC1 agisce come un controller di dominio, server DNS e server DHCP per il dominio corp.contoso.com. Configurare DC1 come segue:  
  
## <a name="verify-user1-has-a-user-principal-name-defined-on-dc1"></a>Verificare che l'utente1 ha un nome dell'entità utente definito in DC1  
  
1.  In DC1 aprire Server Manager e fare clic su **Active Directory Domain Services** nel riquadro sinistro. Fare doppio clic su **DC1** e selezionare **Active Directory Users and Computers**. Nel riquadro sinistro espandere **corp.contoso.com\Users**, fare doppio clic su User1.  
  
2.  Nel **conto** scheda verificare che **nome accesso utente** è impostato su User1. In caso contrario, immettere **User1** nel **nome accesso utente** campo.  
  
3.  Fare clic su **OK**. Chiudere la console **Utenti e computer di Active Directory** .  
  


