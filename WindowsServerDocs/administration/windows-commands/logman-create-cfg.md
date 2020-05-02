---
title: Logman creare cfg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bfc87093-3ff5-4e19-aa93-d185fb8e2239
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eabbbca0e13501e08efa80f19303362bc5ed687b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724410"
---
# <a name="logman-create-cfg"></a>Logman creare cfg

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

creare un agente di raccolta dati di configurazione.  

## <a name="syntax"></a>Sintassi  
```  
logman create cfg <[-n] <name>> [options]  
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
|                      -[-] ni                      |                                                         Abilitare (-NI) o disabilitare (-NI) la query dell'interfaccia di rete.                                                          |
|             reg - < percorso [[...]] >             |                                                                 Specifica i valori del Registro di sistema per raccogliere.                                                                 |
|            -energia < query [query [...]] >            |                                                      Specifica gli oggetti WMI per raccogliere mediante il linguaggio di query SQL.                                                       |
|             -ftc < percorso [[...]] >             |                                                           Specifica il percorso completo per i file da raccogliere.                                                            |

## <a name="remarks"></a>Osservazioni  
Dove [-] è elencato, un ulteriore - Nega l'opzione.  
## <a name="examples"></a>Esempi  
Il comando seguente crea un agente di raccolta dati di configurazione denominato cfg_log usando la chiave del\\registro di sistema HKEY_LOCAL_MACHINE \Software\Microsoft\Windows NT\Currentverion.  
```  
logman create cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\\  
```  
Il comando seguente crea un agente di raccolta dati di configurazione denominato cfg_log che registra tutti gli oggetti WMI da root\wmi nella colonna di database MSNdis_Vendordriverversion.  
```  
logman create cfg cfg_log -mgt root\wmi:select * FROM MSNdis_Vendordriverversion  
```  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
