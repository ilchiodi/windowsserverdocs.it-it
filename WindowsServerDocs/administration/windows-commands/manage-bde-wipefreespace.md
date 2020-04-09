---
title: Manage-bde WipeFreeSpace
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a9995c6872380af61bec5d3b200e733c034ea6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839714"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde: WipeFreeSpace



Cancella lo spazio libero nel volume di rimozione di tutti i frammenti di dati presenti nello spazio. L'esecuzione di questo comando su un volume crittografato con il metodo di crittografia solo spazio usato fornisce lo stesso livello di protezione del metodo di crittografia del volume di crittografia completo. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Unità \<>|Rappresenta una lettera di unità seguita da due punti, un percorso GUID volume o un volume montato.|
|-Annulla|Annulla la cancellazione di spazio che è in corso.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|Nome \<>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **-w** cancellazione di comando per creare lo spazio disponibile sull'unità C.
```
manage-bde -w C:
```
Nell'esempio seguente viene illustrato l'utilizzo del comando **-w** con il parametro **-Cancel** per annullare la cancellazione dello spazio disponibile nell'unità C.
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)