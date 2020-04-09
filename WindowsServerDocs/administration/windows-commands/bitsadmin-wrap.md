---
title: bitsadmin wrap
description: Argomento comandi di Windows per Bitsadmin wrap, che esegue il wrapping di qualsiasi riga di testo di output che si estende oltre il bordo all'estrema destra della finestra di comando alla riga successiva.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 009a0452f44c4944ae110ca6b9e0570793c32a72
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848754"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il wrapping di tutte le righe di output testo che si estende oltre il bordo all'estrema destra della finestra di comando nella riga successiva.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Wrap Job
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Specificare prima di altre opzioni. Per impostazione predefinita, tutte le opzioni, ad eccezione dell'opzione di [monitoraggio Bitsadmin](bitsadmin-monitor.md) , incapsulano l'output.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente recupera le informazioni per il processo denominato *myDownloadJob* e include l'output.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
