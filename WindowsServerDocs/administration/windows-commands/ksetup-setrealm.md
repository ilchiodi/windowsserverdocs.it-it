---
title: che Ksetup
description: Argomento di riferimento per il comando che Ksetup Severity, che imposta il nome di un'area di autenticazione Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03b33977f57e187a8bea69be78c1e9c094b9a73e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817281"
---
# <a name="ksetup-setrealm"></a>che Ksetup

Imposta il nome di un'area di autenticazione Kerberos.

> [!IMPORTANT]
> L'impostazione dell'area di autenticazione Kerberos in un controller di dominio non è supportata. Il tentativo di eseguire questa operazione causa un errore di avviso e di comando.

## <a name="syntax"></a>Sintassi

```
ksetup /setrealm <DNSdomainname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<DNSdomainname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM. È possibile utilizzare il nome di dominio completo o un modulo semplice del nome. Se non si usa il carattere maiuscolo per il nome DNS, verrà richiesta la verifica per continuare. |

### <a name="examples"></a>Esempi

Per impostare l'area di autenticazione del computer su un nome di dominio specifico e per limitare l'accesso da parte di un controller non di dominio solo all'area di autenticazione Kerberos di CONTOSO, digitare:

```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [removerealm che Ksetup](ksetup-removerealm.md)
