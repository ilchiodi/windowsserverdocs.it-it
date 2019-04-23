---
title: subst
description: Informazioni su come associare un percorso con una lettera di unità.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 858195de89ca8661cf47c25b6cf9b519cc4efbf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858072"
---
# <a name="subst"></a>subst



Associa un percorso con una lettera di unità. Se utilizzata senza parametri, **subst** vengono visualizzati i nomi delle unità virtuali attive.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive1>:|Specifica l'unità virtuale a cui si desidera assegnare un percorso.|
|[\<Drive2>:]\<Path>|Specifica l'unità fisica e il percorso che si desidera assegnare a un'unità virtuale.|
|/d|Elimina un'unità sostituita (virtuale).|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   I comandi seguenti non funzionano e non deve essere utilizzati nelle unità specificate nel **subst** comando:

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   Il *unità1* parametro deve essere compreso nell'intervallo specificato dal **lastdrive** comando. In caso contrario, **subst** Visualizza il messaggio di errore seguente:

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>Esempi

Per creare un'unità virtuale Z per il percorso B:\User\Betty\Forms, digitare:
```
subst z: b:\user\betty\forms 
```
Anziché digitare il percorso completo, è possibile raggiungere questa directory digitando la lettera dell'unità virtuale seguita da due punti come indicato di seguito:
```
z: 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)