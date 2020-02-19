---
title: gpupdate
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f73917721fca7b623501b521ad095d40ab98e33
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465415"
---
# <a name="gpupdate"></a>gpupdate

Aggiorna le impostazioni di criteri di gruppo. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#examples).

## <a name="syntax"></a>Sintassi

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parametri

|     Parametro     |                                                                                                                                                                                                                                                                                                                             Descrizione                                                                                                                                                                                                                                                                                                                             |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /target: {computer\|utente} | Specifica che vengono aggiornate solo le impostazioni dei criteri solo utente o solo computer. Per impostazione predefinita, vengono aggiornate le impostazioni dei criteri utente e computer.                                                                                                                                                                                                                                                                                                                                |
|      /Force       |                                                                                                                                                                                                                                                                                   Riapplica tutte le impostazioni di criteri. Per impostazione predefinita, vengono applicate solo le impostazioni di criteri che sono stati modificati.                                                                                                                                                                                                                                                                                    |
|  /Wait: valore\<>   | Imposta il numero di secondi di attesa per completare la prima di tornare al prompt dei comandi di elaborazione dei criteri. Quando viene superato il limite di tempo, viene visualizzato il prompt dei comandi, ma continua l'elaborazione di criteri. Il valore predefinito è 600 secondi. Il valore **0** significa che non è in attesa. Il valore **-1** indica un'attesa infinita.</br>In uno script, tramite questo comando con un limite di tempo specificato, è possibile eseguire **gpupdate** e continuare con i comandi che non dipendono dal completamento di **gpupdate**. In alternativa, è possibile utilizzare questo comando senza alcun limite di tempo specificato per consentire **gpupdate** fine dell'esecuzione prima di eseguono altri comandi che dipendono da esso. |
|      /Logoff      |                                                                                                                                   Forza la disconnessione dopo le impostazioni di criteri di gruppo vengono aggiornate. Ciò è necessario per le estensioni lato client Criteri di gruppo che non elaborano i criteri in un ciclo di aggiornamento in background ma eseguono tale operazione quando un utente accede. Sono esempi di installazione del Software e reindirizzamento cartelle destinati agli utenti. Questa opzione non ha alcun effetto se non esistono estensioni che richiedono la disconnessione.                                                                                                                                    |
|       /Boot       |                                                                                                                                       Provoca un riavvio del computer dopo aver applicate le impostazioni di criteri di gruppo. Ciò è necessario per le estensioni lato client Criteri di gruppo che non elaborano i criteri in un ciclo di aggiornamento in background ma eseguono tale operazione all'avvio del computer. Esempi di installazione Software basata su computer di destinazione. Questa opzione non ha alcun effetto se non esistono estensioni che richiedono un riavvio.                                                                                                                                        |
|       /Sync       |                                                                                                                                                                              Fa sì che l'applicazione dei criteri in primo piano successiva da eseguire in modo sincrono. L'applicazione dei criteri viene applicata al computer avvio e l'accesso. È possibile specificare questo per l'utente, computer o entrambi, utilizzando il **/destinazione** parametro. Il **/force** e **/attesa** i parametri vengono ignorati se vengono specificate.                                                                                                                                                                               |
|        /?         |                                                                                                                                                                                                                                                                                                                Visualizza la Guida dal prompt dei comandi.                                                                                                                                                                                                                                                                                                                 |

## <a name="remarks"></a>Note

-   Il comando **gpupdate** è disponibile in windows Server 2008 R2, windows Server 2008, Windows 7 Ultimate, Windows 7 Professional, Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.

## <a name="examples"></a>Esempi

Forzare un aggiornamento in background di tutte le impostazioni di criteri di gruppo, indipendentemente dal fatto che siano modificati.

```
gpupdate /force
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Criteri di gruppo TechCenter](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
