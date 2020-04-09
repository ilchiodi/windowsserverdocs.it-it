---
title: Scaricamento reg
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5fd1436ed1122a09eea11d358a3711aedddf2c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836274"
---
# <a name="reg-unload"></a>Scaricamento reg



Rimuove una sezione del Registro di sistema che è stato caricato tramite il **Carico reg** operazione.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
reg unload <KeyName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<nome della >|Specifica il percorso completo della sottochiave da scaricare. Per specificare i computer remoti, includere il nome del computer (nel formato \\\\ComputerName\) come parte del *nome*della pagina. Se si omette \\\\nomecomputer \ l'operazione viene impostata sul computer locale per impostazione predefinita. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono HKLM e HKU.|
|/?|Visualizza la Guida per **reg unload** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg unload** (opzione).

|Valore|Descrizione|
|-----|-----------|
|0|Success|
|1|Operazione non riuscita|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per scaricare l'hive TempHive in HKLM il file, digitare:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Non modificare direttamente il Registro di sistema a meno che non vi siano altre alternative. L'editor del registro di sistema ignora le misure di sicurezza standard, consentendo impostazioni che possono compromettere le prestazioni, danneggiare il sistema o anche richiedere la reinstallazione di Windows. È possibile modificare in modo sicuro la maggior parte delle impostazioni del registro di sistema utilizzando i programmi nel pannello di controllo o in Microsoft Management Console (MMC). Se è necessario modificare direttamente il Registro di sistema, eseguirne il backup.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)