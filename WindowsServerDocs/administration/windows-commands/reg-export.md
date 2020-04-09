---
title: esportazione reg
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5901014511fed0c17a641e1ed183ddbf40dd44c8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836494"
---
# <a name="reg-export"></a>esportazione reg



Copia le sottochiavi, voci e valori del computer locale in un file per il trasferimento in altri server.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<nome della >|Specifica il percorso completo della sottochiave. L'operazione di esportazione funziona solo con il computer locale. Il nome di chiave deve includere una chiave radice valido. Le chiavi principali valide sono: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<FileName >|Specifica il nome e percorso del file deve essere creato durante l'operazione. Il file deve avere estensione reg.|
|/y|Sovrascrive tutti i file esistenti con il nome *FileName* senza chiedere conferma.|
|/?|Visualizza la Guida per **reg export** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg export** operazione.

|Valore|Descrizione|
|-----|-----------|
|0|Success|
|1|Operazione non riuscita|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per esportare il contenuto di tutte le sottochiavi e valori della chiave MyApp nel file AppBkUp. reg, digitare:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)