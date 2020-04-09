---
title: PASSAGGIO 3 installare e configurare CLIENT2
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: f009fdd1-94e6-4ccb-8c6e-609a5394db53
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c651a9173fa15856e62bf09767ed90375acefcd5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861594"
---
# <a name="step-3-install-and-configure-client2"></a>PASSAGGIO 3 installare e configurare CLIENT2

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

CLIENT2 è un computer Windows 7&reg; usato per illustrare la compatibilità con le versioni precedenti di accesso remoto in esecuzione nei server Windows Server 2016.  
  
1. Per installare il sistema operativo in CLIENT2. Installare Windows&reg; 7 Enterprise o Windows&reg; 7 Ultimate in CLIENT2.  
  
2. Per aggiungere CLIENT2 al dominio CORP. Aggiungere CLIENT2 al dominio corp.contoso.com.  
  
## <a name="to-install-the-operating-system-on-client2"></a>Per installare il sistema operativo in CLIENT2  
  
1.  Avviare l'installazione di Windows 7.  
  
2.  Quando viene richiesto un nome utente, digitare **User1**. Quando viene richiesto un nome computer, digitare **CLIENT2**.  
  
3.  Quando viene richiesta una password, digitare una password complessa due volte.  
  
4.  Quando vengono richieste le impostazioni di protezione, fare clic su **Usa le impostazioni consigliate**.  
  
5.  Quando viene richiesto il percorso corrente del computer, fare clic su **rete aziendale**.  
  
6.  Connettere CLIENT2 a una rete dotata di accesso a Internet ed eseguire Windows Update per installare gli ultimi aggiornamenti per Windows 7 e quindi disconnettersi da Internet.  
  
7.  Connettere CLIENT2 alla subnet Corpnet.  
  
## <a name="user-account-control"></a>Controllo dell'account utente  
Quando si configura il sistema operativo Windows 7, è necessario fare clic su **continua** nella finestra di dialogo **controllo dell'account utente** (UAC) per alcune attività. Diverse attività di configurazione richiedono l'approvazione del controllo dell'account utente. Quando richiesto, fare sempre clic su **continua** per autorizzare le modifiche.  
  
## <a name="to-join-client2-to-the-corp-domain"></a>Per aggiungere CLIENT2 al dominio CORP  
  
1.  Fare clic sul pulsante **Start**, fare clic con il pulsante destro del mouse su **Computer**, quindi scegliere **Proprietà**.  
  
2.  Nella pagina **sistema** , nell'area **Impostazioni nome computer, dominio e gruppo** di lavoro, fare clic su **Cambia impostazioni**.  
  
3.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
4.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** fare clic su **dominio**, digitare **Corp.contoso.com**e quindi fare clic su **OK**.  
  
5.  Quando viene richiesto di immettere un nome utente e una password, digitare il nome utente e la password per l'account di dominio User1 e quindi fare clic su **OK**.  
  
6.  Quando viene visualizzata una finestra di dialogo che accoglie il dominio corp.contoso.com, fare clic su **OK**.  
  
7.  Quando viene visualizzata una finestra di dialogo in cui viene richiesto di riavviare il computer, fare clic su **OK**.  
  
8.  Nella finestra di dialogo **proprietà del sistema** fare clic su **Chiudi**e, quando viene visualizzata una finestra di dialogo in cui viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, accedere come CORP\User1.
