---
title: PASSAGGIO 7 testare la connettività quando si ritorna al corpnet
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 479ac48bbb04186f97d25e6744e8630b4dc6a51e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819124"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>PASSAGGIO 7 testare la connettività quando si ritorna al corpnet

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Molti utenti si spostano tra le posizioni remote e il corpnet, quindi è importante che quando ritornano al corpnet che sono in grado di accedere alle risorse senza dover apportare modifiche alla configurazione. Accesso remoto rende possibile questa operazione perché, quando il client DirectAccess ritorna al corpnet, è in grado di effettuare una connessione al server dei percorsi di rete. Una volta stabilita la connessione HTTPS al server dei percorsi di rete, il client DirectAccess Disabilita la configurazione del client DirectAccess e usa una connessione diretta a Corpnet.  
  
### <a name="test-connectivity-on-client1"></a>Testare la connettività in CLIENT1  
  
1. Arrestare CLIENT1, quindi scollegare CLIENT1 dalla subnet HomeNET o dal Commuter virtuale e connetterlo alla subnet Corpnet o al Commuter virtuale. Attivare CLIENT1 e accedere come CORP\User1.  
  
2. Aprire una finestra di Windows PowerShell con privilegi elevati, digitare **ipconfig**e premere INVIO. L'output indicherà che CLIENT1 ha un indirizzo IP locale e che non esiste alcun tunnel attivo 6to4, Teredo o IP-HTTPS.  
  
3. Testare la connettività alla condivisione di rete in APP2. Nella schermata **Start** Digitare<strong>\\\APP2\Files</strong>, quindi premere INVIO. Sarà possibile aprire il file in tale cartella.  
  


