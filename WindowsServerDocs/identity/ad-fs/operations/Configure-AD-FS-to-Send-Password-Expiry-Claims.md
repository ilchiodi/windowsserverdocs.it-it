---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configurare ADFS per l'invio attestazioni scadenza Password
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 386a5ac921ba609c371121b8657351667628951b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurare ADFS per l'invio attestazioni scadenza Password

>Si applica a: Windows Server 2016, Windows Server 2012 R2

È possibile configurare Active Directory Federation Services (ADFS) per l'invio di attestazioni di scadenza delle password per il trust della relying party (applicazioni) che sono protetti da ADFS. Utilizzo di tali attestazioni dipende dall'applicazione. Ad esempio, con Office 365 come la relying party, gli aggiornamenti sono stati implementati a Exchange e Outlook per notificare agli utenti federati delle password appena-a--scaduto.

Per configurare ADFS per l'invio di password attestazioni di scadenza per un trust della relying party, è necessario aggiungere le seguenti regole attestazione per questo trust della relying party:

```
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
=> issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Le attestazioni di scadenza password disponibili solo per nome utente e password e Microsoft Passport per i tipi di autenticazione.  Se l'utente esegue l'autenticazione tramite autenticazione integrata di Windows e Passport non è configurato, le attestazioni non sarà disponibile e gli utenti non visualizzeranno le notifiche di scadenza password.

> [!NOTE]
> È disponibile una finestra di 14 giorni in modo che le attestazioni inviate verranno popolate solo se la password sta per scadere entro 14 giorni.

## <a name="see-also"></a>Vedere anche
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md)
