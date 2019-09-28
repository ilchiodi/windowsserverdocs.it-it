---
title: pnputil
description: Informazioni su come gestire l'archivio driver con l'utilità pnputil. exe.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f20c60bfd9ae33497dd356c7797b9fb1d2b51d18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372287"
---
# <a name="pnputil"></a>pnputil

Pnputil. exe è un'utilità della riga di comando che è possibile utilizzare per gestire l'archivio driver. È possibile utilizzare PnPUtil per aggiungere pacchetti driver, rimuovere pacchetti driver ed elencare i pacchetti driver presenti nell'archivio.

## <a name="syntax"></a>Sintassi

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-a|Specifica di aggiungere il file INF identificato.|
|-d|Specifica di eliminare il file INF identificato.|
|-e|Specifica di enumerare tutti i file INF di terze parti.|
|-f|Specifica di forzare l'eliminazione del file INF identificato. Non può essere usato in combinazione con il parametro **– i** .|
|-i|Specifica di installare il file INF identificato. Non può essere usato in combinazione con il parametro **-f** .|
|/?|Visualizza la guida al prompt dei comandi.|


## <a name="examples"></a>Esempi

-   pnputil. exe-a a:\usbcam\USBCAM. INF aggiunge il file INF specificato da USBCAM. INF
-   pnputil. exe: un drivers\*.inf c:\ aggiunge tutti i file INF in c:\Drivers\
-   pnputil. exe-i-a a:\usbcam\USBCAM. INF aggiunge e installa il driver specificato.
-   pnputil. exe – e enumera tutti i driver di terze parti.
-   pnputil. exe-d Oem0. inf Elimina l'oggetto specificato.
-   pnputil. exe-f-d Oem0. inf forza l'eliminazione del file INF specificato.

## <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Popd](popd.md)