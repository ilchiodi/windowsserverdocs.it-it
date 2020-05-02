---
title: reg Salva
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd3e932e67df7eb972bd625ecec24f986cf3f3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722507"
---
# <a name="reg-save"></a>reg Salva



Salva una copia di sottochiavi, voci e valori del Registro di sistema in un file specificato.



## <a name="syntax"></a>Sintassi

```
reg save <KeyName> <FileName> [/y]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome>|Specifica il percorso completo della sottochiave. Per specificare i computer remoti, includere il nome del computer ( \\ \\nel formato\) ComputerName come parte del *nome*della finestra di. \\ \\Se si omette nomecomputer \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *nome* chiave deve includere una chiave radice valida. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|\<> FileName|Specifica il nome e percorso del file che viene creato. Se viene specificato alcun percorso, viene utilizzato il percorso corrente.|
|/y|Sovrascrive un file esistente con il nome *FileName* senza chiedere conferma.|
|/?|Visualizza la Guida per **reg salvare** al prompt dei comandi.|

## <a name="remarks-optional-section"></a>Sezione \<facoltativa osservazioni>

-   Nella tabella seguente sono elencati i valori restituiti per il **reg salvare** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|
-   Prima di modificare le voci del Registro di sistema, salvare la sottochiave padre con il **reg salvare** operazione. Se la modifica non riesce, ripristinare la sottochiave originale con la **ripristino reg** operazione.

## <a name="examples"></a>Esempi

Per salvare il file hive MyApp nella cartella corrente come un file denominato AppBkUp. HIV, digitare:
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)