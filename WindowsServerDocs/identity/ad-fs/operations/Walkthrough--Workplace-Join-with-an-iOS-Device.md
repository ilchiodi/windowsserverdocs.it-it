---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 'Procedura dettagliata: Workplace Join con un dispositivo iOS'
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7b1d2a5f5c32d55e482f5f53a04668b34fc9aece
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815994"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Procedura dettagliata: Aggiunta alla rete aziendale con un dispositivo iOS


> [!IMPORTANT] 
> Questo metodo è pertinente solo per i clienti completamente locali. I clienti ibridi o solo cloud non devono usare questo metodo per registrare i propri dispositivi iOS. Questo metodo non è compatibile quando i clienti locali decidono di passare al cloud. È necessario annullare la registrazione del dispositivo e registrarlo nel cloud. 

In questo argomento viene descritta l'aggiunta alla rete aziendale su un dispositivo iOS. Per poter provare questa procedura dettagliata, è necessario completare i passaggi della sezione [configurare l'ambiente lab per ad FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) . È possibile usare il dispositivo per accedere alla stessa applicazione Web aziendale a cui si accede in [procedura dettagliata: workplace join con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Aggiungere un dispositivo iOS con Aggiunta alla rete aziendale

> [!IMPORTANT]
> Per il corretto funzionamento dell'aggiunta alla rete aziendale, quando è configurato il servizio DRS locale il dispositivo iOS deve considerare attendibile il certificato SSL (Secure Socket Layer) usato per configurare Active Directory Federation Services (AD FS) nel [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4).
> 
> -   Se il certificato SSL ADFS è stato rilasciato da un'Autorità di certificazione (CA) di testing, è necessario installare il certificato dell'Autorità di certificazione sul dispositivo iOS.
> -   Se il certificato dell'Autorità di certificazione è pubblicato su un sito Web, si può accedere al sito Web dal dispositivo iOS e installare il certificato.

In questa dimostrazione si aggiunge il dispositivo alla rete aziendale.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Per aggiungere un dispositivo iOS alla rete aziendale

1. -   **Quando registrazione dispositivo Azure Active Directory servizio è il DRS configurato:** Aprire Apple Safari e passare all'endpoint Registrazione dispositivo Azure Active Directory Service over-the-Air profile per i dispositivi iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > dove <`yourdomainname`> è il nome di dominio configurato con Azure Active Directory. Ad esempio, se il nome di dominio è contoso.com, l'URL sarà: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **Quando DRS locale è il servizio DRS configurato**: aprire Apple Safari e passare all'endpoint del profilo di registrazione del dispositivo (DRS) over-the-Air profile per i dispositivi iOS, `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   È possibile comunicare questo URL agli utenti in diversi modi. Il metodo consigliato consiste nel pubblicare l'URL in un messaggio personalizzato di accesso all'applicazione negato in AD FS. Questa attività viene descritta nella sezione seguente: [Creare un criterio di accesso alle applicazioni e un messaggio di accesso negato personalizzato](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. Accedere alla pagina Web usando un account di dominio aziendale: <strong>roberth@contoso.com</strong> e password: <strong>P@ssword</strong>.

3. Viene richiesto di installare un profilo. Nella schermata **Installa profilo** fare clic su **Installa**.

4. Quando viene richiesto di confermare l'installazione del profilo, fare clic su **Installa ora**.

5. Se per sbloccare il dispositivo è necessario un PIN, viene chiesto di immetterlo.

6. L'installazione del profilo è completata quando viene visualizzata la schermata **Profilo installato**. Fare clic su **Done**.

   Tornare a Safari. Un messaggio indica che è possibile chiudere o uscire da Safari.

> [!TIP]
> Per visualizzare o rimuovere il profilo relativo all'aggiunta alla rete aziendale, sul dispositivo iOS passare a **Impostazioni**, fare clic su **Generale** e quindi su **Profili**.

## <a name="see-also"></a>Vedi anche


- [Accedere a una rete aziendale da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurare l'ambiente lab per AD FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Procedura dettagliata: Workplace Join con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



