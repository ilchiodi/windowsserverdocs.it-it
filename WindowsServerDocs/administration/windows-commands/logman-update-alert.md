---
title: avviso di aggiornamento Logman
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ede94a76-931c-40ed-9fda-6766bed8ff72 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13bd7fbef96b75d9308b1e2c8475389d10bbb921
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840604"
---
# <a name="logman-update-alert"></a>avviso di aggiornamento Logman

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiornare le proprietà di un agente di raccolta dati di avviso esistente.  

## <a name="syntax"></a>Sintassi  
```  
logman update alert <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parametri  

|                 Parametro                  |                                                                               Descrizione                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    Vengono visualizzate sensibile al contesto della Guida.                                                                     |
|             -s <computer name>             |                                                          Eseguire il comando nel computer remoto specificato.                                                          |
|              -config <value>               |                                                         Specifica il file di impostazioni che contiene le opzioni di comando.                                                         |
|                [-n] <name>                 |                                                                       Nome dell'oggetto di destinazione.                                                                        |
|          -u [-] < utente [password] >           | Specifica l'utente di Esegui come. L'immissione di un \* per la password genera una richiesta per la password. La password non viene visualizzata quando si digita. |
| -m < [avvio] [stop] [[start] [stop] [...]] > |                                                modificare l'avvio o l'arresto manuale anziché un'ora di inizio o di fine pianificata.                                                 |
|             -rf < [[hh:] mm:] ss >             |                                                        Eseguire l'agente di raccolta dati per il periodo di tempo specificato.                                                         |
|     -b < g/aaaa h:mm: ss [AM & #124; PM] >      |                                                              Avviare la raccolta dei dati all'ora specificata.                                                               |
|     -e < g/aaaa h:mm: ss [AM & #124; PM] >      |                                                               Terminare la raccolta dei dati all'ora specificata.                                                                |
|             -si < [[hh:] mm:] ss >             |                                                 Specifica l'intervallo di campionamento per raccolta dati dei contatori delle prestazioni.                                                  |
|           -o < percorso & #124; dsn! log >           |                                              Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL.                                               |
|                   -[-]r                    |                                                  Ripetere l'agente di raccolta dati ogni giorno alle ore di inizio specificato e di fine.                                                  |
|                   -[-]a                    |                                                                     Accoda a un file di log esistente.                                                                     |
|                   -[-] Mostra                   |                                                                     Sovrascrivere un file di log esistente.                                                                     |
|        -v [-] < nnnnnn & #124; mmgghhmm >        |                                                   alleghi le informazioni sul controllo delle versioni dei file alla fine del nome del file di log.                                                   |
|               -[-] RC <task>                |                                                         Eseguire il comando specificato ogni volta che il log viene chiuso.                                                          |
|              -max [-] <value>               |                                                 Dimensioni massime in MB o numero massimo di record di log di SQL.                                                  |
|           -cnf [-] < [[hh:] mm:] ss >           |     Quando viene specificata l'ora, creare un nuovo file quando è trascorso il tempo specificato. Quando non viene specificata l'ora, creare un nuovo file quando viene superata la dimensione massima.     |
|                     -y                     |                                                             Rispondere Sì a tutte le domande senza chiedere conferma.                                                              |
|               -<filename> CF               |                       Specifica il file di elenco di contatori delle prestazioni da raccogliere. Il file deve contenere un nome di contatore delle prestazioni per ogni riga.                        |
|                   -el [-]                   |                                                                Abilita o disabilita la segnalazione di Log eventi.                                                                 |
|     -th < soglia [soglia [...]] >      |                                                        Specificare i contatori e i relativi valori di soglia per un avviso.                                                        |
|              -[-] ADR <name>               |                                                     Specifica l'insieme agenti di raccolta dati per l'avvio quando viene generato un avviso.                                                      |
|               -[-] TN <task>                |                                                             Specifica l'attività da eseguire quando viene generato un avviso.                                                              |
|            -[-] es <argument>             |                                               Specifica gli argomenti di attività da utilizzare con l'attività specificata tramite -tn.                                                |

## <a name="remarks"></a>Note  
Dove [-] è elencato, un ulteriore - Nega l'opzione.  
## <a name="examples"></a><a name=BKMK_examples></a>Esempi  
Nell'esempio seguente viene aggiornata la new_alert dell'agente di raccolta dati esistente, impostando il valore di soglia per il contatore% tempo processore nel gruppo di contatori processore (_Total) su 40%.  
```  
logman update alert new_alert -th \Processor(_Total)\% Processor time>40  
```  
## <a name="additional-references"></a>Altre informazioni di riferimento  
[logman](logman.md)  
[logman crea avviso](logman-create-alert.md)  
