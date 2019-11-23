---
title: Visualizzazione scwcmd
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a69db16696f42950af97b62ba6f28c4083954137
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371195"
---
# <a name="scwcmd-view"></a>Scwcmd: visualizzazione

> Si applica a: Windows Server 2012 R2, Windows Server 2012

Esegue il rendering di un file XML utilizzando una trasformazione XSL specificato. Questo comando può essere utile per visualizzare i file XML di configurazione guidata impostazioni di Sicurezza usando diverse visualizzazioni.

## <a name="syntax"></a>Sintassi

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/x:\<XMLFile. XML >|Specifica il file XML da visualizzare. Questo parametro deve essere specificato.|
|/s:\<filexsl. xsl >|Specifica la trasformazione XSL da applicare al file con estensione XML come parte del processo di rendering. Questo parametro è facoltativo per i file XML di Sicurezza. Quando il **visualizzazione** comando viene utilizzato per eseguire il rendering di un file XML di Sicurezza, si tenterà automaticamente di caricare la trasformazione predefinito corretto per il file con estensione XML specificato. Se si specifica una trasformazione XSL, la trasformazione deve essere scritto in base al presupposto che il file XML nella stessa directory come la trasformazione XSL.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Scwcmd.exe è disponibile solo nei computer che eseguono Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Esempi

Per visualizzare Policyfile.xml utilizzando la trasformazione Policyview.xsl, digitare:
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)