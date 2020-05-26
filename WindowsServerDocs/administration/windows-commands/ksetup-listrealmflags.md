---
title: listrealmflags che Ksetup
description: Argomento di riferimento per il comando che Ksetup listrealmflags, in cui sono elencati i flag dell'area di autenticazione disponibili che possono essere segnalati da che Ksetup.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa96e4da-6b98-4c05-bccf-73cbf33258c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c5ebbee7f937733286e0354eca1cf5524459e86
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817721"
---
# <a name="ksetup-listrealmflags"></a>listrealmflags che Ksetup

Elenca i flag dell'area di autenticazione disponibili che possono essere restituiti da **che ksetup**.

## <a name="syntax"></a>Sintassi

```
ksetup /listrealmflags
```

### <a name="remarks"></a>Osservazioni

- I flag dell'area di autenticazione specificano funzionalità aggiuntive di un'area di autenticazione Kerberos che non sono basate sul sistema operativo Windows Server. Nei computer che eseguono Windows Server, è possibile utilizzare un server Kerberos per amministrare l'autenticazione nell'area di autenticazione Kerberos, anziché utilizzare un dominio in cui viene eseguito un sistema operativo Windows Server. Questa voce stabilisce le funzionalità dell'area di autenticazione e sono le seguenti:

| valore | Flag area di autenticazione | Descrizione |
| ----- | ---------- | ----------- |
| 0xF | Tutti | Tutti i flag dell'area di autenticazione sono impostati. |
| 0x00 | nessuno | Nessun flag area di autenticazione è impostato e non sono abilitate funzionalità aggiuntive. |
| 0x01 | SendAddress | L'indirizzo IP verrà incluso nei ticket di concessione ticket. |
| 0x02 | tcpsupported | In questa area di autenticazione sono supportati sia il Transmission Control Protocol (TCP) che il protocollo UDP (User Datagram Protocol). |
| 0x04 | delegato | Tutti gli utenti in questa area di autenticazione sono attendibili per la delega. |
| 0x08 | ncsupported | Questa area di autenticazione supporta la canonizzazione dei nomi, che consente gli standard di denominazione DNS e area di autenticazione. |
| 0x80 | RC4 | L'area di autenticazione supporta la crittografia RC4 per abilitare il trust, che consente l'uso di TLS. |

- I flag dell'area di autenticazione vengono archiviati nel registro di sistema in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Per impostazione predefinita, questa voce non esiste nel registro di sistema. Per popolare il registro di sistema, è possibile usare il [comando che Ksetup addrealmflags](ksetup-addrealmflags.md) .

## <a name="examples"></a>Esempi

Per elencare i flag dell'area di autenticazione noti nel computer, digitare:

```
ksetup /listrealmflags
```

Per impostare i flag dell'area di autenticazione disponibili che **che Ksetup** non conosce, digitare:

```
ksetup /setrealmflags CORP.CONTOSO.COM sendaddress tcpsupported delete ncsupported
```

**O**

```
ksetup /setrealmflags CORP.CONTOSO.COM 0xF
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup addrealmflags](ksetup-addrealmflags.md)

- [comando che ksetup setrealmflags](ksetup-setrealmflags.md)

- [comando che Ksetup delrealmflags](ksetup-delrealmflags.md)
