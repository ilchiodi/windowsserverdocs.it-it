---
title: importazione Logman | esportazione
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81147f9e2e2da69c8e59969f3c176264a7fa353a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840674"
---
# <a name="logman-import--export"></a>importazione Logman | esportazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|   -s <computer name>    |                                                   Eseguire il comando nel computer remoto specificato.                                                   |
|     -config <value>     |                                                  Specifica il file di impostazioni che contiene le opzioni di comando.                                                  |
|       [-n] <name>       |                                                                Nome dell'oggetto di destinazione.                                                                 |
|       -<name> XML       |                                                         Nome del file XML per importare o esportare.                                                         |
|          -ets           |                                       Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.                                        |
| -u [-] < utente [password] > | Utente di Esegui come. L'immissione di un \* per la password genera una richiesta per la password. La password non viene visualizzata quando si digita. |
|           -y            |                                                      Rispondere Sì a tutte le domande senza chiedere conferma.                                                       |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Il comando seguente importa c:\windows\perf_log.xml il file XML da server_1 il computer come un agente di raccolta dati del set denominato perf_log.  
```  
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
[logman](logman.md)  
