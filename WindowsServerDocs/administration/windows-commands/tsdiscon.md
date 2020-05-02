---
title: tsdiscon
description: Argomento di riferimento per tsdiscon, che disconnette una sessione da un server Host sessione Desktop remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2a97d1b157445fd43acce5a80f3d793ed5ae5af
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721257"
---
# <a name="tsdiscon"></a>tsdiscon

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disconnette una sessione da un server host sessione Desktop remoto.



> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|\<> SessionId|Specifica l'ID di sessione da disconnettere.|
|\<Sessionname>|Specifica il nome della sessione da disconnettere.|
|/Server:\<nomeserver>|Specifica il server terminal che contiene la sessione da disconnettere. In caso contrario, viene utilizzato il server Host sessione Desktop remoto corrente.|
|/v|Visualizza le informazioni sulle azioni eseguite.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
-   È necessario disporre dell'autorizzazione controllo completo o disconnettere autorizzazioni di accesso speciali per disconnettere un altro utente da una sessione.
-   Se non viene specificato alcun ID di sessione o nome della sessione, **tsdiscon** disconnette la sessione corrente.
-   Tutte le applicazioni in eseguono al momento disconnessa la sessione eseguono automaticamente quando si riconnette alla sessione senza alcuna perdita di dati. Utilizzare **reimpostazione sessione** per terminare le applicazioni in esecuzione della sessione disconnessa, ma tenere presente che ciò potrebbe comportare la perdita di dati nella sessione.
-   Il **/server** parametro è obbligatorio solo se si utilizza **tsdiscon** da un server remoto.
-   Impossibile disconnettere la sessione della console.

## <a name="examples"></a>Esempi
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
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
  - [Guida di](command-line-syntax-key.md)
  riferimento ai comandi servizi desktop remoto della chiave della sintassi della riga di comando[(Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
