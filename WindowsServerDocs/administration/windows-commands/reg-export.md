---
title: esportazione reg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c697595d5d19c953ef85f7a2e334c6fe05329d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722557"
---
# <a name="reg-export"></a>esportazione reg



Copia le sottochiavi, voci e valori del computer locale in un file per il trasferimento in altri server.



## <a name="syntax"></a>Sintassi

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome>|Specifica il percorso completo della sottochiave. L'operazione di esportazione funziona solo con il computer locale. Il nome di chiave deve includere una chiave radice valido. Le chiavi principali valide sono: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<> FileName|Specifica il nome e percorso del file deve essere creato durante l'operazione. Il file deve avere estensione reg.|
|/y|Sovrascrive tutti i file esistenti con il nome *FileName* senza chiedere conferma.|
|/?|Visualizza la Guida per **reg export** al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Nella tabella seguente sono elencati i valori restituiti per il **reg export** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per esportare il contenuto di tutte le sottochiavi e valori della chiave MyApp nel file AppBkUp. reg, digitare:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)