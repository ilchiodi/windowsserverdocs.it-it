---
title: break
description: Argomento dei comandi di Windows per break_2, che annulla l'associazione di un volume di copia shadow da VSS e lo rende accessibile come volume normale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6683c44c84f4baae5f016f7df62d5bd6591cff70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848254"
---
# <a name="break"></a>break

Annulla l'associazione di un volume di copia shadow a VSS e lo rende accessibile come volume normale. È quindi possibile accedere al volume utilizzando una lettera di unità (se assegnata) o il nome del volume. Se utilizzata senza parametri, **Interrompi** Visualizza la guida al prompt dei comandi.

> [!NOTE]
> Questo comando è pertinente solo per le copie shadow dell'hardware dopo l'importazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
break [writable] <SetID>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|scrivibile|Consente l'accesso in lettura/scrittura al volume.|
|\<> SetId|Specifica l'ID del set di copie shadow.|

## <a name="remarks"></a>Note

-   Per impostazione predefinita, i volumi esposti, come le copie shadow da cui provengono, sono di sola lettura.
-   L'alias dell'ID della copia shadow, archiviato come variabile di ambiente tramite il comando **Load Metadata** , può essere usato nel parametro *SetID* .

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per creare una copia shadow con il nome alias Alias1 accessibile come volume scrivibile nel sistema operativo, digitare:
```
break writable %Alias1%
```

> [!NOTE]
> L'accesso al volume viene effettuato direttamente al provider hardware senza che sia stata registrata una copia shadow del volume.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)