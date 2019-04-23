---
title: cd
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5f907b6162e6767820e23222e287b933397397d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861102"
---
# <a name="cd"></a>cd



Visualizza il nome di o modifiche della directory corrente. Se usato con una lettera di unità (ad esempio, `cd C:`), **cd** vengono visualizzati i nomi della directory corrente nell'unità specificata. Se utilizzata senza parametri, **cd** Visualizza unità corrente e la directory.

> [!NOTE]
> Questo comando è analogo a come le **chdir** comando.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d|Cambia l'unità corrente, nonché la directory corrente per un'unità.|
|\<Drive>:|Specifica l'unità per visualizzare o modificare (se diversa da quella corrente).|
|\<Path>|Specifica il percorso della directory che si desidera visualizzare o modificare.|
|[..]|Specifica che si desidera passare alla cartella padre.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Se sono abilitate le estensioni dei comandi, le condizioni seguenti si applicano per la **cd** comando:
-   Per usare la stessa combinazione come i nomi del disco viene convertita nella stringa di directory corrente. Ad esempio, `cd C:\TEMP` imposterebbe la directory corrente a C:\Temp, se in questo caso sul disco.
-   Gli spazi non vengono considerati come delimitatori, così *percorso* possono contenere spazi non racchiuso tra virgolette. Ad esempio:  
    ```
    cd username\programs\start menu
    ```  
    equivale a:  
    ```
    cd "username\programs\start menu"
    ```  
    Le virgolette sono necessarie, tuttavia, se le estensioni sono disabilitate.

Per disabilitare le estensioni dei comandi, digitare:
```
cmd /e:off
```

## <a name="BKMK_examples"></a>Esempi

La directory radice è la parte superiore della gerarchia di directory per un'unità. Per tornare alla directory radice, digitare:
```
cd\
```
Per modificare la directory predefinita in un'unità che è diversa da quello che si sta su, digitare:
```
cd [<Drive>:\[<Directory>]]
```
Per verificare la modifica della directory, digitare:
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)