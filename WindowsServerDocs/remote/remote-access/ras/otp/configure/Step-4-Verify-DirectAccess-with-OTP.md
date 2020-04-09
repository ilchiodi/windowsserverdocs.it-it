---
title: Passaggio 4 verificare DirectAccess con OTP
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 14d8a3a2138682fb552538948db604a75a1fe898
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858204"
---
# <a name="step-4-verify-directaccess-with-otp"></a>Passaggio 4 verificare DirectAccess con OTP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento descrive come verificare di avere configurato correttamente DirectAccess con la distribuzione OTP.
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>Per verificare l'integrità OTP sul server di accesso remoto

1. Nel server di accesso remoto aprire la console di **gestione accesso remoto** .  

2. In **server di accesso remoto** fare clic sul server di accesso remoto che è stato configurato per il supporto OTP.  

3. Fare clic su **stato operazioni**.  

4. Verificare che lo stato di OTP visualizzi l'icona verde e che funzioni.  
  
    > [!NOTE]  
    > L'intervallo di aggiornamento dello stato di integrità sarà al massimo la somma dei valori della chiave del registro di sistema HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout e l' **intervallo di tempo per l'attività del server** impostata nella configurazione di accesso remoto.  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>Per verificare l'accesso alle risorse interne usando l'autenticazione OTP  
  
1.  Connettere un computer client DirectAccess alla rete aziendale ed eseguire **gpupdate/force** dal prompt dei comandi per ottenere i criteri di gruppo.  
  
2.  Disconnettere il computer client dalla rete aziendale, connettersi alla rete esterna e tentare di accedere alle risorse interne. Non è necessario accedere alle risorse interne.  
  
3.  Nel caso di un token software, accedere al token client OTP usando le istruzioni del fornitore e prendere nota del codice del token corrente. Quando viene usato un token hardware, seguire le istruzioni del fornitore per l'autenticazione.  
  
4.  Scegliere l'icona **Connessioni di rete** nell'area di notifica per accedere a Gestione supporti DirectAccess.  
  
5.  Fare clic sulla **connessione DirectAccess**e quindi su **continua**.  
  
6.  Immettere il codice del token indicato in precedenza, quindi fare clic su **OK**. Attendere il completamento dell'autenticazione. Lo stato della connessione all'area di lavoro DirectAccess verrà ora **connesso**.  
  
7.  Tentativo di accedere alle risorse interne. Si dovrebbe poter accedere a tutte le risorse aziendali.  
  


