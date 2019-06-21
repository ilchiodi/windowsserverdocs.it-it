---
title: PASSAGGIO 7 di testare la connettività quando si torna alla rete aziendale
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 477d11f0e6bf296c41fb7116a7aae43787df263c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283373"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>PASSAGGIO 7 di testare la connettività quando si torna alla rete aziendale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Molti utenti sposterà tra sedi remote e la rete aziendale, pertanto è importante che quando si torna alla rete aziendale che sono in grado di accedere alle risorse senza dover apportare alcuna configurazione viene modificato. Accesso remoto rende possibile questa perché quando viene restituito al client DirectAccess alla rete aziendale, è in grado di stabilire una connessione al server dei percorsi di rete. Dopo che è stata stabilita la connessione HTTPS al server dei percorsi di rete, il client DirectAccess disabilita la configurazione del client DirectAccess e Usa una connessione diretta alla rete aziendale.  
  
### <a name="test-connectivity-on-client1"></a>Testare la connettività in CLIENT1  
  
1. Spegnere il computer CLIENT1 e quindi scollegare CLIENT1 dal commutatore virtuale o subnet Homenet e connetterlo al commutatore virtuale o subnet Corpnet. Attivare CLIENT1 e accesso come CORP\User1.  
  
2. Aprire una finestra di Windows PowerShell con privilegi elevata, digitare **ipconfig /all**, quindi premere INVIO. L'output indicherà che CLIENT1 è un indirizzo IP locale e che non vi sia alcun active 6to4, Teredo o IP-HTTPS tunneling.  
  
3. Testare la connettività alla condivisione di rete in APP2. Nel **avviare** digitare<strong>\\\APP2\Files</strong>, quindi premere INVIO. Sarà possibile aprire il file in tale cartella.  
  


