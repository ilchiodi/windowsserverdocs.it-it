---
title: REG Salva
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a46dfe081421ed727bd7ffeeab364e6c23dd801
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841082"
---
# <a name="reg-save"></a>REG Salva



Salva una copia di sottochiavi, voci e valori del Registro di sistema in un file specificato.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome chiave >|Specifica il percorso completo della sottochiave. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\nomecomputer\) come parte del *KeyName*. L'omissione \\ \\ComputerName\ fa sì che l'operazione per impostazione predefinita nel computer locale. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|\<FileName>|Specifica il nome e percorso del file che viene creato. Se viene specificato alcun percorso, viene utilizzato il percorso corrente.|
|/y|Sovrascrive un file esistente con il nome *FileName* senza chiedere conferma.|
|/?|Visualizza la Guida per **reg salvare** al prompt dei comandi.|

## <a name="remarks-optional-section"></a>La sezione Osservazioni \<sezione facoltativa >

-   Nella tabella seguente sono elencati i valori restituiti per il **reg salvare** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Operazione non riuscita|
-   Prima di modificare le voci del Registro di sistema, salvare la sottochiave padre con il **reg salvare** operazione. Se la modifica non riesce, ripristinare la sottochiave originale con la **ripristino reg** operazione.

## <a name="BKMK_examples"></a>Esempi

Per salvare il file hive MyApp nella cartella corrente come un file denominato AppBkUp. HIV, digitare:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)