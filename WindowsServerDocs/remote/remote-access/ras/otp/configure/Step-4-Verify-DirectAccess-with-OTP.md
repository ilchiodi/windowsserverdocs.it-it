---
title: Passaggio 4 verificare DirectAccess con OTP
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto con autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcc152723ee339c8652c9480647bfb8f438c87d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813242"
---
# <a name="step-4-verify-directaccess-with-otp"></a>Passaggio 4 verificare DirectAccess con OTP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene descritto come verificare di aver configurato correttamente di DirectAccess con la distribuzione OTP.
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>Per verificare l'integrità OTP sul server di accesso remoto

1. Nel server di accesso remoto aprire il **Gestione accesso remoto** console.  

2. Sotto **server di accesso remoto** fare clic sul server di accesso remoto che è stato configurato per supportare OTP.  

3. Fare clic su **stato operazioni**.  

4. Verificare che lo stato di OTP viene visualizzata l'icona verde ed è funzionante.  
  
    > [!NOTE]  
    > L'intervallo di aggiornamento dello stato di integrità saranno al massimo della somma dei valori dalla chiave del Registro di sistema HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout e il **intervallo di tempo per l'attività del server** che è stata impostata di Configurazione di accesso remoto.  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>Per verificare l'accesso alle risorse interne tramite l'autenticazione OTP  
  
1.  Connettere un computer client DirectAccess alla rete aziendale ed eseguire **gpupdate /force** dal prompt dei comandi per ottenere i criteri di gruppo.  
  
2.  Disconnettere il computer client dalla rete aziendale, connettersi alla rete esterna e tentare di accedere alle risorse interne. Non è necessario accedere alle risorse interne.  
  
3.  Nel caso di un token di software, accedere al token OTP client seguendo le istruzioni del fornitore e osservare il codice token corrente. Quando viene usato un token hardware, seguire le istruzioni del fornitore per l'autenticazione.  
  
4.  Scegliere l'icona **Connessioni di rete** nell'area di notifica per accedere a Gestione supporti DirectAccess.  
  
5.  Scegliere il **connessione DirectAccess**, fare clic su **continua**.  
  
6.  Immettere il codice token indicato in precedenza e fare clic su **OK**. Attesa per l'autenticazione per il completamento. Lo stato di connessione DirectAccess all'area di lavoro a questo punto verrà **Connected**.  
  
7.  Tentativo di accedere alle risorse interne. Si dovrebbe poter accedere a tutte le risorse aziendali.  
  


