---
title: fsutil resource
description: Argomento di riferimento per il comando fsutil resource, che consente di gestire un Gestione risorse transazionale e il relativo comportamento.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b64dab3a0d0e772f067b0d3fa548692aad38a4c5
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435736"
---
# <a name="fsutil-resource"></a>fsutil resource

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Crea una Gestione risorse transazionale secondaria, avvia o arresta una Gestione risorse transazionale oppure Visualizza informazioni su un Gestione risorse transazionale e modifica il comportamento seguente:

- Indica se un Gestione risorse transazionale predefinito pulisce i metadati transazionali al montaggio successivo.

- Gestione risorse transazionale specificato per preferire la coerenza rispetto alla disponibilità.

- La transazione specificata Gestione risorse di preferire la disponibilità rispetto alla coerenza.

- Caratteristiche di un Gestione risorse transazionale in esecuzione.

## <a name="syntax"></a>Sintassi

```
fsutil resource [create] <rmrootpathname>
fsutil resource [info] <rmrootpathname>
fsutil resource [setautoreset] {true|false} <Defaultrmrootpathname>
fsutil resource [setavailable] <rmrootpathname>
fsutil resource [setconsistent] <rmrootpathname>
fsutil resource [setlog] [growth {<containers> containers|<percent> percent} <rmrootpathname>] [maxextents <containers> <rmrootpathname>] [minextents <containers> <rmrootpathname>] [mode {full|undo} <rmrootpathname>] [rename <rmrootpathname>] [shrink <percent> <rmrootpathname>] [size <containers> <rmrootpathname>]
fsutil resource [start] <rmrootpathname> [<rmlogpathname> <tmlogpathname>
fsutil resource [stop] <rmrootpathname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| create | Crea una Gestione risorse transazionale secondaria. |
| `<rmrootpathname>` | Specifica il percorso completo di una directory radice Gestione risorse transazionale. |
| info | Consente di visualizzare le informazioni sul Gestione risorse transazionale specificato. |
| setautoreset | Specifica se un Gestione risorse transazionale predefinito pulirà i metadati transazionali al montaggio successivo.<ul><li>**true** : specifica che la transazione gestione risorse eliminerà i metadati transazionali al montaggio successivo, per impostazione predefinita.</li><li>**false** : specifica che la transazione gestione risorse non eliminerà i metadati transazionali al montaggio successivo, per impostazione predefinita. |
| `<defaultrmrootpathname>` | Specifica il nome dell'unità seguito da due punti. |
| seavailable | Specifica che un Gestione risorse transazionale preferisce la disponibilità rispetto alla coerenza. |
| con coerenza | Specifica che un Gestione risorse transazionale preferisce la coerenza rispetto alla disponibilità. |
| setlog | Modifica le caratteristiche di un Gestione risorse transazionale già in esecuzione. |
| growth | Specifica la quantità in base alla quale è possibile aumentare le dimensioni del log di Gestione risorse transazionale.<p>Il parametro Growth può essere specificato come indicato di seguito:<ul><li>Numero di contenitori, usando il formato:`<containers> containers`</li><li>Percentuale, usando il formato:`<percent> percent`</li></ul> |
| `<containers>` | Specifica gli oggetti dati utilizzati dalla Gestione risorse transazionale. |
| maxextent | Specifica il numero massimo di contenitori per il Gestione risorse transazionale specificato. |
| minextent | Specifica il numero minimo di contenitori per il Gestione risorse transazionale specificato. |
| modalità`{full|undo}` | Specifica se tutte le transazioni vengono registrate ( **complete**) o se solo gli eventi sottoposti a rollback vengono registrati (**Annulla**). |
| ridenominazione | Modifica il GUID per la Gestione risorse transazionale. |
| shrink | Specifica la percentuale in base alla quale il log di Gestione risorse transazionale può essere ridotto automaticamente. |
| size | Specifica le dimensioni del Gestione risorse transazionale come numero specificato di *contenitori*. |
| start | Avvia il Gestione risorse transazionale specificato. |
| stop | Arresta il Gestione risorse transazionale specificato. |

### <a name="examples"></a>Esempi

Per impostare il log per il Gestione risorse transazionale specificato da *c:\test*, per avere una crescita automatica di cinque contenitori, digitare:

```
fsutil resource setlog growth 5 containers c:test
```

Per impostare il log per il Gestione risorse transazionale specificato da *c:\test*, per avere una crescita automatica del due%, digitare:

```
fsutil resource setlog growth 2 percent c:test
```

Per specificare che la Gestione risorse transazionale predefinita eliminerà i metadati transazionali sul montaggio successivo sull'unità C, digitare:

```
fsutil resource setautoreset true c:\
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS transazionale](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730726(v=ws.10))
