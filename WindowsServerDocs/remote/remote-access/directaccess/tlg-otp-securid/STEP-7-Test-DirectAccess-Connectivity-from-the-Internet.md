---
title: PASSAGGIO 7 testare la connettività DirectAccess da Internet
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 738e0f10762c0d292e344ba25fa34cdb0d17b766
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367543"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>PASSAGGIO 7 testare la connettività DirectAccess da Internet

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

La distribuzione della password monouso (OTP) DirectAccess è stata testata dalla subnet HomeNET e ora può essere testata da Internet.  
  
### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>Per testare la funzionalità OTP da Internet in CLIENT1  
  
1. In CLIENT1 assicurarsi di essere connessi come **User1**. Connettere CLIENT1 alla subnet Corpnet.  
  
2. Nella schermata **Start** Digitare**PowerShell. exe**, fare clic con il pulsante destro del mouse su **PowerShell**, scegliere **Avanzate**e quindi fare clic su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
3. Nella finestra di Windows PowerShell, digitare **gpupdate/force** e premere INVIO.  
  
4. Scollegare CLIENT1 dalla subnet HomeNET, connetterlo a Internet e riavviare il computer.  
  
5. In CLIENT1 aprire Internet Explorer e nella barra degli indirizzi digitare **https://app1.corp.contoso.com/** e premere INVIO. Premi F5.  
  
   Il sito non dovrebbe aprirsi.  
  
6. Nella schermata **Start** digitare**RSA**, quindi fare clic su **RSA SecurID Token**.  
  
7. Attendere che il token SecurID RSA modifichi la password monouso e quindi fare clic su **copia**.  
  
8. Scegliere l'icona **Connessioni di rete** nell'area di notifica per accedere a Gestione supporti DirectAccess.  
  
9. Fare clic su **connessione all'area di lavoro**e fare clic su **continua**.  
  
10. Premere CTRL + ALT + CANC, quindi fare clic sul riquadro password monouso **(OTP)** .  
  
11. Incollare il codice token di otto cifre copiato in precedenza, quindi fare clic su **OK**. Attendere il completamento dell'autenticazione. Lo stato della connessione all'area di lavoro DirectAccess verrà ora **connesso**.  
  
12. Nella barra degli indirizzi di Internet Explorer digitare **https://app1.corp.contoso.com/** e premere INVIO. Premi F5. Viene visualizzato il sito Web IIS predefinito in APP1.  
  
13. Nella barra degli indirizzi di Internet Explorer digitare **https://app2.corp.contoso.com/** e premere INVIO. Premi F5. Viene visualizzato il sito Web IIS predefinito in APP2.  
  
14. Nella schermata **Start** Digitare<strong>\\ \ APP1\FILES</strong>e premere INVIO.  
  
15. Nella finestra cartella condivisa **file** fare doppio clic sul file **example. txt** . Il contenuto del file example. txt sarà visualizzato.  
  
16. Nella schermata **Start** Digitare<strong>\\ \ APP2\FILES</strong>e premere INVIO.  
  
17. Nella finestra cartella condivisa **file** fare doppio clic sul nuovo file **Document. txt di testo** . Viene visualizzato il contenuto del nuovo file Document. txt di testo.  
  


