---
title: tsdiscon
description: Windows Commands argomento per tsdiscon, che disconnette una sessione da un server Host sessione Desktop remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b008fa920290043b64e7421e91a545123634f1e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832514"
---
# <a name="tsdiscon"></a>tsdiscon

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disconnette una sessione da un server host sessione Desktop remoto.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|\<SessionId >|Specifica l'ID di sessione da disconnettere.|
|\<sessionname >|Specifica il nome della sessione da disconnettere.|
|/Server:\<ServerName >|Specifica il server terminal che contiene la sessione da disconnettere. In caso contrario, viene utilizzato il server Host sessione Desktop remoto corrente.|
|/v|Visualizza le informazioni sulle azioni eseguite.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   È necessario disporre dell'autorizzazione controllo completo o disconnettere autorizzazioni di accesso speciali per disconnettere un altro utente da una sessione.
-   Se non viene specificato alcun ID di sessione o nome della sessione, **tsdiscon** disconnette la sessione corrente.
-   Tutte le applicazioni in eseguono al momento disconnessa la sessione eseguono automaticamente quando si riconnette alla sessione senza alcuna perdita di dati. Utilizzare **reimpostazione sessione** per terminare le applicazioni in esecuzione della sessione disconnessa, ma tenere presente che ciò potrebbe comportare la perdita di dati nella sessione.
-   Il **/server** parametro è obbligatorio solo se si utilizza **tsdiscon** da un server remoto.
-   Impossibile disconnettere la sessione della console.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
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
  ## <a name="additional-references"></a>Altre informazioni di riferimento
  - Guida di riferimento alla [chiave della sintassi della riga di comando](command-line-syntax-key.md)
  [Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
