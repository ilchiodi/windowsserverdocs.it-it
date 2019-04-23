---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: Accedere a una rete aziendale da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0ee22afd6fdec96ab69d915e4730bb834d2ab8ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855522"
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>Accedere a una rete aziendale da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali

>Si applica a: Windows Server 2012 R2

Il rapido aumento del numero di dispositivi di uso personale e l'accesso universale alle informazioni stanno cambiando la percezione della tecnologia da parte delle persone. L'uso costante dell'IT nell'arco dell'intera giornata e l'accesso più facile alle informazioni stanno rendendo sempre più sfocati i tradizionali confini tra il lavoro e la vita privata. Questi confini mutevoli sono accompagnati dalla convinzione che personale selezionata alla tecnologia e personalizzati per adattare personalità degli utenti, le attività e pianificazioni, dovrebbe essere estesa al luogo di lavoro. Per rispondere alla crescente richiesta di connettere i dispositivi di uso personale alla rete aziendale, Microsoft sta introducendo le seguenti proposte di valore:

-   Gli amministratori possono controllare chi può accedere alle risorse dell'azienda sulla base di applicazione, utente, dispositivo e posizione.

-   I dipendenti possono accedere alle applicazioni e ai dati ovunque e con qualsiasi dispositivo. I dipendenti possono usare Single Sign-On nel browser o nelle applicazioni aziendali.

## <a name="key-concepts-introduced-in-the-solution"></a>Concetti principali introdotti nella soluzione

### <a name="workplace-join"></a>Aggiunta all'area di lavoro
Consente agli Information Worker di aggiungere i dispositivi di uso personale ai computer della rete aziendale per accedere alle risorse e ai servizi aziendali. Quando si aggiunge il dispositivo di uso personale alla rete aziendale, questo diventa un dispositivo noto e consente l'autenticazione a due fattori trasparente e Single Sign-On per le applicazioni e le risorse della rete aziendale. Quando si aggiunge un dispositivo con Aggiunta alla rete aziendale, gli attributi del dispositivo possono essere recuperati dalla directory per consentire l'accesso condizionale, allo scopo di autorizzare il rilascio di token di sicurezza per le applicazioni. I dispositivi Windows 8.1 e iOS 6.0+ e i dispositivi Android 4.0+ possono essere aggiunti con Aggiunta all'area di lavoro.

### <a name="BKMK_DRS"></a>Servizio di Azure Active Directory Device Registration
L'aggiunta all'area di lavoro è possibile grazie al servizio Azure Active Directory Device Registration. Quando si aggiunge un dispositivo con Aggiunta alla rete aziendale, il servizio effettua il provisioning dell'oggetto dispositivo in Azure Active Directory e quindi imposta una chiave nel dispositivo locale, usato per rappresentare l'identità del dispositivo. Questa identità del dispositivo può quindi essere usata con le regole di controllo di accesso per le applicazioni ospitate nel cloud e in locale.

Per altre informazioni, vedere [Introduzione alla gestione dei dispositivi in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/device-management-introduction).

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>Aggiunta alla rete aziendale come autenticazione a due fattori trasparente
Le aziende possono gestire il rischio correlato all'accesso alle informazioni e favorire la governance e la conformità, concedendo contemporaneamente l'accesso alle risorse aziendali da parte dei dispositivi di uso personale. Aggiunta alla rete aziendale sui dispositivi offre le seguenti funzionalità agli amministratori:

-   Identifica i dispositivi noti con autenticazione del dispositivo. Gli amministratori possono usare queste informazioni per favorire l'accesso condizionale e il controllo di accesso alle risorse.

-   Fornisce un'esperienza di accesso più trasparente per gli utenti che accedono alle risorse aziendali da dispositivi attendibili.

### <a name="single-sign-on"></a>Single Sign-On
Single Sign-On (SSO) nel contesto di questo scenario è la funzionalità che riduce il numero di richieste di password che l'utente finale deve immettere per accedere alle risorse aziendali da dispositivi noti. Questa funzionalità implica una sola richiesta per tutta la durata dell'SSO per accedere alle applicazioni e alle risorse aziendali da un dispositivo. Se il dispositivo usa Aggiunta alla rete aziendale, per impostazione predefinita l'utente registrato per l'uso di questo dispositivo ottiene l'accesso SSO persistente per sette giorni. L'utente avrà un'esperienza di accesso trasparente nella stessa sessione o in nuove sessioni.

## <a name="solution-overview"></a>Panoramica della soluzione
Nell'ambito di questa soluzione, si apprenderà come usare Aggiunta alla rete aziendale su un dispositivo supportato e come accedere a una risorsa aziendale con Single Sign-On.

> [!NOTE]
> Per supportare dispositivi Windows 8.1, iOS 6.0+ e Android 4.0+, è NECESSARIO configurare Azure Active Directory Device Registration insieme al writeback dell'oggetto dispositivo. Vedere [Guida dettagliata per l'accesso condizionale locale con il servizio Azure Active Directory Device Registration](https://msdn.microsoft.com/library/azure/dn788908.aspx)

Nelle guide alla soluzione sono illustrati i seguenti passaggi della procedura dettagliata:

1.  [Scenario: Aggiunta alla rete con un dispositivo Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [Scenario: Aggiunta alla rete con un dispositivo iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [Scenario: Aggiunta alla rete con un dispositivo Android](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>Vedere anche
[Configurare un server federativo con Device Registration Service](../deployment/configure-a-federation-server-with-device-registration-service.md)



