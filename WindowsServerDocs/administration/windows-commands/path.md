---
title: path
description: Informazioni su come impostare la variabile di ambiente PATH.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65ccaf23b0e19319383952f3a1ca436aaf4d06fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856462"
---
# <a name="path"></a>path



Imposta il percorso del comando nella variabile di ambiente PATH (il set di directory utilizzato per cercare i file eseguibili). Se utilizzata senza parametri, **percorso** consente di visualizzare il percorso del comando corrente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Drive>:]<Path>|Specifica l'unità e directory da impostare nel percorso del comando.|
|;|Consente di separare le directory nel percorso del comando. Se usato senza altri parametri **;** Cancella i percorsi di comandi esistenti dalla variabile di ambiente PATH e indirizza Cmd.exe per la ricerca solo nella directory corrente.|
|% PATH %|Aggiunge il percorso del comando al set esistente di directory elencate nella variabile di ambiente PATH.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Quando si include **% PATH %** nella sintassi Cmd.exe lo sostituisce con i valori di percorso di comandi presenti nella variabile di ambiente PATH, eliminando la necessità di immettere manualmente i valori seguenti al prompt dei comandi.
-   La directory corrente viene sempre cercata prima la directory specificata nel percorso del comando.
-   È possibile che i file in una directory che condividono lo stesso nome ma avere estensioni diverse. Ad esempio, potrebbe essere un file conti.com che avvia un programma di contabilità e un altro file denominato Accnt.bat che collega il server alla rete di sistema di contabilità.

    Il sistema operativo Windows cerca un file con estensioni di file predefinito nel seguente ordine di precedenza: .exe, com,. bat e. cmd. Per eseguire Accnt.bat quando conti.com esiste nella stessa directory, è necessario includere l'estensione. bat al prompt dei comandi.
-   Se due o più file nel percorso del comando hanno lo stesso nome di file e l'estensione **percorso** assegnare un nome file specificato viene cercato innanzitutto nella directory corrente. Quindi cerca le directory nel percorso del comando nell'ordine in cui sono elencati nella variabile di ambiente PATH.
-   Se si inseriscono le **percorso** comando nel file Autoexec, il sistema operativo Windows aggiunge automaticamente il percorso di ricerca del sottosistema MS-DOS specificato ogni volta che si accede al computer in uso. Cmd.exe non usa il file Autoexec. Quando avviata da un collegamento, Cmd.exe eredita le variabili di ambiente impostate in risorse del Computer/proprietà/avanzate/ambiente.

## <a name="BKMK_examples"></a>Esempi

Per cercare i percorsi per i comandi esterni C:\User\Taxes B:\User\Invest e B:\Bin, digitare:

`path c:\user\taxes;b:\user\invest;b:\bin`

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)