---
title: netcfg
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4895928ffdd5d923d370f82e699d69f42c0f81a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838934"
---
# <a name="netcfg"></a>netcfg

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installa il Ambiente preinstallazione di Windows (WinPE), una versione leggera di Windows usata per distribuire le workstation.
## <a name="syntax"></a>Sintassi
```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/v|Esegui in **modalità dettagliata (** dettagliata)|
|/e|Usare le variabili di **ambiente** di manutenzione durante l'installazione e la disinstallazione|
|/winpe|Installa TCP/IP, NetBIOS e Microsoft client per ambiente preinstallazione di Windows (WinPE)|
|/l|Fornisce il **percorso** del inf|
|/c|Fornisce la **classe** del componente da installare. protocollo, servizio o client|
|/i|Fornisce l' **ID** componente|
|/s|Fornisce il tipo di componenti da **visualizzare**.<p>\ta = Adapters, n = NET Components|
|/ b|Consente di visualizzare i **percorsi di associazione**, se seguiti da una stringa contenente il nome del percorso.|
|/?|Visualizza la **Guida** al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Per installare l' *esempio* di protocollo usando c:\oemdir\example.inf:
```
netcfg /l c:\oemdir\example.inf /c p /i example
```
Per installare il servizio *MS_Server* :
```
netcfg /c s /i MS_Server
```
Per installare TCP/IP, NetBIOS e il client Microsoft per l'ambiente preinstallazione di Windows
```
netcfg /v /winpe
```
Per visualizzare se *MS_IPX* componenti è installato:
```
netcfg /q MS_IPX
```
Per disinstallare *MS_IPX*componenti:
```
netcfg /u MS_IPX
```
Per visualizzare tutti i componenti .NET installati:
```
netcfg /s n
```
Per visualizzare i percorsi di binding contenenti *MS_TCPIP*:
```
netcfg /b ms_tcpip
```
## <a name="additional-references"></a>Altre informazioni di riferimento
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
