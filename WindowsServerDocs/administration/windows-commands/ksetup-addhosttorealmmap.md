---
title: addhosttorealmmap che Ksetup
description: Argomento di riferimento per il comando che Ksetup addhosttorealmmap, che aggiunge un mapping del nome dell'entità servizio (SPN) tra l'host dichiarato e l'area di autenticazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee2639a5bb071bdd3d6ac3f6373e881c18f3bf9a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818111"
---
# <a name="ksetup-addhosttorealmmap"></a>addhosttorealmmap che Ksetup

Aggiunge un mapping del nome dell'entità servizio (SPN) tra l'host dichiarato e l'area di autenticazione. Questo comando consente inoltre di eseguire il mapping di un host o di più host che condividono lo stesso suffisso DNS per l'area di autenticazione.

Il mapping viene archiviato nel registro di sistema, in **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**.

## <a name="syntax"></a>Sintassi

```
ksetup /addhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| `<hostname>` | Il nome host è il nome del computer e può essere dichiarato come nome di dominio completo del computer. |
| `<realmname>` | Il nome dell'area di autenticazione è specificato come un nome DNS lettere maiuscole, ad esempio CORP. CONTOSO.COM. |

### <a name="examples"></a>Esempi

Per eseguire il mapping del computer host *IPops897* all'area di autenticazione di *Contoso* , digitare:

```
ksetup /addhosttorealmmap IPops897 CONTOSO
```

Controllare il registro di sistema per verificare che il mapping si sia verificato come previsto.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando che Ksetup](ksetup.md)

- [comando che Ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md)
