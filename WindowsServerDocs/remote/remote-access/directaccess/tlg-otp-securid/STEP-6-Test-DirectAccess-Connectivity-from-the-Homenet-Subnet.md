---
title: PASSAGGIO 6 di testare la connettività DirectAccess dalla Subnet Homenet
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
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 073d05c78631653e41a0962f91fc07e269b4277b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833392"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>PASSAGGIO 6 di testare la connettività DirectAccess dalla Subnet Homenet

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

La distribuzione di DirectAccess One Time password (OTP) è stata completata, è possibile iniziare a testare la connettività dalla subnet Homenet.  
  
### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>Per testare la funzionalità OTP dalla subnet Homenet in CLIENT1  
  
1.  Su CLIENT1, assicurarsi che si è connessi come **User1**.  
  
2.  Nel **avviare** digitare**powershell.exe**, fare doppio clic su **powershell**, fare clic su **avanzate**, quindi fare clic su **eseguire come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
3.  Nella finestra di Windows PowerShell, digitare **gpupdate /force**, quindi premere INVIO.  
  
4.  Disconnettere CLIENT1 dalla subnet Corpnet e connetterlo alla subnet Homenet.  
  
5.  In CLIENT1 aprire Internet Explorer e nella barra degli indirizzi, digitare **https://app1.corp.contoso.com/** e premere INVIO. Premi F5.  
  
    Il sito non devono essere aperti.  
  
6.  Nel **avviare** digitare**RSA**, fare clic su **RSA SecurID Token**.  
  
7.  Attendere fino a quando il token RSA SecurID cambia la password monouso e quindi fare clic su **copia**.  
  
8.  Scegliere l'icona **Connessioni di rete** nell'area di notifica per accedere a Gestione supporti DirectAccess.  
  
9. Fare clic su **connessione DirectAccess di Contoso**, fare clic su **continua**.  
  
10. Premere CTRL + Alt + Canc, quindi scegliere il **One Time password (OTP)** riquadro.  
  
11. Incollare il codice token copiato in precedenza a otto cifre e fare clic su **OK**. Attesa per l'autenticazione per il completamento. Lo stato di connessione DirectAccess all'area di lavoro a questo punto verrà **Connected**.  
  
12. In Internet Explorer, nella barra degli indirizzi, digitare **https://app1.corp.contoso.com/** e premere INVIO. Premi F5. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
13. Nella barra degli indirizzi di Internet Explorer, digitare **https://app2.corp.contoso.com/** e premere INVIO. Premi F5. Verrà visualizzato il sito Web IIS predefinito in APP2.  
  
14. Nel **avviare** digitare**\\\app1\files**, quindi premere INVIO.  
  
15. Nel **file** finestra di cartella condivisa, fare doppio clic sui **example. txt** file. Verrà visualizzato il contenuto del file di example. txt.  
  
16. Nel **avviare** digitare**\\\app2\files**, quindi premere INVIO.  
  
17. Nel **file** finestra di cartella condivisa, fare doppio clic sui **nuovo documento di testo. txt** file. Verrà visualizzato il contenuto del file nuovo documento di testo. txt.  
  


