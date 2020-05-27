---
title: Manage-bde WipeFreeSpace
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc252bd0d9d8227badb35bafea96575e37fca243
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820791"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde: WipeFreeSpace



Cancella lo spazio libero nel volume di rimozione di tutti i frammenti di dati presenti nello spazio. L'esecuzione di questo comando su un volume crittografato con il metodo di crittografia solo spazio usato fornisce lo stesso livello di protezione del metodo di crittografia del volume di crittografia completo.

## <a name="syntax"></a>Sintassi

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità|Rappresenta una lettera di unità seguita da due punti, un percorso GUID volume o un volume montato.|
|-Annulla|Annulla la cancellazione di spazio che è in corso.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare l'uso del comando **-w** per creare la cancellazione dello spazio disponibile nell'unità C.
```
manage-bde -w C:
```
Per illustrare l'uso del comando **-w** con il parametro **-Cancel** per annullare la cancellazione dello spazio disponibile nell'unità C.
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)