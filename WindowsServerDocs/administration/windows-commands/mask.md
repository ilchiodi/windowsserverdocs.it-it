---
title: maschera
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1cb92caacb955449c1baaad411fdbe4cdf05b73
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839644"
---
# <a name="mask"></a>maschera



Rimuove le copie shadow dell'hardware importate tramite il comando **Import** .

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
mask <ShadowSetID>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|ShadowSetID|Rimuove le copie shadow che appartengono all'ID del set di copie shadow specificato.|

## <a name="remarks"></a>Note

-   È possibile usare un alias esistente o una variabile di ambiente al posto di *ShadowSetID*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per rimuovere la copia shadow importata% Import_1%, digitare:
```
mask %Import_1%
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)