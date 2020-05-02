---
title: 'che Ksetup: addkpasswd'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c260c711ae87f88be8b9466e73afaf3fe1c83a1e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724731"
---
# <a name="ksetupaddkpasswd"></a>che Ksetup: addkpasswd



Aggiunge un indirizzo del server Kerberos password (Kpasswd) per un'area di autenticazione.

## <a name="syntax"></a>Sintassi

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

#### <a name="parameters"></a>Parametri

Se l'area di autenticazione Kerberos che la workstation verrà autenticati su supporto di Kerberos modifica protocollo password, è possibile configurare un computer client che eseguono il sistema operativo Windows per utilizzare un server di password Kerberos. Questa impostazione è configurata sul lato dell'area di autenticazione.

|Parametro|Descrizione|
|---------|-----------|
|\<RealmName>|Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM e viene indicato il valore predefinito dell'area di autenticazione o dell'area di autenticazione = quando **che ksetup** viene eseguito.|
|\<> KpasswdName|Il nome KDC che deve essere utilizzato come server delle password Kerberos viene dichiarato come un nome di dominio completo distinzione tra maiuscole e minuscole, ad esempio mitkdc.microsoft.com. Se viene omesso il nome KDC, DNS potrebbe essere utilizzata per individuare KDC.|

## <a name="remarks"></a>Osservazioni

Se l'area di autenticazione Kerberos che la workstation verrà autenticati su supporto di Kerberos modifica protocollo password, è possibile configurare un computer client che eseguono il sistema operativo Windows per utilizzare un server di password Kerberos.

Eseguire il comando **che ksetup** per verificare il nome KDC. Se **kpasswd =** non viene visualizzato nell'output, non è stato configurato il mapping.

È possibile aggiungere ulteriori nomi KDC uno alla volta.

## <a name="examples"></a>Esempi

Configurare l'area di autenticazione, CORP. CONTOSO.COM, in modo che utilizzi il server KDC non Windows, mitkdc.contoso.com, come server di password:
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
In questo modo un server di password Kerberos non Windows che controlla tutte le password per l'autenticazione tra i file e l'area di autenticazione.

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Che Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)