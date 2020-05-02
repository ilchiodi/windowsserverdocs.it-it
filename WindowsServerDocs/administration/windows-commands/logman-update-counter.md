---
title: Logman update contatore
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 606dd6cb8d391f99b5050a28105e8bd32540becd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724300"
---
# <a name="logman-update-counter"></a>Logman update contatore

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiornare le proprietà di un esistente contatore agente di raccolta dati.  

## <a name="syntax"></a>Sintassi  
```  
logman update counter <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parametri  

|                    Parametro                     |                                                                               Descrizione                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Vengono visualizzate sensibile al contesto della Guida.                                                                     |
|                -s<computer name>                |                                                          Eseguire il comando nel computer remoto specificato.                                                          |
|                 -config <value>                  |                                                         Specifica il file di impostazioni che contiene le opzioni di comando.                                                         |
|                   [-n]<name>                    |                                                                       Nome dell'oggetto di destinazione.                                                                        |
| -f < bin & #124; bincirc & #124; csv & #124; tsv & #124; sql > |                                                            Specifica il formato di log per l'agente di raccolta dati.                                                             |
|             -u [-] < utente [password] >              | Specifica l'utente di Esegui come. L'immissione \* di un oggetto per la password genera una richiesta per la password. La password non viene visualizzata quando si digita. |
|    -m < [avvio] [stop] [[start] [stop] [...]] >    |                                                modificare l'avvio o l'arresto manuale anziché un'ora di inizio o di fine pianificata.                                                 |
|                -rf < [[hh:] mm:] ss >                |                                                        Eseguire l'agente di raccolta dati per il periodo di tempo specificato.                                                         |
|        -b < g/aaaa h:mm: ss [AM & #124; PM] >         |                                                              Avviare la raccolta dei dati all'ora specificata.                                                               |
|        -e < g/aaaa h:mm: ss [AM & #124; PM] >         |                                                               Terminare la raccolta dei dati all'ora specificata.                                                                |
|                -si < [[hh:] mm:] ss >                |                                                 Specifica l'intervallo di campionamento per raccolta dati dei contatori delle prestazioni.                                                  |
|              -o < percorso & #124; dsn! log >              |                                              Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL.                                               |
|                      -[-]r                       |                                                  Ripetere l'agente di raccolta dati ogni giorno alle ore di inizio specificato e di fine.                                                  |
|                      -[-]a                       |                                                                     Accoda a un file di log esistente.                                                                     |
|                      -[-] Mostra                      |                                                                     Sovrascrivere un file di log esistente.                                                                     |
|           -v [-] < nnnnnn & #124; mmgghhmm >           |                                                   alleghi le informazioni sul controllo delle versioni dei file alla fine del nome del file di log.                                                   |
|                  -[-] RC<task>                   |                                                         Eseguire il comando specificato ogni volta che il log viene chiuso.                                                          |
|                 -max [-] <value>                  |                                                 Dimensioni massime in MB o numero massimo di record di log di SQL.                                                  |
|              -cnf [-] < [[hh:] mm:] ss >              |     Quando viene specificata l'ora, creare un nuovo file quando è trascorso il tempo specificato. Quando non viene specificata l'ora, creare un nuovo file quando viene superata la dimensione massima.     |
|                        -y                        |                                                             Rispondere Sì a tutte le domande senza chiedere conferma.                                                              |
|                  -CF<filename>                  |                       Specifica il file di elenco di contatori delle prestazioni da raccogliere. Il file deve contenere un nome di contatore delle prestazioni per ogni riga.                        |
|               -c <percorso [path []] >               |                                                              Specifica i contatori delle prestazioni per raccogliere.                                                               |
|                   -sc <value>                    |                                      Specifica il numero massimo di campioni da raccogliere con un agente di raccolta dati del contatore delle prestazioni.                                      |

## <a name="remarks"></a>Osservazioni  
Dove [-] è elencato, un ulteriore - Nega l'opzione.  
## <a name="examples"></a>Esempi  
Il seguente comando Aggiorna perf_log l'agente di raccolta dati, modificare l'intervallo di campionamento su 10 e il formato di log in formato CSV e controllo delle versioni aggiunta al nome del file di log in mmgghhmm il formato.  
```  
logman update perf_log -si 10 -f csv -v mmddhhmm  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
[Logman creazione del contatore](logman-create-counter.md)  
