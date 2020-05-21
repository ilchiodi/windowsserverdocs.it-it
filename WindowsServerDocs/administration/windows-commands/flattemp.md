---
title: flattemp
description: Argomento di riferimento per il comando flattemp, che Abilita o Disabilita le cartelle temporanee flat.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a30a3f7eb6ec56a499864116debfbb6c09756d34
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437226"
---
# <a name="flattemp"></a>flattemp

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita o disabilita le cartelle temporanee. Per eseguire questo comando, è necessario disporre delle credenziali amministrative.

> [!NOTE]
> Questo comando è disponibile solo se è stato installato il servizio ruolo host sessione Desktop remoto.

## <a name="syntax"></a>Sintassi

```
flattemp {/query | /enable | /disable}
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /query | Richiede l'impostazione corrente. |
| /Enable | Abilita cartelle temporanee. Gli utenti condivideranno la cartella temporanea a meno che la cartella temporanea non si trovi nella cartella principale dell'utente. |
| /Disable | Disabilita cartelle temporanee. Ogni cartella temporanea dell'utente si troverà in una cartella separata (determinata dall'ID sessione dell'utente). |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Quando ogni utente dispone di una cartella temporanea univoca, utilizzare `flattemp /enable` per abilitare le cartelle temporanee flat.

- Il metodo predefinito per la creazione di cartelle temporanee per più utenti (in genere indicate dalle variabili di ambiente TEMP e TMP) consiste nel creare sottocartelle di **\Temp** cartella, utilizzando l'ID di accesso come il nome della sottocartella. Ad esempio, se la variabile di ambiente TEMP punta a C:\Temp, la cartella temporanea assegnata per l'ID di accesso utente 4 è c:\Temp\4.

    Utilizzando **flattemp**, è possibile puntare direttamente alla cartella \Temp e impedire che le sottocartelle che compongono. Questa operazione è utile quando si desidera che le cartelle temporanee dell'utente siano contenute nelle Home Directory, in un'unità locale host sessione Desktop remoto server o in un'unità di rete condivisa. Usare il `flattemp /enable*` comando solo quando ogni utente dispone di una cartella temporanea separata.

- È possibile che si verifichino errori dell'app se la cartella temporanea dell'utente si trova in un'unità di rete. Ciò si verifica quando l'unità di rete condivise diventa temporaneamente inaccessibile in rete. Poiché i file temporanei dell'app sono inaccessibili o non sincronizzati, risponde come se il disco fosse stato interrotto. Non è consigliabile spostare la cartella temporanea in un'unità di rete. Il valore predefinito è di mantenere le cartelle temporanee sul disco rigido locale. Se si verificano comportamenti imprevisti o errori di danneggiamento del disco con determinate applicazioni, stabilizzare la rete o riportare le cartelle temporanee per il disco rigido locale.

- Se si utilizzano cartelle temporanee per sessione, **flattemp** le impostazioni vengono ignorate. Questa opzione è impostata nello strumento di configurazione di Servizi Desktop remoto.

### <a name="examples"></a>Esempi

Per visualizzare l'impostazione corrente per le cartelle temporanee, digitare:

```
flattemp /query
```

Per abilitare le cartelle temporanee, digitare:

```
flattemp /enable
```

Per disattivare le cartelle temporanee, digitare:

```
flattemp /disable
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

