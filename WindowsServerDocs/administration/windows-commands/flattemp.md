---
title: flattemp
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1a458e8742ca354eeca821e93590386bca56dff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377134"
---
# <a name="flattemp"></a>flattemp

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita o disabilita le cartelle temporanee.
Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/query|Richiede l'impostazione corrente.|
|/Enable|Abilita cartelle temporanee. Gli utenti condivideranno la cartella temporanea a meno che la cartella temporanea non si trovi nella home directory dell'utente.|
|/Disable|Disabilita cartelle temporanee. Ogni cartella temporanea dell'utente si trova in una cartella separata (determinata dall'ID di sessione dell'utente).|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il comando **flattemp** è disponibile solo se è stato installato il servizio ruolo Terminal Server in un computer che esegue windows Server 2008 o il servizio ruolo Host sessione Desktop remoto in un computer che esegue windows Server 2008 R2.
-   È necessario disporre di credenziali amministrative per eseguire **flattemp**.
-   Dopo ogni utente dispone di una cartella temporanea univoca, utilizzare **flattemp /enable** per abilitare le cartelle temporanee.
-   Il metodo predefinito per la creazione di cartelle temporanee per più utenti (in genere indicate dalle variabili di ambiente TEMP e TMP) consiste nel creare sottocartelle di **\Temp** cartella, utilizzando l'ID di accesso come il nome della sottocartella. Ad esempio, se la variabile di ambiente TEMP punta a C:\Temp, la cartella temporanea assegnata per l'ID di accesso utente 4 è c:\Temp\4. Utilizzando **flattemp**, è possibile puntare direttamente alla cartella \Temp e impedire che le sottocartelle che compongono. Questa operazione è utile quando si desidera che le cartelle temporanee dell'utente siano contenute in cartelle Home, in un'unità locale del server Host sessione Desktop remoto o in un'unità di rete condivisa. Si consiglia di utilizzare il **flattemp /enable** comando solo quando ogni utente dispone di una cartella temporanea separata.
-   Se la cartella temporanea dell'utente è un'unità di rete, potrebbero verificarsi errori dell'applicazione. Ciò si verifica quando l'unità di rete condivise diventa temporaneamente inaccessibile in rete. Poiché i file temporanei dell'applicazione sono inaccessibili o all'esterno di sincronizzazione, il servizio risponde come se il disco è stato arrestato. Non è consigliabile spostare la cartella temporanea in un'unità di rete. Il valore predefinito è di mantenere le cartelle temporanee sul disco rigido locale. Se si verificano comportamenti imprevisti o errori di danneggiamento del disco con determinate applicazioni, stabilizzare la rete o riportare le cartelle temporanee per il disco rigido locale.
-   Se si disabilita l'utilizzo di cartelle temporanee separate per sessione, le impostazioni **flattemp** vengono ignorate. Questa opzione è impostata nello strumento di configurazione di Servizi Desktop remoto.

## <a name="BKMK_examples"></a>Esempi
-   Per visualizzare l'impostazione corrente per le cartelle temporanee, digitare:
    ```
    flattemp /query
    ```
-   Per abilitare le cartelle temporanee, digitare:
    ```
    flattemp /enable
    ```
-   Per disattivare le cartelle temporanee, digitare:
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Guida &#40;di riferimento&#41; ai comandi di Servizi Desktop remoto Servizi terminal](remote-desktop-services-terminal-services-command-reference.md)
