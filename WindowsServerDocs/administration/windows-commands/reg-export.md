---
title: Reg export
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7aeddb4b069b1baf5b8f7aaea2730a2b25bdad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889652"
---
# <a name="reg-export"></a>Reg export



Copia le sottochiavi, voci e valori del computer locale in un file per il trasferimento in altri server.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome chiave >|Specifica il percorso completo della sottochiave. L'operazione di esportazione funziona solo con il computer locale. Il nome di chiave deve includere una chiave radice valido. Le chiavi principali valide sono: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<FileName>|Specifica il nome e percorso del file deve essere creato durante l'operazione. Il file deve avere estensione reg.|
|/y|Sovrascrive tutti i file esistenti con il nome *FileName* senza chiedere conferma.|
|/?|Visualizza la Guida per **reg export** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg export** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Operazione non riuscita|

## <a name="BKMK_examples"></a>Esempi

Per esportare il contenuto di tutte le sottochiavi e valori della chiave MyApp nel file AppBkUp. reg, digitare:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)