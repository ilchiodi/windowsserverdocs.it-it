---
title: gpupdate
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 61131d2bf253c66d93408bc66b78d1dca2502087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840592"
---
# <a name="gpupdate"></a>gpupdate



Aggiorna le impostazioni di criteri di gruppo. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/target:{Computer | Utente}|Aggiorna solo utente o solo le impostazioni dei criteri Computer.|
|/Force|Riapplica tutte le impostazioni di criteri. Per impostazione predefinita, vengono applicate solo le impostazioni di criteri che sono stati modificati.|
|/Wait:\<valore >|Imposta il numero di secondi di attesa per completare la prima di tornare al prompt dei comandi di elaborazione dei criteri. Quando viene superato il limite di tempo, viene visualizzato il prompt dei comandi, ma continua l'elaborazione di criteri. Il valore predefinito è 600 secondi. Il valore **0** significa che non è in attesa. Il valore **-1** indica un'attesa infinita.</br>In uno script, tramite questo comando con un limite di tempo specificato, è possibile eseguire **gpupdate** e continuare con i comandi che non dipendono dal completamento di **gpupdate**. In alternativa, è possibile utilizzare questo comando senza alcun limite di tempo specificato per consentire **gpupdate** fine dell'esecuzione prima di eseguono altri comandi che dipendono da esso.|
|/Logoff|Forza la disconnessione dopo le impostazioni di criteri di gruppo vengono aggiornate. Ciò è necessario per le estensioni lato client Criteri di gruppo che non elaborano i criteri in un ciclo di aggiornamento in background ma eseguono tale operazione quando un utente accede. Sono esempi di installazione del Software e reindirizzamento cartelle destinati agli utenti. Questa opzione non ha alcun effetto se non esistono estensioni che richiedono la disconnessione.|
|/Boot|Provoca un riavvio del computer dopo aver applicate le impostazioni di criteri di gruppo. Ciò è necessario per le estensioni lato client Criteri di gruppo che non elaborano i criteri in un ciclo di aggiornamento in background ma eseguono tale operazione all'avvio del computer. Esempi di installazione Software basata su computer di destinazione. Questa opzione non ha alcun effetto se non esistono estensioni che richiedono un riavvio.|
|/Sync|Fa sì che l'applicazione dei criteri in primo piano successiva da eseguire in modo sincrono. L'applicazione dei criteri viene applicata al computer avvio e l'accesso. È possibile specificare questo per l'utente, computer o entrambi, utilizzando il **/destinazione** parametro. Il **/force** e **/attesa** i parametri vengono ignorati se vengono specificate.|
|/?|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **gpupdate** comando è disponibile in Windows Server 2008 R2, Windows Server 2008, Windows 7 Ultimate, Windows 7 Professional, Windows Vista Ultimate, Windows Vista Enterprise e Windows Vista Business.

## <a name="BKMK_Examples"></a>Esempi

Forzare un aggiornamento in background di tutte le impostazioni di criteri di gruppo, indipendentemente dal fatto che siano modificati.
```
gpupdate /force
```

#### <a name="additional-references"></a>Altri riferimenti

-   [TechCenter di criteri di gruppo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)