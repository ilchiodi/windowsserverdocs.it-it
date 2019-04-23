---
title: Reg unload
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaa7d7a9fa82db2968d988e3b7b3fb8275a72337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834982"
---
# <a name="reg-unload"></a>Reg unload



Rimuove una sezione del Registro di sistema che è stato caricato tramite il **Carico reg** operazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
reg unload <KeyName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome chiave >|Specifica il percorso completo della sottochiave da scaricare. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\nomecomputer\) come parte del *KeyName*. L'omissione \\ \\ComputerName\ fa sì che l'operazione per impostazione predefinita nel computer locale. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono HKLM e HKU.|
|/?|Visualizza la Guida per **reg unload** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg unload** (opzione).

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Operazione non riuscita|

## <a name="BKMK_examples"></a>Esempi

Per scaricare l'hive TempHive in HKLM il file, digitare:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Non modificare direttamente il Registro di sistema a meno che non vi siano altre alternative. L'editor del Registro di sistema consente di ignorare le protezioni standard, che consente di impostazioni che possono influire negativamente sulle prestazioni, il sistema danneggiato o anche richiedere la reinstallazione di Windows. È possibile modificare la maggior parte delle impostazioni del Registro di sistema in modo sicuro usando i programmi nel Pannello di controllo o in Microsoft Management Console (MMC). Se è necessario modificare direttamente il Registro di sistema, eseguirne il backup.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)