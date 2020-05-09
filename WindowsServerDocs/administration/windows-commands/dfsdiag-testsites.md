---
title: testsites Dfsdiag
description: Argomento di riferimento per Dfsdiag testsites, che controlla la configurazione dei siti di servizi di dominio Active Directory (AD DS) verificando che i server che fungono da server dello spazio dei nomi o cartella (collegamento) abbiano le stesse associazioni di sito in tutti i controller di dominio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54eb7c7ec44d7cd4872960ca29cd3146b710f472
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992846"
---
# <a name="dfsdiag-testsites"></a>testsites Dfsdiag

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Controlla la configurazione dei siti di servizi di dominio Active Directory (AD DS) verificando che i server che fungono da server dello spazio dei nomi o da destinazioni di cartelle (collegamento) dispongano delle stesse associazioni di sito in tutti i controller di dominio.

## <a name="syntax"></a>Sintassi

```
dfsdiag /testsites </machine:<server name>| /DFSpath:<namespace root or DFS folder> [/recurse]> [/full]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/machine:<server name>` | Il nome del server in cui si desidera verificare l'associazione del sito. |
| `/DFSpath:<namespace root or DFS folder>` | La radice dello spazio dei nomi o la cartella file system distribuito (DFS) (collegamento) con le destinazioni per cui verificare l'associazione del sito. |
| /recurse | Enumera e verifica le associazioni di sito per tutte le destinazioni cartella sotto la radice dello spazio dei nomi specificato. |
| /Full | Verifica che di dominio Active Directory e Registro di sistema del server contengono le stesse informazioni di associazione del sito. |

## <a name="examples"></a>Esempi

Per verificare le associazioni del sito in *machine\MyServer*, digitare:

```
dfsdiag /testsites /machine:MyServer
```

Per controllare una cartella file system distribuito (DFS) per verificare l'associazione del sito, insieme alla verifica che servizi di dominio Active Directory e il registro di sistema del server contengano le stesse informazioni di associazione sito, digitare:

```
dfsdiag /TestSites /DFSpath:\\contoso.com\namespace1\folder1 /full
```

Per controllare una radice dello spazio dei nomi per verificare l'associazione del sito, insieme all'enumerazione e alla verifica delle associazioni di sito per tutte le destinazioni di cartelle nella radice dello spazio dei nomi specificata e verificare che servizi di dominio Active Directory e il registro di sistema del server contengano le stesse informazioni di associazione sito, digitare:

```
dfsdiag /testsites /DFSpath:\\contoso.com\namespace2 /recurse /full
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
