---
title: gpupdate
description: Argomento di riferimento per il comando gpupdate, che aggiorna le impostazioni di Criteri di gruppo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a2047d6beb515350a534343ca484bc2deeb50daa
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818741"
---
# <a name="gpupdate"></a>gpupdate

Aggiorna le impostazioni di criteri di gruppo.

## <a name="syntax"></a>Sintassi

```
gpupdate [/target:{computer | user}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| /target:`{computer|user}` | Specifica che vengono aggiornate solo le impostazioni dei criteri solo utente o solo computer. Per impostazione predefinita, vengono aggiornate le impostazioni dei criteri utente e computer. |
| /Force | Riapplica tutte le impostazioni di criteri. Per impostazione predefinita, vengono applicate solo le impostazioni di criteri che sono stati modificati. |
| /Wait`<VALUE>` | Imposta il numero di secondi di attesa per completare la prima di tornare al prompt dei comandi di elaborazione dei criteri. Quando viene superato il limite di tempo, viene visualizzato il prompt dei comandi, ma continua l'elaborazione di criteri. Il valore predefinito è 600 secondi. Il valore **0** significa che non è in attesa. Il valore **-1** indica un'attesa infinita.<p>In uno script, tramite questo comando con un limite di tempo specificato, è possibile eseguire **gpupdate** e continuare con i comandi che non dipendono dal completamento di **gpupdate**. In alternativa, è possibile utilizzare questo comando senza alcun limite di tempo specificato per consentire **gpupdate** fine dell'esecuzione prima di eseguono altri comandi che dipendono da esso. |
| /Logoff | Forza la disconnessione dopo le impostazioni di criteri di gruppo vengono aggiornate. Ciò è necessario per le estensioni lato client Criteri di gruppo che non elaborano i criteri in un ciclo di aggiornamento in background ma eseguono tale operazione quando un utente accede. Sono esempi di installazione del Software e reindirizzamento cartelle destinati agli utenti. Questa opzione non ha alcun effetto se non esistono estensioni che richiedono la disconnessione. |
| /Boot | Provoca un riavvio del computer dopo aver applicate le impostazioni di criteri di gruppo. Ciò è necessario per le estensioni lato client Criteri di gruppo che non elaborano i criteri in un ciclo di aggiornamento in background ma eseguono tale operazione all'avvio del computer. Esempi di installazione Software basata su computer di destinazione. Questa opzione non ha alcun effetto se non esistono estensioni che richiedono un riavvio. |
| /Sync | Fa sì che l'applicazione dei criteri in primo piano successiva da eseguire in modo sincrono. L'applicazione dei criteri viene applicata al computer avvio e l'accesso. È possibile specificare questo per l'utente, computer o entrambi, utilizzando il **/destinazione** parametro. Il **/force** e **/attesa** i parametri vengono ignorati se vengono specificate. |
| /? | Visualizza la Guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per forzare un aggiornamento in background di tutte le impostazioni di Criteri di gruppo, indipendentemente dal fatto che siano state modificate, digitare:

```
gpupdate /force
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)