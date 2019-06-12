---
title: tsdiscon
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1b5fca329864ebed9eab66671a17493f0fc3ca8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440916"
---
# <a name="tsdiscon"></a>tsdiscon

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disconnette una sessione da un server Host sessione Desktop remoto (Host sessione rd).
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per scoprire quali sono le novità nella versione più recente, vedere [novità in Servizi Desktop remoto in Windows Server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|\<SessionId>|Specifica l'ID di sessione da disconnettere.|
|\<SessionName >|Specifica il nome della sessione da disconnettere.|
|/server:\<ServerName>|Specifica il server terminal che contiene la sessione da disconnettere. In caso contrario, viene usato il server Host sessione Desktop remoto corrente.|
|/v|Consente di visualizzare informazioni sulle azioni eseguibili in esecuzione.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   È necessario disporre dell'autorizzazione controllo completo o disconnettere autorizzazioni di accesso speciali per disconnettere un altro utente da una sessione.
-   Se non viene specificato alcun ID di sessione o il nome della sessione, **tsdiscon** disconnette la sessione corrente.
-   Tutte le applicazioni in eseguono al momento disconnessa la sessione eseguono automaticamente quando si riconnette alla sessione senza alcuna perdita di dati. Utilizzare **reimpostazione sessione** per terminare le applicazioni in esecuzione della sessione disconnessa, ma tenere presente che ciò potrebbe comportare la perdita di dati nella sessione.
-   Il **/server** parametro è obbligatorio solo se si utilizza **tsdiscon** da un server remoto.
-   Impossibile disconnettere la sessione della console.

## <a name="BKMK_examples"></a>Esempi
- Per disconnettere la sessione corrente, digitare:
  ```
  tsdiscon
  ```
- Per disconnettere la sessione 10, digitare:
  ```
  tsdiscon 10
  ```
- Per disconnettere la sessione TERM04, digitare:
  ```
  tsdiscon TERM04
  ```
  #### <a name="additional-references"></a>Altri riferimenti
  [Chiave sintassi della riga di comando](command-line-syntax-key.md)
  [Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)
