---
title: TestReferral Dfsdiag
description: Argomento dei comandi di Windows per Dfsdiag TestReferral, che controlla i riferimenti file system distribuito (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5c0a75d557d816ac9e19a1e22b3273195b93f53
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846247"
---
# <a name="dfsdiag-testreferral"></a>TestReferral Dfsdiag

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica i riferimenti file system distribuito (DFS) eseguendo i test seguenti:

- Quando si usa il parametro DFSpath senza argomenti, questo comando convalida che l'elenco di riferimenti includa tutti i domini trusted.

- Quando si specifica un dominio, il comando esegue un controllo di integrità dei controller di dominio (Dfsdiag/testdcs) e testa le associazioni del sito e la cache di dominio dell'host locale.

- Quando si specifica un dominio, \SYSvol o \NETLOGON, oltre a eseguire gli stessi controlli di integrità di quando si specifica un dominio, il comando verifica che la durata (\TTL) dei riferimenti di SYSvol o NETLOGON corrisponda al valore predefinito di 900 secondi.

- Quando si specifica una radice dello spazio dei nomi, oltre a eseguire gli stessi controlli di integrità di quando si specifica un dominio, il comando esegue un controllo della configurazione DFS (Dfsdiag/TestDFSConfig) e un controllo di integrità dello spazio dei nomi (Dfsdiag/TestDFSIntegrity).

- Quando si specifica una cartella DFS (collegamento), oltre a eseguire gli stessi controlli di integrità di quando si specifica una radice dello spazio dei nomi, il comando convalida la configurazione del sito per le destinazioni cartella (Dfsdiag/testsites) e convalida l'associazione sito dell'host locale.

## <a name="syntax"></a>Sintassi

```
dfsdiag /TestReferral /DFSpath:<DFS path for getting referrals> [/Full]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
| /DFSpath:<path for getting referrals>|Il percorso DFS può essere uno dei seguenti:<p>-   \(\)vuoto: verifica i domini trusted.<br />-   \\\\dominio: riferimenti del controller di dominio.<br />-   \\\\dominio\\SYSvol: riferimenti SYSvol.<br />-   \\\\Doma in\\NETLOGON: riferimenti NETLOGON.<br />-   \\\\<Domain or server>\\<Namespace Root>: riferimenti radice dello spazio dei nomi.<br />-   \\\\<Domain or server>\\<Namespace root>\\<DFS folder>: cartella DFS \(collegamento\) riferimenti.|
|/Full|Applicabile solo a riferimenti dominio radice. Verifica la coerenza delle informazioni di associazione del sito tra il registro di sistema e i servizi di dominio Active Directory \(\)AD DS.|

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

```
dfsdiag /TestReferral /DFSpath:\\Contoso.com\MyNamespace
```

```
dfsdiag /TestReferral /DFSpath:
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


