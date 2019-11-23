---
title: reg Salva
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6ae07cd3c90c51e7bd494bc6c35919680cde912a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371700"
---
# <a name="reg-save"></a>reg Salva



Salva una copia di sottochiavi, voci e valori del Registro di sistema in un file specificato.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<nome della >|Specifica il percorso completo della sottochiave. Per specificare i computer remoti, includere il nome del computer (nel formato \\\\ComputerName\) come parte del *nome*della pagina. Se si omette \\\\nomecomputer \ l'operazione viene impostata sul computer locale per impostazione predefinita. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|\<FileName >|Specifica il nome e percorso del file che viene creato. Se viene specificato alcun percorso, viene utilizzato il percorso corrente.|
|/y|Sovrascrive un file esistente con il nome *FileName* senza chiedere conferma.|
|/?|Visualizza la Guida per **reg salvare** al prompt dei comandi.|

## <a name="remarks-optional-section"></a>Osservazioni \<sezione facoltativa >

-   Nella tabella seguente sono elencati i valori restituiti per il **reg salvare** operazione.

|Valore|Descrizione|
|-----|-----------|
|0|Operazione completata con successo|
|1|Operazione non riuscita|
-   Prima di modificare le voci del Registro di sistema, salvare la sottochiave padre con il **reg salvare** operazione. Se la modifica non riesce, ripristinare la sottochiave originale con la **ripristino reg** operazione.

## <a name="BKMK_examples"></a>Esempi

Per salvare il file hive MyApp nella cartella corrente come un file denominato AppBkUp. HIV, digitare:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)