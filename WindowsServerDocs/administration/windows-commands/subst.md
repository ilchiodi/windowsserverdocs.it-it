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
ms.openlocfilehash: 43cbc57aba29ea0b9150dccdfc566a93017a09a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833644"
---
# <a name="subst"></a>subst



Associa un percorso con una lettera di unità. Se utilizzata senza parametri, **subst** vengono visualizzati i nomi delle unità virtuali attive.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> unità1 \<:|Specifica l'unità virtuale a cui si desidera assegnare un percorso.|
|[\<unità2 >:]\<percorso >|Specifica l'unità fisica e il percorso che si desidera assegnare a un'unità virtuale.|
|/d|Elimina un'unità sostituita (virtuale).|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)