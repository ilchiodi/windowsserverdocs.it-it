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
ms.openlocfilehash: b55063c3c5ea41b43573e6322b5efb36d2dad90e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828332"
---
# <a name="fsutil-resource"></a>Fsutil risorse
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Crea un gestore di risorse transazionale secondario, avvia o arresta un gestore di risorse transazionale, o Visualizza informazioni su una gestione risorse di transazione e modifica il comportamento seguente:

-   Indica se un gestore delle risorse predefinito eseguirà la pulizia dei relativi metadati transazionali al montaggio successivo

-   Gestione delle risorse transazionale specificato a preferire coerenza rispetto alla disponibilità

-   Il gestore delle risorse delle transazioni specificato per preferisce disponibilità coerenza

-   Le caratteristiche di un'esecuzione transazionale Gestione risorse

Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples) .

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

|Parametro|Descrizione|
|-------------|---------------|
|creazione|Crea un gestore di risorse transazionale secondario.|
|<RmRootPathname>|Specifica il percorso completo in una directory radice di gestore di risorse transazionale.|
|info|Visualizza le informazioni di specificato transazionale Resource Manager.|
|transazione serautoreset specifica|Specifica se un gestore delle risorse predefinito eseguirà la pulizia di metadati delle transazioni al montaggio successivo.<br /><br />-Impostare il **transazione serautoreset specifica** parametro per **true** per specificare che il gestore di risorse delle transazioni eseguirà la pulizia dei metadati transazionali nella montaggio successivo, per impostazione predefinita.<br />-Impostare il **transazione serautoreset specifica** parametro per **false** per specificare che il gestore di risorse delle transazioni non eseguirà la pulizia dei metadati transazionali nella montaggio successivo, per impostazione predefinita.|
|<DefaultRmRootPathname>|Specifica il nome di unità seguito da due punti.|
|setavailable|Specifica che un gestore di risorse transazionale verrà preferisce disponibilità coerenza.|
|setconsistent|Specifica che un gestore di risorse transazionale verrà preferire coerenza rispetto alla disponibilità.|
|SETLOG|Modifica le caratteristiche di una transazionale Gestione risorse che è già in esecuzione.|
|aumento delle dimensioni|Specifica la quantità mediante il quale il Registro di gestore di risorse transazionale può raggiungere.<br /><br />Il parametro di aumento delle dimensioni può essere specificato come indicato di seguito:<br /><br />-Numero di contenitori usando il formato: *I contenitori***contenitori**<br />:   percentuale utilizzando il formato: *%***Percentuale**|
|<containers>|Specifica gli oggetti dati che vengono usati da Gestione risorse transazionali.|
|maxextent|Specifica il numero massimo di contenitori per specificato transazionale Resource Manager.|
|minextent|Specifica il numero minimo di contenitori per specificato transazionale Resource Manager.|
|modalità {completo&#124;Annulla}|Specifica se tutte le transazioni vengono registrate ( **completa**) o solo eseguire il rollback vengono registrati gli eventi (**Annulla**).|
|rename|Modifica il GUID per la gestione delle risorse transazionale.|
|shrink|Specifica percentuale mediante il quale può automaticamente ridurre il log di gestore di risorse transazionale.|
|dimensioni|Specifica la dimensione di gestione risorse transazionali come un numero specificato di *contenitori*.|
|start|Avvia il gestore di risorse transazionale specificato.|
|stop|Arresta il gestore di risorse transazionale specificato.|

### <a name="BKMK_examples"></a>Esempi
Per impostare il log per la gestione delle risorse transazionale specificato da c:\test, per avere un aumento automatico di cinque contenitori, digitare:

```
fsutil resource setlog growth 5 containers c:test
```

Per impostare il log per la gestione delle risorse transazionale specificato da c:\test, per avere un aumento automatico di % 2%, digitare:

```
fsutil resource setlog growth 2 percent c:test
```

Per specificare che il gestore delle risorse predefinito eseguirà la pulizia dei metadati transazionali nella montaggio successivo nell'unità C, digitare:

```
fsutil resource setautoreset true c:\  
```

### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Transactional NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


