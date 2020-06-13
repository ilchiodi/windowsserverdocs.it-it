---
title: netcfg
description: Argomento di riferimento per il comando netcfg, che installa il Ambiente preinstallazione di Windows (WinPE), una versione leggera di Windows usata per distribuire le workstation.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a308441df55873b205972d703ec52f53345beb5
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721554"
---
# <a name="netcfg"></a>netcfg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installa il Ambiente preinstallazione di Windows (WinPE), una versione leggera di Windows usata per distribuire le workstation.

## <a name="syntax"></a>Sintassi

```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /v | Viene eseguito in modalità dettagliata (dettagliata). |
| /e | Usa le variabili di ambiente di manutenzione durante l'installazione e la disinstallazione. |
| /winpe | Installa TCP/IP, NetBIOS e Microsoft client per ambiente preinstallazione di Windows (WinPE). |
| /l | Fornisce il percorso del file INF. |
| /C | Fornisce la classe del componente da installare. **protocollo**, **servizio**o **client**. |
| /i | Fornisce l'ID del componente. |
| /s | Fornisce il tipo di componenti da visualizzare, incluso **\tA** per gli adapter o **n** per i componenti di rete. |
| /b | Consente di visualizzare i percorsi di associazione, se seguiti da una stringa contenente il nome del percorso. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempio

Per installare l' *esempio* di protocollo usando c:\oemdir\example.inf, digitare:

```
netcfg /l c:\oemdir\example.inf /c p /i example
```

Per installare il servizio *MS_Server* , digitare:

```
netcfg /c s /i MS_Server
```

Per installare TCP/IP, NetBIOS e Microsoft client per ambiente preinstallazione di Windows, digitare:

```
netcfg /v /winpe
```

Per visualizzare se *MS_IPX* componenti è installato, digitare:

```
netcfg /q MS_IPX
```

Per disinstallare *MS_IPX*componenti, digitare:

```
netcfg /u MS_IPX
```

Per visualizzare tutti i componenti .NET installati, digitare:

```
netcfg /s n
```

Per visualizzare i percorsi di binding contenenti *MS_TCPIP*, digitare:

```
netcfg /b ms_tcpip
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
