---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: Procedura dettagliata - aggiunta alla rete aziendale con un dispositivo iOS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a8643bab913dfec07c6bbea0c068e1240f16c5b
ms.sourcegitcommit: a2260c96b0e49519d180c7382b921ce8ddb053fe
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Procedura dettagliata: Aggiunta alla rete aziendale con un dispositivo iOS

>Si applica a: Windows Server 2012 R2

In questo argomento viene aggiunta alla rete aziendale su un dispositivo iOS. È necessario completare i passaggi descritti nel [impostare dell'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) sezione prima di provare questa procedura dettagliata. È possibile utilizzare il dispositivo per accedere alla stessa applicazione web aziendale usata in [procedura dettagliata: aggiunta alla rete aziendale con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).

## <a name="join-an-ios-device-with-workplace-join"></a>Aggiungere un dispositivo iOS con aggiunta alla rete aziendale

> [!IMPORTANT]
> Quando il servizio DRS locale è configurato, il dispositivo iOS deve considerare attendibile il certificato Secure Socket Layer (SSL) utilizzato per configurare Active Directory Federation Services (ADFS) in [passaggio 2: configurare il server federativo (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), per l'unione all'area di lavoro abbia esito positivo.
> 
> -   Se il certificato SSL ADFS è stato rilasciato da un'autorità di certificazione (CA) di test, è necessario installare il certificato dell'autorità di certificazione sul dispositivo iOS.
> -   Se il certificato dell'autorità di certificazione è pubblicato in un sito Web, è possibile passare al sito Web dal dispositivo iOS e installare il certificato.

In questa dimostrazione si aggiunge il dispositivo all'area di lavoro.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Per aggiungere un dispositivo iOS a una rete aziendale

1.  -   **Quando il servizio Azure Active Directory Device Registration è il servizio DRS configurato:** aprire Apple Safari e passare all'endpoint di Azure Active Directory Device Registration service ota profilo per i dispositivi iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > in <`yourdomainname`> è il nome di dominio che è stato configurato con Azure Active Directory. Ad esempio, se il nome di dominio è contoso.com, l'URL sarà: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **Quando DRS locale è il servizio DRS configurato**: aprire Apple Safari e passare all'endpoint Device Registration Service (DRS) ota Profile per i dispositivi iOS`https://adf1s.contoso.com/enrollmentserver/otaprofile`

    Esistono molti modi per comunicare questo URL agli utenti. Il metodo consigliato consiste nel pubblicare l'URL di un accesso dell'applicazione personalizzata negato messaggio in ADFS. Questa attività viene descritta nella sezione seguente: [creare un criterio di accesso di applicazioni e di accesso negato messaggio personalizzato](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  Accedere a una pagina Web usando un account di dominio aziendale: ** roberth@contoso.com ** e la password: ** P@ssword **.

3.  Viene chiesto di installare un profilo. Nel **Installa profilo** schermata, fare clic su **installare**.

4.  Quando viene richiesto di confermare l'installazione del profilo, fare clic su **installa**.

5.  Se il dispositivo richiede un PIN sbloccare il dispositivo, viene chiesto di immettere il PIN.

6.  L'installazione del profilo è completata quando viene visualizzata la **profilo installato** dello schermo. Fare clic su **eseguita**.

    Tornare a Safari. Un messaggio indica che è possibile chiudere o uscire da Safari.

> [!TIP]
> Per visualizzare o rimuovere il profilo aggiunta alla rete aziendale, passare a **impostazioni**, fare clic su **generale**, quindi fare clic su **profili** sul dispositivo iOS.

## <a name="see-also"></a>Vedere anche


- [Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurare l'ambiente di testing per ADFS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Procedura dettagliata: All'area di lavoro con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



