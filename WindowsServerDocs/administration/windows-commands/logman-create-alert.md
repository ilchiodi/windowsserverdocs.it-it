---
title: Logman crea avviso
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37a13ab5623a295f96bde2f734bcb17e1eca2be9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437840"
---
# <a name="logman-create-alert"></a>Logman crea avviso

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creare un agente di raccolta dati di avviso.  

## <a name="syntax"></a>Sintassi  
```  
logman create alert <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parametri  

|                 Parametro                  |                                                                               Descrizione                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    Vengono visualizzate sensibile al contesto della Guida.                                                                     |
|             -s <computer name>             |                                                          Eseguire il comando nel computer remoto specificato.                                                          |
|              -config <value>               |                                                         Specifica il file di impostazioni che contiene le opzioni di comando.                                                         |
|                [-n] <name>                 |                                                                       Nome dell'oggetto di destinazione.                                                                        |
|          -u [-] < utente [password] >           | Specifica l'utente di Esegui come. Immettere un \* per la password produce una richiesta per la password. La password non viene visualizzata quando si digita. |
| -m < [avvio] [stop] [[start] [stop] [...]] > |                                                modificare per l'avvio manuale o arrestare anziché un'ora di inizio e di fine pianificata.                                                 |
|             -rf < [[hh:] mm:] ss >             |                                                        Eseguire l'agente di raccolta dati per il periodo di tempo specificato.                                                         |
|     -b < g/aaaa h:mm: ss [AM &#124; PM] >      |                                                              Avviare la raccolta dei dati all'ora specificata.                                                               |
|     -e < g/aaaa h:mm: ss [AM &#124; PM] >      |                                                               Terminare la raccolta dei dati all'ora specificata.                                                                |
|             -si < [[hh:] mm:] ss >             |                                                 Specifica l'intervallo di campionamento per raccolta dati dei contatori delle prestazioni.                                                  |
|           -o < percorso &#124; dsn! log >           |                                              Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL.                                               |
|                   -[-]r                    |                                                  Ripetere l'agente di raccolta dati ogni giorno alle ore di inizio specificato e di fine.                                                  |
|                   -[-]a                    |                                                                     aggiungere a un file di log esistente.                                                                     |
|                   -[-] Mostra                   |                                                                     Sovrascrivere un file di log esistente.                                                                     |
|        -v [-] < nnnnnn &#124; mmgghhmm >        |                                                   collegare le informazioni di controllo delle versioni dei file alla fine del nome file di log.                                                   |
|               -[-]rc <task>                |                                                         Eseguire il comando specificato ogni volta che il log viene chiuso.                                                          |
|              -max [-] <value>               |                                                 Dimensioni massime in MB o numero massimo di record di log di SQL.                                                  |
|           -cnf [-] < [[hh:] mm:] ss >           |     Quando viene specificata l'ora, creare un nuovo file quando è trascorso il tempo specificato. Quando non viene specificata l'ora, creare un nuovo file quando viene superata la dimensione massima.     |
|                     -y                     |                                                             Rispondere Sì a tutte le domande senza chiedere conferma.                                                              |
|               -cf <filename>               |                       Specifica il file di elenco di contatori delle prestazioni da raccogliere. Il file deve contenere un nome di contatore delle prestazioni per ogni riga.                        |
|                   -el [-]                   |                                                                Abilita o disabilita la segnalazione di Log eventi.                                                                 |
|     -th < soglia [soglia [...]] >      |                                                        Specificare i contatori e i relativi valori di soglia per un avviso.                                                        |
|              -RDC [-] <name>               |                                                     Specifica l'insieme agenti di raccolta dati per l'avvio quando viene generato un avviso.                                                      |
|               -[-]tn <task>                |                                                             Specifica l'attività da eseguire quando viene generato un avviso.                                                              |
|            -[-]targ <argument>             |                                               Specifica gli argomenti di attività da utilizzare con l'attività specificata tramite -tn.                                                |

## <a name="remarks"></a>Note  
Dove [-] è elencato, un ulteriore - Nega l'opzione.  
## <a name="BKMK_examples"></a>Esempi  
Il comando seguente crea un avviso denominato new_alert che viene attivata quando le prestazioni del contatore % Tempo processore nel gruppo di contatori processore (totale) supera il valore del contatore di 50.  
```  
logman create alert new_alert -th "\Processor(_Total)\% Processor time>50"  
```  
> [!NOTE]
> Il valore soglia definito si basa sul valore raccolto per il contatore, pertanto in questo esempio, il valore di 50 equivale al 50% tempo processore.  
> #### <a name="additional-references"></a>Riferimenti aggiuntivi  
> [logman](logman.md)  
