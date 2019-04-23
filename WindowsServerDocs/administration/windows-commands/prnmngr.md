---
title: prnmngr
description: Informazioni su come aggiungere, eliminare ed elencare le stampanti e connessioni.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 2c0aa44cc6f27e553bf8c1b57356b884bc0cd632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887202"
---
# <a name="prnmngr"></a>prnmngr

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge, Elimina e sono elencate le stampanti o le connessioni alle stampanti, oltre all'impostazione e la visualizzazione della stampante predefinita.

## <a name="syntax"></a>Sintassi
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|-a|Aggiunge una connessione stampante locale.|
|-d|Elimina una connessione alla stampante.|
|-x|Elimina tutte le stampanti dal server specificato con il **-s** parametro. Se non si specifica un server, Windows elimina tutte le stampanti nel computer locale.|
|-g|Visualizza la stampante predefinita.|
|-t|Imposta la stampante predefinita sulla stampante specificata per il **-p** parametro.|
|-l|Elenca tutte le stampanti installate nel server specificato per il **-s** parametro. Se non si specifica un server, Windows elenca le stampanti installate nel computer locale.|
|c|Specifica che il parametro deve essere applicato per le connessioni alle stampanti. Può essere usato con il **- un** e **- x** parametri.|
|-s <ServerName>|Specifica il nome del computer remoto che ospita la stampante che si desidera gestire. Se non si specifica un computer, viene utilizzato il computer locale.|
|-p \<nomestampante >|Specifica il nome della stampante che si desidera gestire.|
|-m \<DrivermodelName >|Specifica (nome) del driver da installare. I driver vengono spesso denominati per il modello di stampante che supportano. Vedere la documentazione della stampante per altre informazioni.|
|-r \<PortName >|Specifica la porta in cui è connesso alla stampante. Se si tratta di una porta seriale o parallelo, usare l'ID della porta (ad esempio, LPT1: o COM1:). Se si tratta di una porta TCP/IP, usare il nome della porta che è stato specificato quando è stata aggiunta la porta.|
|-u \<UserName > -w \<Password >|Specifica un account con autorizzazioni sufficienti per connettersi al computer che ospita la stampante che si desidera gestire. Tutti i membri del gruppo Administrators locale del computer di destinazione dispongono di queste autorizzazioni, ma è anche possibile concedere le autorizzazioni ad altri utenti. Se non si specifica un account, è necessario essere connessi con un account con queste autorizzazioni per il comando lavorare.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   Il **prndrvr** comando è uno script Visual Basic che si trova nel %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Per usare questo comando, un prompt dei comandi, digitare **cscript** aggiungendo il percorso completo per il **prnmngr** file oppure passare alla directory nella cartella appropriata. Ad esempio:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Se le informazioni fornite contengono spazi, utilizzare le virgolette intorno al testo (ad esempio, `"computer Name"`).

## <a name="BKMK_examples"></a>Esempi
Per aggiungere una stampante denominata StampanteColori_2 che è connesso a LPT1 nel computer locale e richiede un driver della stampante denominato colore stampante Driver1, digitare:
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Per eliminare la stampante denominata del computer remoto denominato ServerRU StampanteColori_2, digitare:
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[riferimenti ai comandi di stampa](print-command-reference.md)
