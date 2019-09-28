---
title: ripristino reg
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6c121256cecaebc26e2c402d9b9ced8890eddc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384669"
---
# <a name="reg-restore"></a>ripristino reg



Le voci e sottochiavi scritture salvate nuovamente nel Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg restore <KeyName> <FileName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<KeyName >|Specifica il percorso completo della sottochiave da ripristinare. L'operazione di ripristino funziona solo con il computer locale. Il nome di chiave deve includere una chiave radice valido. Le chiavi radice valide sono: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<> FileName|Specifica il nome e percorso del file con contenuto da scrivere nel Registro di sistema. Questo file deve essere creato in anticipo con il **reg salvare** operazione utilizzando l'estensione hiv.|
|/?|Visualizza la Guida per **ripristino reg** al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Prima di modificare le voci del Registro di sistema, salvare la sottochiave padre con il **reg salvare** operazione. Se la modifica non riesce, ripristinare la sottochiave originale con la **ripristino reg** operazione.
-   Nella tabella seguente sono elencati i valori restituiti per il **ripristino reg** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Errore|

## <a name="BKMK_examples"></a>Esempi

Per ripristinare il file NTRKBkUp nella chiave HKLM\Software\Microsoft\ResKit e sovrascrivere il contenuto esistente della chiave, digitare:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)