---
title: PASSAGGIO 7 di testare la connettività DirectAccess da Internet
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
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0fe096bcf697e5131eeefc2fa72e61e0096e2f94
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446914"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>PASSAGGIO 7 di testare la connettività DirectAccess da Internet

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

La distribuzione di DirectAccess One Time password (OTP) è stata testata dalla subnet Homenet e può ora essere testata da Internet.  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>Per testare la funzionalità OTP da Internet su CLIENT1  
  
1. In CLIENT1 assicurarsi che si è connessi come **User1**. Connettere CLIENT1 alla subnet Corpnet.  
  
2. Nel **avviare** digitare**powershell.exe**, fare doppio clic su **powershell**, fare clic su **avanzate**, quindi fare clic su **eseguire come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
3. Nella finestra di Windows PowerShell, digitare **gpupdate /force** e premere INVIO.  
  
4. Disconnettere CLIENT1 dalla subnet Homenet, connetterla a Internet e riavviare il computer.  
  
5. In CLIENT1 aprire Internet Explorer e nella barra degli indirizzi, digitare **https://app1.corp.contoso.com/** e premere INVIO. Premi F5.  
  
   Il sito non devono essere aperti.  
  
6. Nel **avviare** digitare**RSA**, fare clic su **RSA SecurID Token**.  
  
7. Attendere fino a quando il token RSA SecurID cambia la password monouso e quindi fare clic su **copia**.  
  
8. Scegliere l'icona **Connessioni di rete** nell'area di notifica per accedere a Gestione supporti DirectAccess.  
  
9. Fare clic su **connessione all'area di lavoro**, fare clic su **continua**.  
  
10. Premere CTRL + Alt + Canc, quindi scegliere il **One Time password (OTP)** riquadro.  
  
11. Incollare il codice token copiato in precedenza a otto cifre e fare clic su **OK**. Attesa per l'autenticazione per il completamento. Lo stato di connessione DirectAccess all'area di lavoro a questo punto verrà **Connected**.  
  
12. In Internet Explorer, nella barra degli indirizzi, digitare **https://app1.corp.contoso.com/** e premere INVIO. Premi F5. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
13. Nella barra degli indirizzi di Internet Explorer, digitare **https://app2.corp.contoso.com/** e premere INVIO. Premi F5. Verrà visualizzato il sito Web IIS predefinito in APP2.  
  
14. Nel **avviare** digitare<strong>\\\app1\files</strong>, quindi premere INVIO.  
  
15. Nel **file** finestra di cartella condivisa, fare doppio clic sui **example. txt** file. Verrà visualizzato il contenuto del file di example. txt.  
  
16. Nel **avviare** digitare<strong>\\\app2\files</strong>, quindi premere INVIO.  
  
17. Nel **file** finestra di cartella condivisa, fare doppio clic sui **nuovo documento di testo. txt** file. Verrà visualizzato il contenuto del file nuovo documento di testo. txt.  
  


