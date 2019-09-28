---
title: carico reg
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db661e311e3fe8c393750716de5dab375e7817f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384704"
---
# <a name="reg-load"></a>carico reg



Scritture salvata sottochiavi e le voci in una sottochiave diversa nel Registro di sistema. Deve essere utilizzato con i file temporanei utilizzati per la risoluzione dei problemi o modificare le voci del Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
reg load KeyName FileName
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<KeyName >|Specifica il percorso completo della sottochiave da caricare. Per specificare i computer remoti, includere il nome del computer (nel formato \\ @ no__t-1ComputerName @ no__t-2 come parte del *nome*della pagina. Se si omette \\ @ no__t-1ComputerName \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *KeyName* deve includere una chiave radice valido. Le chiavi radice valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi radice valide sono: HKLM e HKU.|
|\<> FileName|Specifica il nome e percorso del file da caricare. Questo file deve essere creato in anticipo usando il **reg salvare** operazione e l'estensione hiv.|
|/?|Visualizza la Guida per **Carico reg** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **Carico reg** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Errore|

## <a name="BKMK_examples"></a>Esempi

Per caricare il file TempHive HKLM\TempHive la chiave, digitare:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)