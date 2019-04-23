---
title: takeown
description: Informazioni su come ottenere l'accesso a un file da diventare il proprietario del file.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b5a4874edf9fa4406d4643e686fed2b725699dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854362"
---
# <a name="takeown"></a>takeown

Consente agli amministratori di ripristinare l'accesso a un file che in precedenza è stato negato, rendendo l'amministratore proprietario del file.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<computer >|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). L'impostazione predefinita è il computer locale. Questo parametro si applica a tutti i file e cartelle specificate nel comando.|
|/u [\<Domain>\]<User name>|Esegue lo script con le autorizzazioni dell'account utente specificato. Il valore predefinito è le autorizzazioni di sistema.|
|/p [\<Password>]|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/f \<nome file >|Specifica il nome di file o directory pattern. È possibile usare il carattere jolly * quando si specifica il modello. È anche possibile usare la sintassi *ShareName*\*FileName *.|
|/a|Assegna la proprietà al gruppo Administrators anziché sull'utente corrente.|
|/r|Esegue un'operazione ricorsiva su tutti i file nella directory specificata e le sottodirectory.|
|/d {Y \| N}|Elimina la richiesta di conferma che viene visualizzata quando l'utente corrente non dispone dell'autorizzazione "Visualizzazione contenuto cartella" in una directory specificata e Usa invece il valore predefinito specificato. I valori validi per il **/d** opzione sono i seguenti:</br>-Y: Assumere la proprietà della directory.</br>-   N: Ignorare le directory.</br>Si noti che è necessario utilizzare questa opzione in combinazione con il **/r** opzione.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Questo comando viene in genere utilizzato nei file batch.
-   Se il **/a** parametro viene omesso, la proprietà del file fornita per l'utente attualmente connesso al computer.
-   Usando i modelli misti (**?** e **&#42;**) non sono supportate da **takeown** comando.
-   Dopo l'eliminazione del blocco con **takeown**, potrebbe essere necessario usare Windows Explorer o nella **cacls** comando per concedere manualmente autorizzazioni complete per i file e directory prima che è possibile eliminarle. Per altre informazioni sulle **cacls**, vedere "Riferimenti aggiuntivi" alla fine di questo argomento.

## <a name="BKMK_examples"></a>Esempi

Per diventare proprietario di un file denominato filed, digitare:
```
takeown /f lostfile
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)