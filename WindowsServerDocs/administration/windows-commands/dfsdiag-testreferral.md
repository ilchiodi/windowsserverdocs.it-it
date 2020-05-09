---
title: testreferral Dfsdiag
description: Argomento di riferimento per il comando Dfsdiag testreferral, che controlla i riferimenti file system distribuito (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23abcd738170d5f53e12ae83c41d632d2d7ac738
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992929"
---
# <a name="dfsdiag-testreferral"></a>testreferral Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica i riferimenti file system distribuito (DFS) eseguendo i test seguenti:

- Se si usa il parametro **DFSpath*** senza argomenti, il comando convalida che l'elenco di riferimenti includa tutti i domini trusted.

- Se si specifica un dominio, il comando esegue un controllo di integrità dei controller di`dfsdiag /testdcs`dominio () e testa le associazioni del sito e la cache di dominio dell'host locale.

- Se si specifica un dominio, \SYSvol o \NETLOGON, il comando esegue gli stessi controlli di integrità del controller di dominio, oltre a verificare che la durata **(TTL)** dei riferimenti di SYSVOL o Netlogon corrisponda al valore predefinito di 900 secondi.

- Se si specifica una radice dello spazio dei nomi, il comando esegue gli stessi controlli di integrità del controller di dominio, oltre a`dfsdiag /testdfsconfig`eseguire un controllo della configurazione DFS (`dfsdiag /testdfsintegrity`) e un controllo di integrità dello spazio dei nomi ().

- Se si specifica una cartella DFS (collegamento), il comando esegue gli stessi controlli di integrità radice dello spazio dei nomi, oltre a convalidare la configurazione del sito per le destinazioni di cartella (Dfsdiag/testsites) e di convalidare l'associazione sito dell'host locale.

## <a name="syntax"></a>Sintassi

```
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| DFSpath`<path to get referrals>` | Può essere uno dei seguenti:<ul><li>**Vuoto:** Verifica solo domini trusted.</li><li>`\\Domain:`Verifica solo i riferimenti del controller di dominio.</li><li>`\\Domain\SYSvol:`Verifica solo i riferimenti SYSvol.</li><li>`\\Domain\NETLOGON:`Verifica solo i riferimenti NETLOGON.</li><li>`\\<domain or server>\<namespace root>:`Verifica solo i riferimenti alla radice dello spazio dei nomi.</li><li>`\\<domain or server>\<namespace root>\<DFS folder>:`Verifica solo i riferimenti alla cartella DFS (collegamento).</li></ul> |
| /Full | Si applica solo ai riferimenti di dominio e radice. Verifica la coerenza delle informazioni di associazione del sito tra il registro di sistema e servizi di dominio Active Directory (AD DS). |

## <a name="examples"></a>Esempi

Per controllare i riferimenti file system distribuito (DFS) in *contoso. com\MyNamespace*, digitare:

```
dfsdiag /testreferral /DFSpath:\\contoso.com\MyNamespace
```

Per controllare i riferimenti file system distribuito (DFS) in tutti i domini trusted, digitare:

```
dfsdiag /testreferral /DFSpath:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
