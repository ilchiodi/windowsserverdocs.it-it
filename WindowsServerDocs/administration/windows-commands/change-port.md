---
title: change port
description: Argomento dei comandi di Windows per Change Port, che elenca o modifica i mapping delle porte COM in modo che siano compatibili con le applicazioni MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39273382038edb7709f2d99baea8090d71df3a57
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848074"
---
# <a name="change-port"></a>change port

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elenca o modifica i mapping delle porte COM per essere compatibile con le applicazioni MS-DOS.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi

```
change port [<PortX>=<PortY| /d <PortX| /query]
```

### <a name="parameters"></a>Parametri


|    Parametro    |              Descrizione               |
|-----------------|----------------------------------------|
| <PortX>=<PortY> | Esegue il mapping di < COM*portax*> a <*PortaY*> |
|   /d <PortX>    | Elimina il mapping per < COM*portax*> |
|     /query      | Visualizza i mapping delle porte correnti |
|       /?        | Visualizza la guida al prompt dei comandi |

## <a name="remarks"></a>Note

- La maggior parte delle applicazioni MS-DOS supporta solo la COM1 tramite porte seriali COM4. Il comando **Change Port** esegue il mapping di una porta seriale a un numero di porta diverso, consentendo alle applicazioni che non supportano porte com con numero elevato di accedere alla porta seriale. il rimapping funziona solo per la sessione corrente e non viene mantenuto se si disconnette da una sessione di e quindi si esegue di nuovo l'accesso.

- Utilizzare **Change Port** senza parametri per visualizzare le porte com disponibili e i relativi mapping correnti.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

- Per eseguire il mapping di COM12 a COM1 per l'uso da parte di un'applicazione basata su MS-DOS, digitare:
  ```
  change port com12=com1
  ```
- Per visualizzare i mapping delle porte correnti, digitare:
  ```
  change port /query
  ```

### <a name="additional-references"></a>Altre informazioni di riferimento
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
- [change](change.md)
- [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
