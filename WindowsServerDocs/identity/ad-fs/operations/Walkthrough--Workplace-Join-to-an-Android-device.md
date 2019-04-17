---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: Procedura dettagliata - aggiunta alla rete aziendale per un dispositivo Android
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Procedura dettagliata: All'area di lavoro a un dispositivo Android

>Si applica a: Windows Server 2016, Windows Server 2012 R2


## <a name="join-your-device-with-workplace-join"></a>Aggiungere il dispositivo con aggiunta alla rete aziendale

> [!NOTE]
> Aggiunta alla rete aziendale Android richiede Azure Active Directory Device Registration Service. Per applicare criteri condizionali periferica in locale, strumento di sincronizzazione della Directory (DirSync) deve essere distribuito con opzione di write-back oggetto dispositivo abilitato. Al momento, writeback del dispositivo in Active Directory da Azure Active Directory può richiedere fino a 3 ore. Di conseguenza, gli utenti devono attendere 3 ore accedere alle applicazioni web in locale, dopo aver creato un account aziendale. Per ulteriori informazioni sulla distribuzione di Azure Active Directory Device Registration service, vedere, [Panoramica di Azure Active Directory dispositivo registrazione servizio](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Creare un account aziendale che unisce il dispositivo con aggiunta all'area di lavoro

1.  È necessario installare Azure Authenticator applicazione nel tuo dispositivo per creare un account aziendale che unisce il dispositivo con aggiunta alla rete aziendale. L'URL seguente contiene le istruzioni su come installare app Azure authenticator sul dispositivo Android e aggiungere un account aziendale. L'account aziendale rende il dispositivo Android in un dispositivo attendibile e fornisce Single Sign-On (SSO) per le applicazioni nel dispositivo. È possibile utilizzare il dispositivo attendibile per accedere ad applicazioni web e applicazioni line-of-business moderne, come consigliato dall'amministratore IT. Per ulteriori informazioni, vedere [Azure Authenticator per Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Vedere anche
[Accedere a una rete aziendale da qualsiasi dispositivo per SSO e trasparente secondo fattore di autenticazione tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurazione di accesso condizionale locale con Azure Active Directory Device Registration Service](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


