---
title: delkpasswd che Ksetup
description: Argomento di riferimento per il comando che Ksetup delkpasswd, che rimuove un server di password Kerberos (kpasswd) per un'area di autenticazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84a70158ad707fb36d1ca8a4879a93fe0b7df06
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817841"
---
# <a name="ksetup-delkpasswd"></a>delkpasswd che Ksetup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove un server di password Kerberos (kpasswd) per un'area di autenticazione.

## <a name="syntax"></a>Sintassi

```
ksetup /delkpasswd <realmname> <kpasswdname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<realmname>` |  Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM ed è elencato come area di autenticazione predefinita o **Realm =** quando viene eseguito **che Ksetup** . |
| `<kpasswdname>` | Specifica il server di password Kerberos. Viene indicato come nome di dominio completo senza distinzione tra maiuscole e minuscole, ad esempio mitkdc.contoso.com. Se viene omesso il nome KDC, DNS potrebbe essere utilizzata per individuare KDC. |

### <a name="examples"></a>Esempi

Per assicurarsi che la CORP dell'area di autenticazione. CONTOSO.COM utilizza il server KDC non Windows mitkdc.contoso.com come server delle password, tipo:

```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

Per assicurarsi che la CORP dell'area di autenticazione. CONTOSO.COM non è mappato a un server di password Kerberos (il nome KDC), digitare `ksetup` sul computer Windows e quindi visualizzare l'output.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup delkpasswd](ksetup-delkpasswd.md)
