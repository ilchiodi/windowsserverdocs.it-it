---
ms.assetid: b198d8ca-a5b7-430f-8911-5cbb9f50484c
title: Fsutil risorse
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: d4d2782a1d0ce4329feee03d6c3cb2e6b6a6dc55
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914663"
---
# <a name="fsutil-resource"></a>Fsutil risorse
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Crea un gestore di risorse transazionale secondario, avvia o arresta un gestore di risorse transazionale, o Visualizza informazioni su una gestione risorse di transazione e modifica il comportamento seguente:

-   Indica se un Gestione risorse transazionale predefinito eliminerà i metadati transazionali al montaggio successivo

-   Gestione risorse transazionale specificato per preferire la coerenza rispetto alla disponibilità

-   La transazione specificata Gestione risorse di preferire la disponibilità rispetto alla coerenza

-   Caratteristiche di un Gestione risorse transazionale in esecuzione

Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples) .

## <a name="syntax"></a>Sintassi

```
fsutil resource [create] <RmRootPathname>
fsutil resource [info] <RmRootPathname>
fsutil resource [setautoreset] {true|false} <DefaultRmRootPathname>
fsutil resource [setavailable] <RmRootPathname>
fsutil resource [setconsistent] <RmRootPathname>
fsutil resource [setlog] [growth {<Containers> containers|<Percent> percent} <RmRootPathname>] [maxextents <Containers> <RmRootPathname>] [minextents <Containers> <RmRootPathname>] [mode {full|undo} <RmRootPathname>] [rename <RmRootPathname>] [shrink <percent> <RmRootPathname>] [size <Containers> <RmRootPathname>]
fsutil resource [start] <RmRootPathname> [<RmLogPathname> <TmLogPathname>
fsutil resource [stop] <RmRootPathname>
```

### <a name="parameters"></a>Parametri

|        Parametro        |                                                                                                                                                                                                                                        Descrizione                                                                                                                                                                                                                                         |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         create          |                                                                                                                                                                                                                    Crea una Gestione risorse transazionale secondaria.                                                                                                                                                                                                                     |
|    <RmRootPathname>     |                                                                                                                                                                                                        Specifica il percorso completo di una directory radice Gestione risorse transazionale.                                                                                                                                                                                                         |
|          info           |                                                                                                                                                                                                            Consente di visualizzare le informazioni sul Gestione risorse transazionale specificato.                                                                                                                                                                                                            |
|      setautoreset       | Specifica se un Gestione risorse transazionale predefinito pulirà i metadati transazionali al montaggio successivo.<br /><br />-Impostare il parametro **setautoreset** su **true** per specificare che la transazione gestione risorse eliminerà i metadati transazionali al montaggio successivo, per impostazione predefinita.<br />-Impostare il parametro **setautoreset** su **false** per specificare che la transazione gestione risorse non eliminerà i metadati transazionali al montaggio successivo, per impostazione predefinita. |
| <DefaultRmRootPathname> |                                                                                                                                                                                                                       Specifica il nome dell'unità seguito da due punti.                                                                                                                                                                                                                        |
|      seavailable       |                                                                                                                                                                                                 Specifica che un Gestione risorse transazionale preferisce la disponibilità rispetto alla coerenza.                                                                                                                                                                                                 |
|      con coerenza      |                                                                                                                                                                                                 Specifica che un Gestione risorse transazionale preferisce la coerenza rispetto alla disponibilità.                                                                                                                                                                                                 |
|         setlog          |                                                                                                                                                                                                  Modifica le caratteristiche di un Gestione risorse transazionale già in esecuzione.                                                                                                                                                                                                  |
|         crescita          |                                                                                                  Specifica la quantità in base alla quale è possibile aumentare le dimensioni del log di Gestione risorse transazionale.<br /><br />Il parametro Growth può essere specificato come indicato di seguito:<br /><br />-Numero di contenitori che usano il formato: _Contenitori_ **contenitori**<br />-Percentuale usando il formato: _Percentuale_ di **percentuale** di                                                                                                   |
|      <containers>       |                                                                                                                                                                                                      Specifica gli oggetti dati utilizzati dalla Gestione risorse transazionale.                                                                                                                                                                                                       |
|        maxextent        |                                                                                                                                                                                                Specifica il numero massimo di contenitori per il Gestione risorse transazionale specificato.                                                                                                                                                                                                |
|        minextent        |                                                                                                                                                                                                Specifica il numero minimo di contenitori per il Gestione risorse transazionale specificato.                                                                                                                                                                                                |
|  modalità {Undo&#124;completo}  |                                                                                                                                                                                        Specifica se tutte le transazioni vengono registrate ( **complete**) o se solo gli eventi sottoposti a rollback vengono registrati (**Annulla**).                                                                                                                                                                                         |
|         rename          |                                                                                                                                                                                                                  Modifica il GUID per la Gestione risorse transazionale.                                                                                                                                                                                                                  |
|         shrink          |                                                                                                                                                                                              Specifica la percentuale in base alla quale il log di Gestione risorse transazionale può essere ridotto automaticamente.                                                                                                                                                                                              |
|          size           |                                                                                                                                                                                              Specifica le dimensioni del Gestione risorse transazionale come numero specificato di *contenitori*.                                                                                                                                                                                               |
|          start          |                                                                                                                                                                                                                    Avvia il Gestione risorse transazionale specificato.                                                                                                                                                                                                                    |
|          stop           |                                                                                                                                                                                                                    Arresta il Gestione risorse transazionale specificato.                                                                                                                                                                                                                     |

### <a name="BKMK_examples"></a>Esempi
Per impostare il log per il Gestione risorse transazionale specificato da c:\test, per avere una crescita automatica di cinque contenitori, digitare:

```
fsutil resource setlog growth 5 containers c:test
```

Per impostare il log per il Gestione risorse transazionale specificato da c:\test, per avere una crescita automatica del due%, digitare:

```
fsutil resource setlog growth 2 percent c:test
```

Per specificare che la Gestione risorse transazionale predefinita eliminerà i metadati transazionali sul montaggio successivo sull'unità C, digitare:

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[NTFS transazionale](https://go.microsoft.com/fwlink/?LinkID=165402)


