---
title: delrealmflags che Ksetup
description: Argomento di riferimento per il comando che Ksetup delrealmflags, che rimuove i flag dell'area di autenticazione dall'area di autenticazione specificata.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22053041-1eb4-47f5-bed9-3d5681bcde7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8d983a00683fec0fa1bb9801caabe226a4ffeb9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817831"
---
# <a name="ksetup-delrealmflags"></a>delrealmflags che Ksetup

Rimuove i flag dell'area di autenticazione dall'area di autenticazione specificata.

## <a name="syntax"></a>Sintassi

```
ksetup /delrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<realmname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM ed è elencato come area di autenticazione predefinita o **Realm =** quando viene eseguito **che Ksetup** . |

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

- I flag dell'area di autenticazione vengono archiviati nel registro di sistema in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\Kerberos\Domains\<realmname>` . Per impostazione predefinita, questa voce non esiste nel registro di sistema. Per popolare il registro di sistema, è possibile usare il [comando che Ksetup addrealmflags](ksetup-addrealmflags.md) .

- È possibile visualizzare i flag di area di autenticazione disponibili e impostati visualizzando l'output di **che Ksetup** o `ksetup /dumpstate` .

### <a name="examples"></a>Esempi

Per elencare i flag dell'area di autenticazione disponibili per l'area di autenticazione CONTOSO, digitare:

```
ksetup /listrealmflags
```

Per rimuovere due flag attualmente presenti nel set, digitare:

```
ksetup /delrealmflags CONTOSO ncsupported delegate
```

Per verificare che i flag dell'area di autenticazione siano stati rimossi, digitare `ksetup` e quindi visualizzare l'output, cercando il testo **Realm Flags =**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup listrealmflags](ksetup-listrealmflags.md)

- [comando che ksetup setrealmflags](ksetup-setrealmflags.md)

- [comando che Ksetup addrealmflags](ksetup-addrealmflags.md)

- [comando che Ksetup dumpstate](ksetup-dumpstate.md)
