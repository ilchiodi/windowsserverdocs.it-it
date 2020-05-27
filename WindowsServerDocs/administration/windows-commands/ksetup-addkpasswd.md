---
title: addkpasswd che Ksetup
description: Argomento di riferimento per il comando che Ksetup addkpasswd, che aggiunge un indirizzo del server di password Kerberos (kpasswd) per un'area di autenticazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0cff2f3d74e6d862bbdd000602a1d03312373d2
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818071"
---
# <a name="ksetup-addkpasswd"></a>addkpasswd che Ksetup

Aggiunge un indirizzo del server di password Kerberos (kpasswd) per un'area di autenticazione.

## <a name="syntax"></a>Sintassi

```
ksetup /addkpasswd <realmname> [<kpasswdname>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<realmname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM ed è elencato come area di autenticazione predefinita o **Realm =** quando viene eseguito **che Ksetup** . |
| `<kpasswdname>` | Specifica il server di password Kerberos. Viene indicato come nome di dominio completo senza distinzione tra maiuscole e minuscole, ad esempio mitkdc.contoso.com. Se viene omesso il nome KDC, DNS potrebbe essere utilizzata per individuare KDC. |

#### <a name="remarks"></a>Osservazioni

- Se l'area di autenticazione Kerberos che la workstation verrà autenticati su supporto di Kerberos modifica protocollo password, è possibile configurare un computer client che eseguono il sistema operativo Windows per utilizzare un server di password Kerberos.

- È possibile aggiungere ulteriori nomi KDC uno alla volta.

### <a name="examples"></a>Esempi

Per configurare la CORP. CONTOSO.COM area di autenticazione per utilizzare il server KDC non Windows, mitkdc.contoso.com, come server delle password, digitare:

```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

Per verificare che sia impostato il nome KDC, digitare `ksetup` e quindi visualizzare l'output, cercando il testo **kpasswd =**. Se il testo non viene visualizzato, significa che il mapping non è stato configurato.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup delkpasswd](ksetup-delkpasswd.md)
