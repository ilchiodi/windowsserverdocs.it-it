---
title: takeown
description: Informazioni su come ottenere l'accesso a un file diventando il proprietario del file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da43b13f0333f3a12a8763db4cad31e12283bb8b
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436196"
---
# <a name="takeown"></a>takeown

Consente agli amministratori di ripristinare l'accesso a un file che in precedenza è stato negato, rendendo l'amministratore proprietario del file.



## <a name="syntax"></a>Sintassi

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<> computer|Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. Questo parametro si applica a tutti i file e le cartelle specificati nel comando.|
|/u [ \< dominio>\]<User name>|Esegue lo script con le autorizzazioni dell'account utente specificato. Il valore predefinito è le autorizzazioni di sistema.|
|/p [ \< Password>]|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/f \< Nome File>|Specifica il nome file o il modello di nome di directory. È possibile usare il carattere jolly * quando si specifica il criterio. È anche possibile usare la sintassi *ShareName* \* filename *.|
|/a|Assegna la proprietà al gruppo Administrators anziché all'utente corrente.|
|/r|Esegue un'operazione ricorsiva su tutti i file nella directory e nelle sottodirectory specificate.|
|/d {Y \| N}|Disattiva la richiesta di conferma visualizzata quando l'utente corrente non dispone dell'autorizzazione "elenco cartelle" per una directory specificata e utilizza invece il valore predefinito specificato. I valori validi per l'opzione **/d** sono i seguenti:</br>-Y: assume la proprietà della directory.</br>-N: ignora la directory.</br>Si noti che è necessario usare questa opzione in combinazione con l'opzione **/r** .|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Questo comando viene in genere utilizzato nei file batch.
-   Se il parametro **/a** non è specificato, la proprietà del file viene assegnata all'utente attualmente connesso al computer.
-   Schemi misti con (**?** e **&#42;**) non sono supportati dal comando **takeown** .
-   Dopo l'eliminazione del blocco con **takeown**, potrebbe essere necessario utilizzare Esplora risorse o il comando **cacls** per concedere a se stessi le autorizzazioni complete per i file e le directory prima di poterli eliminare. Per ulteriori informazioni su **cacls**, vedere "riferimenti aggiuntivi" alla fine di questo argomento.

## <a name="examples"></a><a name="BKMK_examples"></a>Esempi

Per assumere la proprietà di un file denominato lostfile, digitare:
```
takeown /f lostfile
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)