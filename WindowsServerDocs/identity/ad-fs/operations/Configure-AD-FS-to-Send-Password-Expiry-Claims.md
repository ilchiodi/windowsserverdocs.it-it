---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configurare AD FS per l'invio di attestazioni di scadenza password
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3be14b824038e9424b86c40bfd657dd988fa99e9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189874"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurare AD FS per l'invio di attestazioni di scadenza password


È possibile configurare Active Directory Federation Services (ADFS) per l'invio attestazioni scadenza password per il trust della relying party (applicazioni) che sono protetti da ad FS. Utilizzo di tali attestazioni dipende dall'applicazione. Ad esempio, con Office 365 come la relying party, gli aggiornamenti sono stati implementati a Exchange e Outlook per notificare agli utenti federati delle password appena-a--scaduto.

Per configurare ADFS per l'invio di password scadenza delle attestazioni per un trust della relying party, è necessario aggiungere le seguenti regole attestazioni per questo trust della relying party:

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Le attestazioni di scadenza delle password sono disponibili solo per nome utente e password e Microsoft Passport per i tipi di autenticazione di lavoro.  Se l'utente viene autenticato tramite l'autenticazione integrata di Windows e Passport non è configurato, le attestazioni non saranno disponibili e gli utenti non visualizzeranno le notifiche di scadenza password.

> [!NOTE]
> C'è una finestra di 14 giorni, in modo che le attestazioni inviate verranno popolate solo se la password è in scadenza entro 14 giorni.

## <a name="see-also"></a>Vedere anche
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
