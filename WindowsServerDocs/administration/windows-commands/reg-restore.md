---
title: ripristino reg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d490f99f032b38c8bbbe9352b8571b4a85202e1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722517"
---
# <a name="reg-restore"></a>ripristino reg



Le voci e sottochiavi scritture salvate nuovamente nel Registro di sistema.



## <a name="syntax"></a>Sintassi

```
Reg restore <KeyName> <FileName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome>|Specifica il percorso completo della sottochiave da ripristinare. L'operazione di ripristino funziona solo con il computer locale. Il nome di chiave deve includere una chiave radice valido. Le chiavi principali valide sono: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<> FileName|Specifica il nome e percorso del file con contenuto da scrivere nel Registro di sistema. Questo file deve essere creato in anticipo con il **reg salvare** operazione utilizzando l'estensione hiv.|
|/?|Visualizza la Guida per **ripristino reg** al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Prima di modificare le voci del Registro di sistema, salvare la sottochiave padre con il **reg salvare** operazione. Se la modifica non riesce, ripristinare la sottochiave originale con la **ripristino reg** operazione.
-   Nella tabella seguente sono elencati i valori restituiti per il **ripristino reg** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per ripristinare il file NTRKBkUp nella chiave HKLM\Software\Microsoft\ResKit e sovrascrivere il contenuto esistente della chiave, digitare:
```
REG RESTORE HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)