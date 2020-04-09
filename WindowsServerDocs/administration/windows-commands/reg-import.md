---
title: importazione reg
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0816297e837bbce91ca069e3506405cbdb53c51a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836424"
---
# <a name="reg-import"></a>importazione reg



Copia il contenuto di un file che contiene esportate le sottochiavi del Registro di sistema, le voci e valori nel Registro di sistema del computer locale.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg import FileName
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<FileName >|Specifica il nome e percorso del file di contenuto deve essere copiato nel Registro di sistema del computer locale. Questo file deve essere creato in anticipo tramite **reg export**.|
|/?|Visualizza la Guida per **reg import** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg import** operazione.

|Valore|Descrizione|
|-----|-----------|
|0|Success|
|1|Operazione non riuscita|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per importare le voci del Registro di sistema dal file AppBkUp, digitare:
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)