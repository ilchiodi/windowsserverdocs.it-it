---
title: PASSAGGIO 7 testare la connettività quando si ritorna al corpnet
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 491533ae5d141de4ab4f15126d8977cf15c8f7f4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314680"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>PASSAGGIO 7 testare la connettività quando si ritorna al corpnet

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Molti utenti si spostano tra le posizioni remote e il corpnet, quindi è importante che quando ritornano al corpnet che sono in grado di accedere alle risorse senza dover apportare modifiche alla configurazione. Accesso remoto rende possibile questa operazione perché, quando il client DirectAccess ritorna al corpnet, è in grado di effettuare una connessione al server dei percorsi di rete. Una volta stabilita la connessione HTTPS al server dei percorsi di rete, il client DirectAccess Disabilita la configurazione del client DirectAccess e usa una connessione diretta a Corpnet.  
  
### <a name="test-connectivity-on-client1"></a>Testare la connettività in CLIENT1  
  
1. Arrestare CLIENT1, quindi scollegare CLIENT1 dalla subnet HomeNET o dal Commuter virtuale e connetterlo alla subnet Corpnet o al Commuter virtuale. Attivare CLIENT1 e accedere come CORP\User1.  
  
2. Aprire una finestra di Windows PowerShell con privilegi elevati, digitare **ipconfig**e premere INVIO. L'output indicherà che CLIENT1 ha un indirizzo IP locale e che non esiste alcun tunnel attivo 6to4, Teredo o IP-HTTPS.  
  
3. Testare la connettività alla condivisione di rete in APP2. Nella schermata **Start** Digitare<strong>\\\APP2\Files</strong>, quindi premere INVIO. Sarà possibile aprire il file in tale cartella.  
  


