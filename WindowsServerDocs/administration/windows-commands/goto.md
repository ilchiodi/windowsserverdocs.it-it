---
title: goto
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 928a9031a7f86261789676257afe95ffc3be8a99
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842564"
---
# <a name="goto"></a>goto



Indirizza cmd.exe a una riga con etichetta in un file batch. All'interno di un file batch, **goto** indirizza l'elaborazione del comando a una riga identificata da un'etichetta. Quando viene trovato l'etichetta, l'elaborazione continua a partire da quelli che iniziano nella riga successiva.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
goto <Label> 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Etichetta \<>|Specifica una stringa di testo che viene utilizzata come etichetta nel programma batch.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Utilizzo di estensioni di comando

    Se sono abilitate le estensioni dei comandi (impostazione predefinita) e si usa il comando **goto** con un'etichetta di destinazione **: EOF**, si trasferisce il controllo alla fine del file di script batch corrente e si chiude il file di script batch senza definire un'etichetta. Quando si utilizza **goto** con il **: EOF** etichetta, è necessario inserire due punti prima l'etichetta. Ad esempio,  
    ```
    goto:EOF
    ```  
-   Utilizzo di valori di *etichetta* validi

    È possibile usare gli spazi nel parametro *Label* , ma non è possibile includere altri separatori, ad esempio punti e virgola o segni di uguale.
-   *Etichetta* corrispondente con l'etichetta nel programma batch

    Il valore dell' *etichetta* specificato deve corrispondere a un'etichetta nel programma batch. L'etichetta all'interno del programma batch deve iniziare con due punti (:). Se una riga che inizia con due punti, viene considerato come un'etichetta e vengono ignorati tutti i comandi nella riga. Se il programma batch non contiene l'etichetta specificata in *etichetta*, il programma batch si interrompe e viene visualizzato il messaggio seguente:  
    ```
    Label not found
    ```  
-   Uso di **goto** per le operazioni condizionali

    È possibile utilizzare **goto** con altri comandi per eseguire operazioni condizionali. Per ulteriori informazioni sull'utilizzo **goto** per operazioni condizionali, vedere il [Se](if.md) comando riferimento.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Il programma batch seguente formatta un disco nell'unità come disco di sistema. Se l'operazione ha esito positivo, il **goto** comando indirizza l'elaborazione di **: fine** etichetta:
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Cmd](cmd.md)

[Se](if.md)