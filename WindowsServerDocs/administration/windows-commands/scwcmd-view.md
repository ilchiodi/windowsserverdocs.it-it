---
title: Visualizzazione scwcmd
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb38d5100eab74573d5f5ffb4ec684b2b19c3bbb
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820941"
---
# <a name="scwcmd-view"></a>Scwcmd: visualizzazione

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Esegue il rendering di un file XML utilizzando una trasformazione XSL specificato. Questo comando può essere utile per visualizzare i file XML di configurazione guidata impostazioni di Sicurezza usando diverse visualizzazioni.

## <a name="syntax"></a>Sintassi

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/x: \< XMLFile. xml>|Specifica il file XML da visualizzare. Questo parametro deve essere specificato.|
|/s: \< filexsl. xsl>|Specifica la trasformazione XSL da applicare al file con estensione XML come parte del processo di rendering. Questo parametro è facoltativo per i file XML di Sicurezza. Quando il **visualizzazione** comando viene utilizzato per eseguire il rendering di un file XML di Sicurezza, si tenterà automaticamente di caricare la trasformazione predefinito corretto per il file con estensione XML specificato. Se si specifica una trasformazione XSL, la trasformazione deve essere scritto in base al presupposto che il file XML nella stessa directory come la trasformazione XSL.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Esempi

Per visualizzare Policyfile.xml utilizzando la trasformazione Policyview.xsl, digitare:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)