---
title: Scaricamento reg
description: Argomento di riferimento per il comando reg unload, che rimuove una sezione del registro di sistema caricata utilizzando l'operazione di caricamento reg.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 029b9225f8a437be18c3056d97e153075d9df7c9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722498"
---
# <a name="reg-unload"></a>Scaricamento reg



Rimuove una sezione del Registro di sistema che è stato caricato tramite il **Carico reg** operazione.



## <a name="syntax"></a>Sintassi

```
reg unload <KeyName>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome>|Specifica il percorso completo della sottochiave da scaricare. Per specificare i computer remoti, includere il nome del computer ( \\ \\nel formato\) ComputerName come parte del *nome*della finestra di. \\ \\Se si omette nomecomputer \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *nome* chiave deve includere una chiave radice valida. Le chiavi principali valide per il computer locale sono HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono HKLM e HKU.|
|/?|Visualizza la Guida per **reg unload** al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Nella tabella seguente sono elencati i valori restituiti per il **reg unload** (opzione).

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per scaricare l'hive TempHive in HKLM il file, digitare:
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Non modificare direttamente il Registro di sistema a meno che non vi siano altre alternative. L'editor del registro di sistema ignora le misure di sicurezza standard, consentendo impostazioni che possono compromettere le prestazioni, danneggiare il sistema o anche richiedere la reinstallazione di Windows. È possibile modificare in modo sicuro la maggior parte delle impostazioni del registro di sistema utilizzando i programmi nel pannello di controllo o in Microsoft Management Console (MMC). Se è necessario modificare direttamente il Registro di sistema, eseguirne il backup.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando reg load](reg-load.md)