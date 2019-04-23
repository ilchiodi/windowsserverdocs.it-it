---
title: Logman creare traccia
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b4dfecd-6f56-4c51-b622-c2054b4aabd7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 065c5bfc9278d4e7deef453ee8bd3e20296d3a56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832282"
---
# <a name="logman-create-trace"></a>Logman creare traccia

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

creare una raccolta di dati di traccia eventi.  
  
## <a name="syntax"></a>Sintassi  
```  
logman create trace <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/?|Vengono visualizzate sensibile al contesto della Guida.|  
|-s <computer name>|Eseguire il comando nel computer remoto specificato.|  
|-config <value>|Specifica il file di impostazioni che contiene le opzioni di comando.|  
|-ets|Inviare comandi alle sessioni di traccia eventi direttamente senza salvare o pianificare.|  
|[-n] <name>|Nome dell'oggetto di destinazione.|  
|-f < bin & #124; bincirc & #124; csv & #124; tsv & #124; sql >|Specifica il formato di log per l'agente di raccolta dati.|  
|-u [-] < utente [password] >|Specifica l'utente di Esegui come. Immissione di un * per la password produce una richiesta per la password. La password non viene visualizzata quando si digita.|  
|-m < [avvio] [stop] [[start] [stop] [...]] >|modificare per l'avvio manuale o arrestare anziché un'ora di inizio e di fine pianificata.|  
|-rf < [[hh:] mm:] ss >|Eseguire l'agente di raccolta dati per il periodo di tempo specificato.|  
|-b < g/aaaa h:mm: ss [AM & #124; PM] >|Avviare la raccolta dei dati all'ora specificata.|  
|-e < g/aaaa h:mm: ss [AM & #124; PM] >|Terminare la raccolta dei dati all'ora specificata.|  
|-o < percorso & #124; dsn! log >|Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL.|  
|-[-]r|Ripetere l'agente di raccolta dati ogni giorno alle ore di inizio specificato e di fine.|  
|-[-]a|aggiungere a un file di log esistente.|  
|-[-] Mostra|Sovrascrivere un file di log esistente.|  
|-v [-] < nnnnnn & #124; mmgghhmm >|collegare le informazioni di controllo delle versioni dei file alla fine del nome file di log.|  
|-[-]rc <task>|Eseguire il comando specificato ogni volta che il log viene chiuso.|  
|-max [-] <value>|Dimensioni massime in MB o numero massimo di record di log di SQL.|  
|-cnf [-] < [[hh:] mm:] ss >|Quando viene specificata l'ora, creare un nuovo file quando è trascorso il tempo specificato. Quando non viene specificata l'ora, creare un nuovo file quando viene superata la dimensione massima.|  
|-y|Rispondere Sì a tutte le domande senza chiedere conferma.|  
|-ct < prestazioni & #124; sistema & #124; ciclo >|Specifica il tipo di orologio della sessione di traccia eventi.|  
|-ln < nome_registratore >|Specifica il nome del logger per sessioni di traccia eventi.|  
|-ft < [[hh:] mm:] ss >|Specifica il timer di cancellazione di sessione di traccia eventi.|  
|-p [-] < provider [flag [livello]] >|Specifica un unico provider di traccia eventi per abilitare.|  
|-pf <filename>|Specifica un file più provider di traccia di eventi da abilitare. Il file deve essere un file di testo contenente un provider per ogni riga.|  
|-[-] rt|Eseguire la sessione di traccia degli eventi in modalità in tempo reale.|  
|-ul [-]|Eseguire la sessione di traccia degli eventi in modalità utente.|  
|-bs <value>|Specifica la dimensione del buffer di sessione di traccia eventi in kb.|  
|-nb <min max>|Specifica il numero di buffer di sessione di traccia eventi.|  
|-modalità < globalsequence & #124; localsequence & #124; pagedmemory >|Specifica la modalità di logger sessione di traccia eventi.<br /><br />**Globalsequence** Specifica che la traccia di eventi aggiungere un numero di sequenza per ogni evento ricevuto indipendentemente dall'analisi della sessione ha ricevuto l'evento.<br /><br />**Localsequence** Specifica che la traccia di eventi aggiungere numeri di sequenza per gli eventi ricevuti in una sessione di traccia specifico. Quando il **localsequence** viene utilizzata l'opzione, i numeri di sequenza duplicati possono esistere in tutte le sessioni, ma saranno univoci all'interno di ogni sessione di traccia.<br /><br />**Pagedmemory** Specifica che la traccia di eventi utilizza la memoria di paging anziché il pool di memoria non di paging predefinito per le allocazioni dei buffer interno.|  
## <a name="remarks"></a>Note  
Dove [-] è elencato, un ulteriore - Nega l'opzione.  
## <a name="BKMK_examples"></a>Esempi  
Nell'esempio seguente viene creato un evento traccia agente di raccolta dati denominato trace_log utilizzando alcun buffer meno di 16 e non più di 256, ogni buffer 64kb di dimensioni e restituisce i risultati in c:\logfile il percorso.  
```  
logman create trace trace_log -nb 16 256 -bs 64 -o c:\logfile  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  