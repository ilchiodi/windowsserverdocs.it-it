---
title: change port
description: Argomento di riferimento per il comando change port, che elenca o modifica i mapping delle porte COM in modo che siano compatibili con le applicazioni MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dcf1097ea037aff9269edafea6e640054a697e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716073"
---
# <a name="change-port"></a>change port

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca o modifica i mapping delle porte COM per essere compatibile con le applicazioni MS-DOS.

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
change port [<portX>=<portY| /d <portX | /query]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
|-----------------|----------------------------------------|
| <portX>=<portY> | Esegue il `<*portX*>` mapping di com a`<*portY*>` |
| /d<portX> | Elimina il mapping per COM`<*portX*>` |
| /query | Consente di visualizzare i mapping delle porte correnti. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- La maggior parte delle applicazioni MS-DOS supporta solo la COM1 tramite porte seriali COM4. Il comando **Change Port** esegue il mapping di una porta seriale a un numero di porta diverso, consentendo alle app che non supportano porte com con numero elevato di accedere alla porta seriale. Il rimapping funziona solo per la sessione corrente e non viene mantenuto se si disconnette da una sessione di e quindi si esegue di nuovo l'accesso.

- Utilizzare **Change Port** senza parametri per visualizzare le porte com disponibili e i relativi mapping correnti.

## <a name="examples"></a>Esempi

- Per eseguire il mapping di COM12 a COM1 per l'uso da parte di un'applicazione basata su MS-DOS, digitare:
  
  ```
  change port com12=com1
  ```

- Per visualizzare i mapping delle porte correnti, digitare:
  
  ```
  change port /query
  ```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Change](change.md)

- [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
