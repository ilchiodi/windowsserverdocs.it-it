---
title: subst
description: Informazioni su come associare un percorso a una lettera di unità.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62ba0de33e69998e7d3e343b1e53c1de7e630e10
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721615"
---
# <a name="subst"></a>subst



Associa un percorso con una lettera di unità. Se utilizzata senza parametri, **subst** vengono visualizzati i nomi delle unità virtuali attive.



## <a name="syntax"></a>Sintassi

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità1:|Specifica l'unità virtuale a cui si desidera assegnare un percorso.|
|[\<Unità2>:] \<> percorso|Specifica l'unità fisica e il percorso che si desidera assegnare a un'unità virtuale.|
|/d|Elimina un'unità sostituita (virtuale).|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   I comandi seguenti non funzionano e non devono essere usati nelle unità specificate nel comando **SUBST** :

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   Il *unità1* parametro deve essere compreso nell'intervallo specificato dal **lastdrive** comando. In caso contrario, **subst** Visualizza il messaggio di errore seguente:

    `Invalid parameter - drive1:`

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi

Per creare un'unità virtuale Z per il percorso B:\User\Betty\Forms, digitare:
```
subst z: b:\user\betty\forms 
```
Anziché digitare il percorso completo, è possibile raggiungere questa directory digitando la lettera dell'unità virtuale seguita da due punti come indicato di seguito:
```
z: 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)