---
title: importazione Logman | esportazione
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffc2e42f353352f69cf61dfb1f108a7d53cb7c4b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724367"
---
# <a name="logman-import--export"></a>importazione Logman | esportazione

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

importare un insieme agenti di raccolta dati da un file XML o esportare un insieme agenti di raccolta dati in un file XML.  

## <a name="syntax"></a>Sintassi  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
### <a name="parameters"></a>Parametri  

|        Parametro        |                                                                        Descrizione                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             Vengono visualizzate sensibile al contesto della Guida.                                                              |
|   -s<computer name>    |                                                   Eseguire il comando nel computer remoto specificato.                                                   |
|     -config <value>     |                                                  Specifica il file di impostazioni che contiene le opzioni di comando.                                                  |
|       [-n]<name>       |                                                                Nome dell'oggetto di destinazione.                                                                 |
|       -XML<name>       |                                                         Nome del file XML per importare o esportare.                                                         |
|          -ets           |                                       Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.                                        |
| -u [-] < utente [password] > | Utente di Esegui come. L'immissione \* di un oggetto per la password genera una richiesta per la password. La password non viene visualizzata quando si digita. |
|           -y            |                                                      Rispondere Sì a tutte le domande senza chiedere conferma.                                                       |

## <a name="examples"></a>Esempi  
Il comando seguente importa c:\windows\perf_log.xml il file XML da server_1 il computer come un agente di raccolta dati del set denominato perf_log.  
```  
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
