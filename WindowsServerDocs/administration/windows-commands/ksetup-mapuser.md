---
title: mapuser che Ksetup
description: Argomento di riferimento per il comando che Ksetup mapuser, che esegue il mapping del nome di un'entità Kerberos a un account.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ac2f3e30b3057ceea4376d7ffe8286875d5301d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817671"
---
# <a name="ksetup-mapuser"></a>mapuser che Ksetup

Il nome dell'entità Kerberos viene eseguito il mapping a un account.

## <a name="syntax"></a>Sintassi

```
ksetup /mapuser <principal> <account>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<principal>` | Specifica il nome di dominio completo di qualsiasi utente principale. Ad esempio: mike@corp.CONTOSO.COM. Se non si specifica un parametro dell'account, il mapping viene eliminato per l'entità specificata. |
| `<account>` | Specifica il nome di un account o di un gruppo di sicurezza presente nel computer, ad esempio **Guest**, **Domain Users**o **Administrator**. Se questo parametro viene omesso, il mapping viene eliminato per l'entità specificata. |

#### <a name="remarks"></a>Osservazioni

- Un account può essere identificato in modo specifico, ad esempio **Guest di dominio**, oppure è possibile usare un carattere jolly (*) per includere tutti gli account.

- Il computer autentica solo le entità dell'area di autenticazione specificata se presentano ticket Kerberos validi.

- Quando vengono apportate modifiche per il centro di distribuzione chiavi (KDC) esterna e la configurazione dell'area di autenticazione, è necessario un riavvio del computer in cui è stata modificata l'impostazione.

### <a name="examples"></a>Esempi

Per visualizzare le impostazioni correnti di cui è stato eseguito il mapping e l'area di autenticazione predefinita, digitare:

```
ksetup
```

Per eseguire il mapping dell'account di Mike Petrucci nell'area di autenticazione Kerberos CONTOSO all'account Guest del computer, concedendo a tutti i privilegi di un membro dell'account Guest predefinito senza dover eseguire l'autenticazione al computer, digitare:

```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```

Per rimuovere il mapping dell'account di Mike Petrucci all'account Guest sul computer per impedire l'autenticazione al computer con le credenziali di CONTOSO, digitare:

```
ksetup /mapuser mike@corp.CONTOSO.COM
```

Per eseguire il mapping dell'account di Mike Petrucci nell'area di autenticazione Kerberos di CONTOSO a qualsiasi account esistente nel computer, digitare:

```
ksetup /mapuser mike@corp.CONTOSO.COM *
```

> [!NOTE]
> Se sul computer sono attivi solo gli account utente e Guest standard, i privilegi di Mike sono impostati su questi.

Per eseguire il mapping di tutti gli account all'interno dell'area di autenticazione Kerberos di CONTOSO a qualsiasi account esistente con lo stesso nome nel computer, digitare:

```
ksetup /mapuser * *
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)
