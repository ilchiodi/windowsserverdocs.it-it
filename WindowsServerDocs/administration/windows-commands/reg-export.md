---
title: esportazione reg
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fb3a779ffe5a4e7d513ca9a3afed8ee90901688
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384755"
---
# <a name="reg-export"></a>esportazione reg



Copia le sottochiavi, voci e valori del computer locale in un file per il trasferimento in altri server.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<KeyName >|Specifica il percorso completo della sottochiave. L'operazione di esportazione funziona solo con il computer locale. Il nome di chiave deve includere una chiave radice valido. Le chiavi radice valide sono: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<> FileName|Specifica il nome e percorso del file deve essere creato durante l'operazione. Il file deve avere estensione reg.|
|/y|Sovrascrive tutti i file esistenti con il nome *FileName* senza chiedere conferma.|
|/?|Visualizza la Guida per **reg export** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg export** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Errore|

## <a name="BKMK_examples"></a>Esempi

Per esportare il contenuto di tutte le sottochiavi e valori della chiave MyApp nel file AppBkUp. reg, digitare:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)