---
title: pnputil
description: Informazioni su come gestire l'archivio driver con l'utilità pnputil.exe.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5bde78d97be8def9f8594572869c34ef213db480
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862542"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe è un'utilità della riga di comando che è possibile usare per gestire l'archivio driver. Pnputil consente di aggiungere pacchetti driver, rimuovere i pacchetti di driver ed elencare i pacchetti driver nell'archivio.

## <a name="syntax"></a>Sintassi

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-a|Specifica per aggiungere il file INF identificato.|
|-d|Specifica di eliminare il file INF identificato.|
|-e|Specifica per enumerare tutti i file INF di terze parti.|
|-f|Specifica per forzare l'eliminazione del file INF identificato. Non può essere utilizzato in combinazione con il **– i** parametro.|
|-i|Specifica di installare il file INF identificato. Non può essere utilizzato in combinazione con il **-f** parametro.|
|/?|Visualizza la guida al prompt dei comandi.|


## <a name="examples"></a>Esempi

-   pnputil.exe - un a:\usbcam\USBCAM. INF aggiunge il file INF specificato da USBCAM. INF
-   -un c:\drivers pnputil.exe\*. inf aggiunge tutti i file INF in c:\drivers\
-   pnputil.exe -i - un a:\usbcam\USBCAM. INF aggiunge e installa i driver specificati.
-   pnputil.exe – e consente di enumerare tutti i driver di terze parti.
-   Oem0 -d pnputil.exe elimina l'oggetto specificato.
-   pnputil.exe -f -d oem0 forza l'eliminazione del file INF specificato.

## <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Popd](popd.md)