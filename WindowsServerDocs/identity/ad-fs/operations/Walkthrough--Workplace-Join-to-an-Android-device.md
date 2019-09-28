---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 'Procedura dettagliata: Workplace Join a un dispositivo Android'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c021e8b67963df4f885059c75eff5e94d6dcdafb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407476"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Procedura dettagliata: Workplace Join a un dispositivo Android



## <a name="join-your-device-with-workplace-join"></a>Aggiungere il dispositivo con Aggiunta alla rete aziendale

> [!NOTE]
> L'aggiunta all'area di lavoro Android richiede il servizio Registrazione dispositivo Azure Active Directory. Per applicare i criteri dei dispositivi condizionali in locale, lo strumento di sincronizzazione della directory (DirSync) deve essere distribuito con l'opzione write-back dell'oggetto dispositivo abilitata. Attualmente, il writeback del dispositivo in Active Directory da Azure Active Directory può richiedere fino a 3 ore. Di conseguenza, gli utenti devono attendere 3 ore per accedere alle applicazioni Web locali, dopo la creazione di un account aziendale. Per ulteriori informazioni sulla distribuzione di Registrazione dispositivo Azure Active Directory Service, vedere [Panoramica del servizio registrazione dispositivo Azure Active Directory](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Creare un account aziendale che aggiunge il dispositivo con aggiunta all'area di lavoro

1.  Per creare un account aziendale aggiunto al dispositivo con aggiunta all'area di lavoro, sarà necessario installare Azure Authenticator applicazione nel dispositivo. L'URL seguente contiene istruzioni su come installare l'app Azure Authenticator nel dispositivo Android e aggiungere un account aziendale. L'account aziendale rende il dispositivo Android in un dispositivo attendibile e fornisce l'accesso Single Sign-on (SSO) alle applicazioni nel dispositivo. È possibile usare il dispositivo attendibile per accedere alle applicazioni Web e alle applicazioni line-of-business moderne, come consigliato dall'amministratore IT. Per ulteriori informazioni, vedere [Azure Authenticator per Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Vedere anche
[Aggiunta all'area di lavoro da qualsiasi dispositivo per SSO e autenticazione a due fattori trasparente per tutte le applicazioni aziendali](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurazione dell'accesso condizionale locale con registrazione dispositivo Azure Active Directory servizio](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


