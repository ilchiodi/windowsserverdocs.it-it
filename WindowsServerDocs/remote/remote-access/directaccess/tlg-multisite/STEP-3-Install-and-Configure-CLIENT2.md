---
title: PASSAGGIO 3 di installare e configurare CLIENT2
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2c7ff4953fd4369340f55f40f93cfc01d4240b26
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281464"
---
# <a name="step-3-install-and-configure-client2"></a>PASSAGGIO 3 di installare e configurare CLIENT2

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

CLIENT2 è un Windows 7&reg; computer in cui viene usato per illustrare il con le versioni precedenti la compatibilità di accesso remoto in esecuzione nei server Windows Server 2016.  
  
1. Per installare il sistema operativo su CLIENT2. Installare Windows&reg; 7 Enterprise o Windows&reg; 7 Ultimate su CLIENT2.  
  
2. Per aggiungere CLIENT2 al dominio CORP. Aggiungere CLIENT2 al dominio corp.contoso.com.  
  
## <a name="to-install-the-operating-system-on-client2"></a>Per installare il sistema operativo in CLIENT2  
  
1.  Avviare l'installazione di Windows 7.  
  
2.  Quando viene chiesto di immettere un nome utente, digitare **User1**. Quando viene chiesto di immettere un nome computer, digitare **CLIENT2**.  
  
3.  Quando viene chiesto di immettere una password, digitare due volte una password complessa.  
  
4.  Quando richiesto per le impostazioni di protezione, fare clic su **Usa impostazioni consigliate**.  
  
5.  Quando richiesto per la posizione corrente del computer, fare clic su **rete lavoro**.  
  
6.  Connettersi CLIENT2 a una rete che abbia accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows 7 e quindi disconnettersi da Internet.  
  
7.  Connettersi CLIENT2 alla subnet Corpnet.  
  
## <a name="user-account-control"></a>Controllo dell'account utente  
Quando si configura il sistema operativo Windows 7, è necessario fare clic su **continuazione** nel **User Account Control** (UAC) finestra di dialogo per alcune attività. Alcune delle attività di configurazione richiedono l'approvazione di controllo dell'account utente. Quando viene richiesto, scegliere sempre **continuazione** per autorizzare le modifiche.  
  
## <a name="to-join-client2-to-the-corp-domain"></a>Per aggiungere CLIENT2 al dominio CORP  
  
1.  Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Computer**, quindi scegliere **Proprietà**.  
  
2.  Nel **System** nella pagina il **impostazioni del gruppo di lavoro, dominio e nome Computer** area, fare clic su **modificare le impostazioni**.  
  
3.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
4.  Nel **cambiamenti dominio/nome Computer** della finestra di dialogo fare clic su **dominio**, tipo **corp.contoso.com**, quindi fare clic su **OK**.  
  
5.  Quando viene chiesto di immettere un nome utente e password, digitare il nome utente e la password dell'account di dominio User1 e quindi fare clic su **OK**.  
  
6.  Quando viene visualizzata una finestra di dialogo di benvenuto al dominio corp.contoso.com, fare clic su **OK**.  
  
7.  Quando viene visualizzata una finestra di dialogo casella che viene richiesto di riavviare il computer, fare clic su **OK**.  
  
8.  Nel **le proprietà di sistema** finestra di dialogo, fare clic su **Chiudi**, e quando viene visualizzata una finestra di dialogo che chiede di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, accedere come CORP\User1.
