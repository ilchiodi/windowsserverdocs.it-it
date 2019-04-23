---
title: goto
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1ad0190519d58bd879ae391f378d800760c204f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857532"
---
# <a name="goto"></a>goto



Indirizza cmd.exe a una riga con etichetta in un file batch. All'interno di un file batch, **goto** indirizza l'elaborazione del comando a una riga identificata da un'etichetta. Quando viene trovato l'etichetta, l'elaborazione continua a partire da quelli che iniziano nella riga successiva.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
goto <Label> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Label>|Specifica una stringa di testo che viene utilizzata come etichetta nel programma batch.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Utilizzo di estensioni di comando

    Se le estensioni comando sono abilitata (impostazione predefinita) e si utilizza il **goto** comando con etichetta di destinazione **: EOF**, di trasferire il controllo alla fine del file di script batch corrente e di chiudere il file di script batch senza definire un'etichetta. Quando si utilizza **goto** con il **: EOF** etichetta, è necessario inserire due punti prima l'etichetta. Ad esempio:  
    ```
    goto:EOF
    ```  
-   Utilizzo valido *etichetta* valori

    È possibile usare gli spazi nel *etichetta* parametro, ma è possibile includere altri separatori (ad esempio, un punto e virgola o segni di uguale).
-   Corrispondenza *etichetta* con l'etichetta nel programma batch

    Il *etichetta* valore specificato deve corrispondere a un'etichetta nel programma batch. L'etichetta all'interno del programma batch deve iniziare con due punti (:). Se una riga che inizia con due punti, viene considerato come un'etichetta e vengono ignorati tutti i comandi nella riga. Se il programma batch non contiene l'etichetta specificata in *etichetta*, il programma batch si interrompe e viene visualizzato il messaggio seguente:  
    ```
    Label not found
    ```  
-   Usando **goto** per operazioni condizionali

    È possibile usare **goto** con altri comandi per eseguire operazioni condizionali. Per ulteriori informazioni sull'utilizzo **goto** per operazioni condizionali, vedere il [Se](if.md) comando riferimento.

## <a name="BKMK_examples"></a>Esempi

Il programma batch seguente formatta un disco nell'unità come disco di sistema. Se l'operazione ha esito positivo, il **goto** comando indirizza l'elaborazione di **: fine** etichetta:
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Cmd](cmd.md)

[If](if.md)