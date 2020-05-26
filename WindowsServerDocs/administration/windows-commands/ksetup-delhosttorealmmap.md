---
title: delhosttorealmmap che Ksetup
description: Argomento di riferimento per il comando che Ksetup delhosttorealmmap, che rimuove un mapping del nome dell'entità servizio (SPN) tra l'host dichiarato e l'area di autenticazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17fc30e76247c570c653d5ec38501a2199435c7f
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817861"
---
# <a name="ksetup-delhosttorealmmap"></a>delhosttorealmmap che Ksetup

Rimuove un mapping del nome dell'entità servizio (SPN) tra l'host dichiarato e l'area di autenticazione. Questo comando rimuove anche tutti i mapping tra un host e l'area di autenticazione (o più host nell'area di autenticazione).

Il mapping viene archiviato nel registro di sistema, in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm` . Dopo aver eseguito questo comando, è consigliabile verificare che il mapping venga visualizzato nel registro di sistema.

## <a name="syntax"></a>Sintassi

```
ksetup /delhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<hostname>` | Specifica il nome di dominio completo del computer. |
| `<realmname>` | Specifica il nome DNS in maiuscolo, ad esempio CORP. CONTOSO.COM. |

### <a name="examples"></a>Esempi

Per modificare la configurazione dell'area di autenticazione CONTOSO e per eliminare il mapping del computer host IPops897 all'area di autenticazione, digitare:

```
ksetup /delhosttorealmmap IPops897 CONTOSO
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md)
