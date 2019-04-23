---
title: break
description: "Argomento i comandi di Windows per **break_2** : rimuove l'associazione di un volume copia shadow da VSS e lo rende accessibile come un volume normale."
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 49516539ae603e2c93b3fc395c77786be790d663
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886332"
---
# <a name="break"></a>break



Rimuove l'associazione di un volume copia shadow da VSS e la rende accessibile come un volume normale. Il volume è quindi accessibile con una lettera di unità (se assegnato) o il nome del volume. Se utilizzata senza parametri, **interruzione** Visualizza la Guida al prompt dei comandi.

> [!NOTE]
> Questo comando è rilevante solo per le copie shadow hardware dopo l'importazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
break [writable] <SetID>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[scrivibile]|Abilita accesso in lettura/scrittura nel volume.|
|\<SetID>|Specifica l'ID del set di copie shadow.|

## <a name="remarks"></a>Note

-   I volumi esposti, come le copie shadow provengano da, sono di sola lettura per impostazione predefinita.
-   L'alias dell'ID copia shadow, che viene archiviata come una variabile di ambiente per il **caricare i metadati** comando, può essere usato nel *SetID* parametro.

## <a name="BKMK_examples"></a>Esempi

Per rendere un'ombreggiatura copiare con il nome dell'alias Alias1 accessibile come un volume accessibile in scrittura nel sistema operativo, digitare:
```
break writable %Alias1%
```

> [!NOTE]
> Accedere al volume viene eseguito direttamente il provider hardware senza record di essere stata una copia shadow del volume.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)