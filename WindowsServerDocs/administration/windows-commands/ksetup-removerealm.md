---
title: removerealm che Ksetup
description: Argomento di riferimento per il comando che Ksetup removerealm, che consente di eliminare tutte le informazioni per l'area di autenticazione specificata dal registro di sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5da1be77a3b585e566bfd3b051b2fb391b326f32
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817611"
---
# <a name="ksetup-removerealm"></a>removerealm che Ksetup

Elimina tutte le informazioni per l'area di autenticazione specificato dal Registro di sistema.

Il nome dell'area di autenticazione viene archiviato nel registro di sistema in `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001` e `\CurrentControlSet\Control\Lsa\Kerberos` . Per impostazione predefinita, questa voce non esiste nel registro di sistema. Per popolare il registro di sistema, è possibile usare il comando [che Ksetup addrealmflags](ksetup-addrealmflags.md) .

> [!IMPORTANT]
> Non è possibile rimuovere il nome dell'area di autenticazione predefinito dal controller di dominio perché questo Reimposta le informazioni DNS e la rimozione potrebbe rendere inutilizzabile il controller di dominio.

## <a name="syntax"></a>Sintassi

```
ksetup /removerealm <realmname>
```
### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<realmname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM ed è elencato come area di autenticazione predefinita o **Realm =** quando viene eseguito **che Ksetup** . |

### <a name="examples"></a>Esempi

Per rimuovere un nome dell'area di autenticazione errato (. CON anziché. COM) dal computer locale, digitare:
```
ksetup /removerealm CORP.CONTOSO.CON
```

Per verificare la rimozione, è possibile eseguire il comando **che Ksetup** ed esaminare l'output.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup serealone](ksetup-setrealm.md)
