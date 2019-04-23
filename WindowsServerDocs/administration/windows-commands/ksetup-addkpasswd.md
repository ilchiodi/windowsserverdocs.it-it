---
title: ksetup:addkpasswd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a85eb6dfe30c33126504744a7659fe2cc573087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856822"
---
# <a name="ksetupaddkpasswd"></a>ksetup:addkpasswd



Aggiunge un indirizzo del server Kerberos password (Kpasswd) per un'area di autenticazione. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>Parametri

Se l'area di autenticazione Kerberos che la workstation verrà autenticati su supporto di Kerberos modifica protocollo password, è possibile configurare un computer client che eseguono il sistema operativo Windows per utilizzare un server di password Kerberos. Questa impostazione è configurata sul lato dell'area di autenticazione.

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName >|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e viene indicato il valore predefinito dell'area di autenticazione o dell'area di autenticazione = quando **che ksetup** viene eseguito.|
|\<KpasswdName >|Il nome KDC che deve essere utilizzato come server delle password Kerberos viene dichiarato come un nome di dominio completo distinzione tra maiuscole e minuscole, ad esempio mitkdc.microsoft.com. Se viene omesso il nome KDC, DNS potrebbe essere utilizzata per individuare KDC.|

## <a name="remarks"></a>Note

Se l'area di autenticazione Kerberos che la workstation verrà autenticati su supporto di Kerberos modifica protocollo password, è possibile configurare un computer client che eseguono il sistema operativo Windows per utilizzare un server di password Kerberos.

Eseguire il comando **che ksetup** per verificare il nome KDC. Se **kpasswd =** non viene visualizzato nell'output, non è stato configurato il mapping.

È possibile aggiungere ulteriori nomi KDC uno alla volta.

## <a name="BKMK_Examples"></a>Esempi

Configurare l'area di autenticazione, CORP. CONTOSO.COM, in modo che utilizzi il server KDC non Windows, mitkdc.contoso.com, come server di password:
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
In questo modo un server di password Kerberos non Windows che controlla tutte le password per l'autenticazione tra i file e l'area di autenticazione.

#### <a name="additional-references"></a>Altri riferimenti

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)