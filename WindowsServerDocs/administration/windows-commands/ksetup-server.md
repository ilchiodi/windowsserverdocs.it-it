---
title: Server che Ksetup
description: Argomento di riferimento per il comando server che Ksetup, che consente di specificare un nome per un computer che esegue il sistema operativo Windows, in modo che le modifiche apportate dal comando che Ksetup aggiornino il computer di destinazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3407111-ac92-457f-aa1f-a04fe9109d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e39a3fbef4b99848d2a90c81007c526597c77275
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817521"
---
# <a name="ksetup-server"></a>Server che Ksetup

Consente di specificare un nome per un computer che esegue il sistema operativo Windows, in modo che le modifiche apportate dal comando **che Ksetup** aggiornino il computer di destinazione.

Il nome del server di destinazione viene archiviato nel registro di sistema in `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\LSA\Kerberos` . Questa voce non viene segnalata quando si esegue il comando **che Ksetup** .

> [!IMPORTANT]
> Non è possibile rimuovere il nome del server di destinazione. Al contrario, è possibile ripristinare il nome del computer locale, ovvero il valore predefinito.

## <a name="syntax"></a>Sintassi

```
ksetup /server <servername>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<servername>` | Specifica il nome completo del computer in cui la configurazione sarà efficace, ad esempio *IPops897.Corp.contoso.com*.<p>Se viene specificato un nome di computer di dominio completo incompleto, il comando avrà esito negativo. |

### <a name="examples"></a>Esempi

Per rendere effettive le configurazioni **che Ksetup** nel computer *IPops897* , che è connesso al dominio contoso, digitare:

```
ksetup /server IPops897.corp.contoso.com
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)
