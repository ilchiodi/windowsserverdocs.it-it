---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configurare AD FS per l'invio di attestazioni di scadenza password
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0c04f42cbe29b1ea36d09f1f148fb28280d7fec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859894"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurare AD FS per l'invio di attestazioni di scadenza password


È possibile configurare Active Directory Federation Services (AD FS) per inviare attestazioni di scadenza password ai trust di relying party (applicazioni) protetti da ADFS. Utilizzo di tali attestazioni dipende dall'applicazione. Ad esempio, con Office 365 come la relying party, gli aggiornamenti sono stati implementati a Exchange e Outlook per notificare agli utenti federati delle password appena-a--scaduto.

Per configurare AD FS per inviare attestazioni di scadenza password a una relying party attendibilità, è necessario aggiungere le seguenti regole attestazioni a questo relying party trust:

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Le attestazioni per la scadenza delle password sono disponibili solo per nome utente e password e tipi di autenticazione Microsoft Passport for Work.  Se l'utente esegue l'autenticazione con autenticazione integrata di Windows e Passport non è configurato, le attestazioni non saranno disponibili e gli utenti non visualizzeranno le notifiche di scadenza delle password.

> [!NOTE]
> È presente una finestra di 14 giorni in modo che le attestazioni inviate verranno popolate solo se la password scade entro 14 giorni.

## <a name="see-also"></a>Vedi anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
