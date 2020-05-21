---
title: pnputil
description: Informazioni su come gestire l'archivio driver con l'utilità pnputil. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4c6bcb138e8bd7308c01c2c53fba83b69362298a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436366"
---
# <a name="pnputil"></a>pnputil

Pnputil. exe è un'utilità della riga di comando che è possibile utilizzare per gestire l'archivio driver. È possibile utilizzare PnPUtil per aggiungere pacchetti driver, rimuovere pacchetti driver ed elencare i pacchetti driver presenti nell'archivio.

## <a name="syntax"></a>Sintassi

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-a|Specifica di aggiungere il file INF identificato.|
|-d|Specifica di eliminare il file INF identificato.|
|-E|Specifica di enumerare tutti i file INF di terze parti.|
|-f|Specifica di forzare l'eliminazione del file INF identificato. Non può essere usato in combinazione con il parametro **– i** .|
|-i|Specifica di installare il file INF identificato. Non può essere usato in combinazione con il parametro **-f** .|
|/?|Visualizza la guida al prompt dei comandi.|


## <a name="examples"></a>Esempi

-   pnputil. exe-a a:\usbcam\USBCAM. INF aggiunge il file INF specificato da USBCAM. INF
-   pnputil. exe-c:\Drivers \* . inf aggiunge tutti i file inf in c:\Drivers\
-   pnputil. exe-i-a a:\usbcam\USBCAM. INF aggiunge e installa il driver specificato.
-   pnputil. exe – e enumera tutti i driver di terze parti.
-   pnputil. exe-d Oem0. inf Elimina l'oggetto specificato.
-   pnputil. exe-f-d Oem0. inf forza l'eliminazione del file INF specificato.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Popd](popd.md)