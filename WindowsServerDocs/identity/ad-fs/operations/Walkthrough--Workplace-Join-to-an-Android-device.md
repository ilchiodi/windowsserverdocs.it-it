---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 'Procedura dettagliata: aggiunta alla rete in un dispositivo Android'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873832"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Scenario: Workplace Join a un dispositivo Android

>Si applica a: Windows Server 2016, Windows Server 2012 R2


## <a name="join-your-device-with-workplace-join"></a>Aggiungere il dispositivo con Aggiunta alla rete aziendale

> [!NOTE]
> Android workplace join richiede servizio registrazione dispositivo Azure Active Directory. Per applicare dispositivo condizionale i criteri locali, strumento di sincronizzazione della Directory (DirSync) deve essere distribuito abilitata l'opzione writeback degli oggetti dispositivo. Attualmente il writeback dei dispositivi ad Active Directory da Azure Active Directory può richiedere fino a 3 ore. Di conseguenza, gli utenti devono attendere tre ore per accedere alle applicazioni web in locale, dopo aver creato un account aziendale. Per altre informazioni sulla distribuzione di Azure Active Directory Device Registration service, vedere, [Panoramica di Azure Active Directory Device Registration Service](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Creare un account di lavoro che aggiunge il dispositivo con workplace Join

1.  È necessario installare l'applicazione Azure Authenticator nel dispositivo per creare un account di lavoro che aggiunge il dispositivo con aggiunta alla rete. L'URL seguente contiene istruzioni su come installare l'app Azure authenticator nel dispositivo Android e aggiungere un account aziendale. L'account aziendale rende il dispositivo Android in un dispositivo attendibile e fornisce Single Sign-On (SSO) alle applicazioni nel dispositivo. È possibile usare il dispositivo attendibile per accedere alle applicazioni web e applicazioni line-of-business moderne, come consigliato dall'amministratore IT. Per altre informazioni, vedere [Azure Authenticator per Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Vedere anche
[Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tra le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurazione dell'accesso condizionale locale con Azure Active Directory Device Registration Service](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


