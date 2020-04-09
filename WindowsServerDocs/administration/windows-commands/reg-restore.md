---
title: ripristino reg
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e511694247c04f2befc9c0148498e43b85f664ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836364"
---
# <a name="reg-restore"></a>ripristino reg



Le voci e sottochiavi scritture salvate nuovamente nel Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg restore <KeyName> <FileName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<nome della >|Specifica il percorso completo della sottochiave da ripristinare. L'operazione di ripristino funziona solo con il computer locale. Il nome di chiave deve includere una chiave radice valido. Le chiavi principali valide sono: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<FileName >|Specifica il nome e percorso del file con contenuto da scrivere nel Registro di sistema. Questo file deve essere creato in anticipo con il **reg salvare** operazione utilizzando l'estensione hiv.|
|/?|Visualizza la Guida per **ripristino reg** al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Prima di modificare le voci del Registro di sistema, salvare la sottochiave padre con il **reg salvare** operazione. Se la modifica non riesce, ripristinare la sottochiave originale con la **ripristino reg** operazione.
-   Nella tabella seguente sono elencati i valori restituiti per il **ripristino reg** operazione.

|Valore|Descrizione|
|-----|-----------|
|0|Success|
|1|Operazione non riuscita|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per ripristinare il file NTRKBkUp nella chiave HKLM\Software\Microsoft\ResKit e sovrascrivere il contenuto esistente della chiave, digitare:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)