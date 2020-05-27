---
title: setrealmflags che Ksetup
description: Argomento di riferimento per il comando che ksetup setrealmflags, che imposta i flag dell'area di autenticazione per l'area di autenticazione specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcb2824e-fba7-4ebe-be62-e62b4fae5b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2de568ae3df522f43d7a84e22d76ee3975d8e209
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817321"
---
# <a name="ksetup-setrealmflags"></a>setrealmflags che Ksetup

Imposta i flag dell'area di autenticazione per l'area di autenticazione specificata.

## <a name="syntax"></a>Sintassi

```
ksetup /setrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<realmname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM. |

#### <a name="remarks"></a>Osservazioni

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

- I flag dell'area di autenticazione vengono archiviati nel registro di sistema in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Per impostazione predefinita, questa voce non esiste nel registro di sistema. Per popolare il registro di sistema, è possibile usare il comando [che Ksetup addrealmflags](ksetup-addrealmflags.md) .

- È possibile visualizzare i flag di area di autenticazione disponibili e impostati visualizzando l'output di **che Ksetup** o `ksetup /dumpstate` .

### <a name="examples"></a>Esempi

Per elencare i campi disponibili e per impostare i flag di autenticazione per l'area di autenticazione CONTOSO, digitare:

```
ksetup
```

Per impostare due flag che non sono attualmente impostati, digitare:

```
ksetup /setrealmflags CONTOSO ncsupported delegate
```

Per verificare che il flag area di autenticazione sia impostato, digitare `ksetup` e quindi visualizzare l'output, cercando il testo **Realm Flags =**. Se il testo non viene visualizzato, significa che il flag non è stato impostato.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup listrealmflags](ksetup-listrealmflags.md)

- [comando che Ksetup addrealmflags](ksetup-addrealmflags.md)

- [comando che Ksetup delrealmflags](ksetup-delrealmflags.md)

- [comando che Ksetup dumpstate](ksetup-dumpstate.md)
