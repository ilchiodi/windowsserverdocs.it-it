---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: 'Procedura dettagliata: Workplace Join con un dispositivo Windows'
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 68249c4afcd3fc23f040020a221e53df6d2f6865
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816014"
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Procedura dettagliata: Aggiunta alla rete aziendale con un dispositivo Windows

Questo argomento illustra come usare Aggiunta alla rete aziendale per connettere il dispositivo Windows alla rete aziendale e come accedere a un'applicazione Web con Single Sign-On. Per poter provare questa procedura dettagliata, è necessario completare i passaggi della sezione [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) .

## <a name="access-the-web-application-before-device-registration"></a>Accedere all'applicazione Web prima della registrazione del dispositivo
In questa procedura dettagliata si accede a un'applicazione Web della società prima di aggiungere il proprio dispositivo alla rete aziendale. Nella pagina Web sono visualizzate le attestazioni incluse nel token di sicurezza. Si noti che l'elenco di attestazioni non include alcuna informazione sul dispositivo. Si noterà probabilmente anche che non si ha l'accesso Single Sign-On.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Per accedere all'applicazione Web prima di usare Aggiunta alla rete aziendale sul dispositivo

1. Accedere a Client1 con l'account Microsoft.

2. Aprire Internet Explorer e passare all'app per le attestazioni generica, **https://webserv1.contoso.com/claimapp** .

3. Accedere alla pagina Web usando un account di dominio aziendale: <strong>roberth@contoso.com</strong>, password: <strong>P@ssword</strong>.

4. Nella pagina Web sono elencate tutte le attestazioni incluse nel token di sicurezza. Nel proprio token di sicurezza sono presenti solo le attestazioni utente.

5. Chiudere Internet Explorer.

6. Aprire Internet Explorer e passare alla stessa app Claims, **https://webserv1.contoso.com/claimapp** .

7. Verrà chiesto di immettere di nuovo le credenziali. Non essendo connessi alla rete aziendale da un dispositivo con Aggiunta alla rete aziendale, non si ha Single Sign-On.

## <a name="join-your-device-with-workplace-join"></a>Aggiungere il dispositivo con Aggiunta alla rete aziendale

> [!IMPORTANT]
> Per il corretto funzionamento dell'aggiunta all'area di lavoro, il computer client (Client1) deve considerare attendibile il certificato SSL usato per configurare Active Directory Federation Services (AD FS) nel [Step 2: Configure the Federation Server with Device Registration Service (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Deve essere inoltre in grado di convalidare le informazioni di revoca per il certificato. In caso di problemi con l'unione all'area di lavoro, è possibile visualizzare il registro eventi in Client1.
> 
> Per visualizzare il registro eventi, aprire il Visualizzatore eventi, espandere **Registri applicazioni e servizi**, espandere **Microsoft**, espandere **Windows** e quindi fare clic su **Aggiunta alla rete aziendale**.

#### <a name="to-join-your-device-with-workplace-join"></a>Per aggiungere il dispositivo con Aggiunta alla rete aziendale

1. Accedere a Client1 con l'account Microsoft.

2. Nella schermata **Start** del computer client aprire la **barra degli accessi** e quindi selezionare l'accesso alle **impostazioni**. Selezionare **Modifica impostazioni PC**.

3. Nella pagina **Impostazioni PC** fare clic su **Rete** e quindi su **Rete aziendale**.

4. Nella casella **immettere l'ID utente per ottenere l'accesso alla rete aziendale o attivare la gestione dei dispositivi** Digitare <strong>roberth@contoso.com</strong>e quindi fare clic su **Aggiungi**.

5. Quando vengono richieste le credenziali, digitare <strong>roberth@contoso.com</strong>e password: <strong>P@ssword</strong>. Fare clic su **OK**.

6. Dovrebbe essere visualizzato il messaggio: "Il dispositivo è stato aggiunto alla tua rete aziendale".

### <a name="access-the-web-application-after-joining-the-workplace"></a>Accedere all'applicazione Web dopo l'aggiunta alla rete aziendale
In questa parte della dimostrazione si accede all'applicazione Web della società dal dispositivo connesso con Aggiunta alla rete aziendale. Nella pagina Web sono visualizzate le attestazioni incluse nel token di sicurezza. Nell'elenco di attestazioni sono incluse informazioni sia sul dispositivo che sull'utente. Si noterà probabilmente anche che ora si ha l'accesso Single Sign-On.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Per accedere all'applicazione Web dopo l'aggiunta alla rete aziendale

1. Accedere a **Client1** con l'account Microsoft.

2. Aprire Internet Explorer e passare all'app per le attestazioni generica, **https://webserv1.contoso.com/claimapp** .

3. Accedere alla pagina Web usando un account di dominio aziendale: <strong>roberth@contoso.com</strong>, password: <strong>P@ssword</strong>.

4. Nella pagina Web sono elencate le attestazioni incluse nel token di sicurezza. Il token contiene sia le attestazioni utente che quelle del dispositivo.

5. Chiudere Internet Explorer.

6. Aprire Internet Explorer e passare alla stessa app Claims, **https://webserv1.contoso.com/claimapp** .

7. **Non**  verrà chiesto di immettere di nuovo le credenziali. Essendo connessi alla rete aziendale da un dispositivo con Aggiunta alla rete aziendale, si ha Single Sign-On.

## <a name="see-also"></a>Vedi anche
[Aggiunta all'area di lavoro da qualsiasi dispositivo per SSO e autenticazione a due fattori trasparente per tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
[procedura dettagliata: workplace join con un dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



