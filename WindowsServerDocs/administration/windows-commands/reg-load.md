---
title: carico reg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2523876e2ea2305ede3289226c934c9352825ba9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722545"
---
# <a name="reg-load"></a>carico reg



Scritture salvata sottochiavi e le voci in una sottochiave diversa nel Registro di sistema. Deve essere utilizzato con i file temporanei utilizzati per la risoluzione dei problemi o modificare le voci del Registro di sistema.



## <a name="syntax"></a>Sintassi

```
reg load KeyName FileName
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome>|Specifica il percorso completo della sottochiave da caricare. Per specificare i computer remoti, includere il nome del computer ( \\ \\nel formato\) ComputerName come parte del *nome*della finestra di. \\ \\Se si omette nomecomputer \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *nome* chiave deve includere una chiave radice valida. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|\<> FileName|Specifica il nome e percorso del file da caricare. Questo file deve essere creato in anticipo usando il **reg salvare** operazione e l'estensione hiv.|
|/?|Visualizza la Guida per **Carico reg** al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Nella tabella seguente sono elencati i valori restituiti per il **Carico reg** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per caricare il file TempHive HKLM\TempHive la chiave, digitare:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)