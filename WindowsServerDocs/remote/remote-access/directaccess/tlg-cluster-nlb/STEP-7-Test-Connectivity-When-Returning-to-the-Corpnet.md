---
title: PASSAGGIO 7 di testare la connettività quando si torna alla rete aziendale
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
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c21b890523ca61b8f0821692178d050bc11efd79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890812"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>PASSAGGIO 7 di testare la connettività quando si torna alla rete aziendale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Molti utenti sposterà tra sedi remote e la rete aziendale, pertanto è importante che quando si torna alla rete aziendale che sono in grado di accedere alle risorse senza dover apportare alcuna configurazione viene modificato. Accesso remoto rende possibile questa perché quando viene restituito al client DirectAccess alla rete aziendale, è in grado di stabilire una connessione al server dei percorsi di rete. Dopo che è stata stabilita la connessione HTTPS al server dei percorsi di rete, il client DirectAccess disabilita la configurazione del client DirectAccess e Usa una connessione diretta alla rete aziendale.  
  
### <a name="test-connectivity-on-client1"></a>Testare la connettività in CLIENT1  
  
1.  Spegnere il computer CLIENT1 e quindi scollegare CLIENT1 dal commutatore virtuale o subnet Homenet e connetterlo al commutatore virtuale o subnet Corpnet. Attivare CLIENT1 e accesso come CORP\User1.  
  
2.  Aprire una finestra di Windows PowerShell con privilegi elevata, digitare **ipconfig /all**, quindi premere INVIO. L'output indicherà che CLIENT1 è un indirizzo IP locale e che non vi sia alcun active 6to4, Teredo o IP-HTTPS tunneling.  
  
3.  Testare la connettività alla condivisione di rete in APP2. Nel **avviare** digitare**\\\APP2\Files**, quindi premere INVIO. Sarà possibile aprire il file in tale cartella.  
  


