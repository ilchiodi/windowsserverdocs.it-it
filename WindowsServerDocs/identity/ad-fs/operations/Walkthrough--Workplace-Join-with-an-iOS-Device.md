---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 'Procedura dettagliata: aggiunta alla rete con un dispositivo iOS'
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 979802469737066612bc6242f942fd3acd077479
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444796"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Scenario: Aggiunta alla rete aziendale con un dispositivo iOS


> [!IMPORTANT] 
> Questo metodo è rilevante per solo completamente i clienti dall'ambiente locale. Ibrido o solo cloud i clienti non devono utilizzare questo metodo per registrare i propri dispositivi iOS. E questo metodo non compatibile quando i clienti di on-premises decidono di passare al cloud. Il dispositivo deve essere stata annullata la registrazione e registrato con il cloud. 

In questo argomento viene descritta l'aggiunta alla rete aziendale su un dispositivo iOS. È necessario completare i passaggi descritti nel [configurare l'ambiente lab per AD FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) sezione prima di provare questa procedura dettagliata. È possibile usare il dispositivo per accedere alla stessa applicazione web aziendale usata in [procedura dettagliata: Aggiunta alla rete con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Aggiungere un dispositivo iOS con Aggiunta alla rete aziendale

> [!IMPORTANT]
> Quando il servizio DRS locale è configurato, il dispositivo iOS deve considerare attendibile il certificato Secure Socket Layer (SSL) che è stato usato per configurare Active Directory Federation Services (ADFS) in [passaggio 2: Configurare il server federativo (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), per Workplace Join abbia esito positivo.
> 
> -   Se il certificato SSL ADFS è stato rilasciato da un'Autorità di certificazione (CA) di testing, è necessario installare il certificato dell'Autorità di certificazione sul dispositivo iOS.
> -   Se il certificato dell'Autorità di certificazione è pubblicato su un sito Web, si può accedere al sito Web dal dispositivo iOS e installare il certificato.

In questa dimostrazione si aggiunge il dispositivo alla rete aziendale.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Per aggiungere un dispositivo iOS alla rete aziendale

1. -   **Quando il servizio Azure Active Directory Device Registration è il servizio DRS configurato:** Aprire Apple Safari e passare all'endpoint di Azure Active Directory Device Registration service Over-the-Air profilo per dispositivi iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > in cui <`yourdomainname`> è il nome di dominio configurato con Azure Active Directory. Ad esempio, se il nome di dominio è contoso.com, l'URL sarà: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **Quando il servizio DRS locale è il servizio DRS configurato**: Aprire Apple Safari e passare all'endpoint Device Registration Service (DRS) Over-the-Air Profile per i dispositivi iOS `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   È possibile comunicare questo URL agli utenti in diversi modi. Il metodo consigliato consiste nel pubblicare l'URL in un messaggio personalizzato di accesso all'applicazione negato in AD FS. Questa attività viene descritta nella sezione seguente: [Creare un criterio di accesso alle applicazioni e un messaggio di accesso negato personalizzato](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. Accedere a una pagina Web usando un account di dominio aziendale: <strong>roberth@contoso.com</strong> e la password: <strong>P@ssword</strong>.

3. Viene richiesto di installare un profilo. Nella schermata **Installa profilo** fare clic su **Installa**.

4. Quando viene richiesto di confermare l'installazione del profilo, fare clic su **Installa ora**.

5. Se per sbloccare il dispositivo è necessario un PIN, viene chiesto di immetterlo.

6. L'installazione del profilo è completata quando viene visualizzata la schermata **Profilo installato** . Fare clic su **Fine**.

   Tornare a Safari. Un messaggio indica che è possibile chiudere o uscire da Safari.

> [!TIP]
> Per visualizzare o rimuovere il profilo relativo all'aggiunta alla rete aziendale, sul dispositivo iOS passare a **Impostazioni**, fare clic su **Generale** e quindi su **Profili**.

## <a name="see-also"></a>Vedere anche


- [Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurare l'ambiente lab per AD FS in Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Scenario: Aggiungere alla rete aziendale con un dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



