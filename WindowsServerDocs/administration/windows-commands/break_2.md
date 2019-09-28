---
title: break
description: "Argomento dei comandi di Windows per **break_2** : Annulla l'associazione di un volume di copia shadow a VSS e lo rende accessibile come volume normale."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5789e3442152c705b3197bf1ce5e63dc782a15c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379783"
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

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|scrivibile|Consente l'accesso in lettura/scrittura al volume.|
|\<SetID >|Specifica l'ID del set di copie shadow.|

## <a name="remarks"></a>Note

-   Per impostazione predefinita, i volumi esposti, come le copie shadow da cui provengono, sono di sola lettura.
-   L'alias dell'ID della copia shadow, archiviato come variabile di ambiente tramite il comando **Load Metadata** , può essere usato nel parametro *SetID* .

## <a name="BKMK_examples"></a>Esempi

Per creare una copia shadow con il nome alias Alias1 accessibile come volume scrivibile nel sistema operativo, digitare:
```
break writable %Alias1%
```

> [!NOTE]
> L'accesso al volume viene effettuato direttamente al provider hardware senza che sia stata registrata una copia shadow del volume.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)