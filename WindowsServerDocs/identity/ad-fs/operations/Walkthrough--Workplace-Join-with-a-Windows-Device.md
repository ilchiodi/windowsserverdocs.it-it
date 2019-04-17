---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: Procedura dettagliata - aggiunta alla rete aziendale con un dispositivo Windows
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fd222eb47982591e051594e8a572443b65c0357f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Procedura dettagliata: All'area di lavoro con un dispositivo Windows

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In questo argomento illustra come usare aggiunta alla rete aziendale per connettere il dispositivo Windows alla rete aziendale e come accedere a un'applicazione web con Single Sign-On. È necessario completare i passaggi descritti nel [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) sezione prima di provare questa procedura dettagliata.

## <a name="access-the-web-application-before-device-registration"></a>Accedere all'applicazione web prima di registrazione del dispositivo
In questa procedura dettagliata, si accede un'applicazione web della società prima di aggiungere il dispositivo all'area di lavoro. La pagina Web sono visualizzate le attestazioni incluse nel token di sicurezza. Si noti che l'elenco di attestazioni non include le informazioni sul dispositivo. Si noterà probabilmente anche che non è Single Sign-On.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Per accedere all'applicazione web prima di usare aggiunta alla rete aziendale sul dispositivo

1.  Accedere a Client1 con l'account Microsoft.

2.  Apri Internet Explorer e passare all'App attestazioni generica, **https://webserv1.contoso.com/claimapp**.

3.  Accedere a una pagina Web usando un account di dominio aziendale: ** roberth@contoso.com **, password: ** P@ssword **.

4.  La pagina Web sono elencate tutte le attestazioni incluse nel token di sicurezza. Solo le attestazioni utente sono presenti nel token di sicurezza.

5.  Chiudere Internet Explorer.

6.  Aprire Internet Explorer e passare alla stessa app, le attestazioni **https://webserv1.contoso.com/claimapp**.

7.  Si noti che viene chiesto di immettere di nuovo le credenziali. Puoi non sono connessi alla rete aziendale da un dispositivo con aggiunta alla rete aziendale e pertanto non è Single Sign-On.

## <a name="join-your-device-with-workplace-join"></a>Aggiungere il dispositivo con aggiunta alla rete aziendale

> [!IMPORTANT]
> Per aggiunta alla rete aziendale abbia esito positivo, il computer client (Client1) deve considerare attendibile il certificato SSL usato per configurare Active Directory Federation Services (ADFS) nel [passaggio 2: configurare il Server federativo con Device Registration Service (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Deve inoltre essere in grado di convalidare le informazioni di revoca del certificato. Se hai problemi con l'aggiunta alla rete aziendale, è possibile visualizzare il registro eventi in Client1.
> 
> Per visualizzare il registro eventi, aprire il Visualizzatore eventi, espandere **registri applicazioni e servizi**, espandere **Microsoft**, espandere **Windows**, quindi fare clic su **aggiunta alla rete aziendale**.

#### <a name="to-join-your-device-with-workplace-join"></a>Per aggiungere il dispositivo con aggiunta alla rete aziendale

1.  Accedere a Client1 con l'account Microsoft.

2.  Nel **Start** dello schermo, aprire il **accessi** e quindi selezionare il **impostazioni** accesso. Selezionare **Modifica impostazioni PC**.

3.  Nel **impostazioni PC** selezionare **rete**, quindi fare clic su **all'area di lavoro**.

4.  Nel **immettere l'ID utente per accedere alla rete aziendale o attivare la gestione dei dispositivi** digitare ** roberth@contoso.com **, quindi fare clic su **aggiungere**.

5.  Quando vengono richieste le credenziali, digitare ** roberth@contoso.com **e la password: ** P@ssword **. Fare clic su **OK**.

6.  Dovrebbe essere visualizzato il messaggio: "il dispositivo è stato aggiunto alla tua rete aziendale."

### <a name="access-the-web-application-after-joining-the-workplace"></a>Accedere all'applicazione web dopo l'aggiunta di area di lavoro
In questa parte della dimostrazione, si accede a un'applicazione web aziendale dal dispositivo connesso con aggiunta alla rete aziendale. La pagina Web sono visualizzate le attestazioni incluse nel token di sicurezza. Si noti che l'elenco di attestazioni include informazioni utente e dispositivo. Si noterà probabilmente anche che ora si ha Single Sign-On.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Per accedere all'applicazione web dopo l'aggiunta di area di lavoro

1.  Accedere a **Client1** con il tuo account Microsoft.

2.  Apri Internet Explorer e passare all'App attestazioni generica, **https://webserv1.contoso.com/claimapp**.

3.  Accedere a una pagina Web usando un account di dominio aziendale: ** roberth@contoso.com **, password: ** P@ssword **.

4.  La pagina Web sono elencate le attestazioni nel token di sicurezza. Il token contiene le attestazioni utente e dispositivo.

5.  Chiudere Internet Explorer.

6.  Aprire Internet Explorer e passare alla stessa app, le attestazioni **https://webserv1.contoso.com/claimapp**.

7.  Si noti che sono **non** richiesto di immettere di nuovo le credenziali. Si è connessi da un dispositivo con aggiunta alla rete aziendale e pertanto ha Single Sign-On.

## <a name="see-also"></a>Vedere anche
[Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
[procedura dettagliata: aggiunta alla rete aziendale con un dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



