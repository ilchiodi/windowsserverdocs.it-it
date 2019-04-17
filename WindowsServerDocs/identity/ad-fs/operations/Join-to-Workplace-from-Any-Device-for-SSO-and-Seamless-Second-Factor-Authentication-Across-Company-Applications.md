---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4926eb32a0bbffb092ec02ca2508fe97d52d1466
ms.sourcegitcommit: 46439194e5deb0fa5f338b428f95dd6b5b799337
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2018
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali

>Si applica a: Windows Server 2012 R2

Il rapido aumento del numero di dispositivi e l'accesso universale alle informazioni sta cambiando il modo che gli utenti hanno la percezione della tecnologia. L'uso costante dell'IT nel corso della giornata, insieme a accesso più facile alle informazioni, è sfocatura tradizionali confini tra il lavoro e la vita privata. Questi confini mutevoli sono accompagnati dalla convinzione che personale selezionata alla tecnologia e personalizzati per adattare personalità degli utenti, attività e pianificazioni, dovrebbe essere estesa al luogo di lavoro. Per rispondere alla crescente richiesta di dispositivi personali per la connessione a reti aziendali, stiamo introducendo le seguenti proposte di valore:

-   Gli amministratori possono controllare chi ha accesso alle risorse aziendali basate su applicazione, utente, dispositivo e posizione.

-   I dipendenti possono accedere applicazioni e ai dati ovunque e con qualsiasi dispositivo. I dipendenti possono usare Single Sign-On nel browser o le applicazioni aziendali.

## <a name="key-concepts-introduced-in-the-solution"></a>Concetti principali introdotti nella soluzione

### <a name="workplace-join"></a>Aggiunta alla rete aziendale
Tramite l'aggiunta alla rete aziendale, gli information worker possono aggiungere i propri dispositivi personali con i computer all'area di lavoro aziendale per accedere ai servizi e alle risorse aziendali. Quando si aggiunge il dispositivo personale all'area di lavoro, diventa un dispositivo noto e fornisce trasparente secondo fattore di autenticazione e Single Sign-On per applicazioni e risorse della rete aziendale. Quando si aggiunge un dispositivo con aggiunta alla rete aziendale, gli attributi del dispositivo possono essere recuperati dalla directory per consentire l'accesso condizionale allo scopo di autorizzare il rilascio di token di sicurezza per le applicazioni. Windows 8.1 e i dispositivi iOS 6.0 + e Android 4.0 + possono essere aggiunti con aggiunta alla rete aziendale.

### <a name="BKMK_DRS"></a>Servizio di Azure Active Directory Device Registration
Aggiunta alla rete aziendale è resa possibile dal servizio Azure Active Directory Device Registration. Quando si aggiunge un dispositivo con aggiunta alla rete aziendale, il servizio provisioning di un oggetto dispositivo in Azure Active Directory e quindi imposta una chiave nel dispositivo locale che viene utilizzato per rappresentare l'identità del dispositivo. Questa identità del dispositivo può quindi essere usata con le regole di controllo di accesso per le applicazioni ospitate nel cloud e locali.

Per ulteriori informazioni sull'abilitazione del servizio Registrazione dispositivi di Azure Active Directory, vedere [Panoramica di Azure Active Directory dispositivo registrazione servizio](https://msdn.microsoft.com/6a14cb1f-a058-4453-8ede-d9f4a66a7073.aspx).

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>Aggiunta all'area di lavoro come un secondo trasparente l'autenticazione a fattore
Le aziende possono gestire il rischio che è correlato all'accesso alle informazioni e favorire la governance e conformità durante la concessione consumer dispositivi dell'accesso alle risorse aziendali. Aggiunta alla rete aziendale sui dispositivi offre le funzionalità seguenti per gli amministratori:

-   Identifica i dispositivi noti con autenticazione del dispositivo. Gli amministratori possono utilizzare queste informazioni per favorire l'accesso condizionale e controllare l'accesso alle risorse.

-   Offre un'esperienza di accesso più trasparente per gli utenti di accedere alle risorse aziendali da dispositivi attendibili.

### <a name="single-sign-on"></a>Single Sign-On
Single Sign-On (SSO) nel contesto di questo scenario è la funzionalità che riduce il numero di richieste di password che l'utente finale deve immettere per accedere alle risorse aziendali da dispositivi noti. Questa funzionalità implica che agli utenti viene richiesto solo una volta durante la durata del SSO per le applicazioni aziendali di accesso e risorse da un dispositivo. Se il dispositivo usa aggiunta alla rete aziendale, l'utente che ha registrato per l'uso di questo dispositivo Ottiene l'accesso SSO persistente, per impostazione predefinita per sette giorni. L'utente ha una segno di esperienza trasparente nella stessa sessione o in nuove sessioni.

## <a name="solution-overview"></a>Panoramica della soluzione
Come parte di questa soluzione, si impara a usare aggiunta alla rete aziendale su un dispositivo supportato e Single Sign-On a una risorsa aziendale.

> [!NOTE]
> Per il supporto di Windows 8.1, i dispositivi iOS 6.0 + e Android 4.0 +, è necessario configurare Azure Active Directory Device Registration insieme al writeback dell'oggetto dispositivo, vedere [Guida dettagliata per l'accesso condizionale locale con Azure Active Directory Device Registration Service](https://msdn.microsoft.com/library/azure/dn788908.aspx)

Questa soluzione nelle guide alla tramite i seguenti passaggi di questa procedura dettagliata:

1.  [Procedura dettagliata: All'area di lavoro con un dispositivo Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [Procedura dettagliata: Aggiunta alla rete aziendale con un dispositivo iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [Procedura dettagliata: All'area di lavoro con un dispositivo Android](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>Vedere anche
[Configurare un server federativo con Device Registration Service](../deployment/configure-a-federation-server-with-device-registration-service.md)



