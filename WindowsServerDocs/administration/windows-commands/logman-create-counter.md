---
title: Logman creazione del contatore
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e214c32-b704-43c1-b548-e1cf43b583c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a76ef6888cc14884619d37b54ddd866cc41df3f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846802"
---
# <a name="logman-create-counter"></a>Logman creazione del contatore

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creare un agente di raccolta dati del contatore.  
  
## <a name="syntax"></a>Sintassi  
```  
logman create counter <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/?|Vengono visualizzate sensibile al contesto della Guida.|  
|-s <computer name>|Eseguire il comando nel computer remoto specificato.|  
|-config <value>|Specifica il file di impostazioni che contiene le opzioni di comando.|  
|[-n] <name>|Nome dell'oggetto di destinazione.|  
|-f < bin &#124; bincirc &#124; csv &#124; tsv &#124; sql >|Specifica il formato di log per l'agente di raccolta dati.|  
|-u [-] < utente [password] >|Specifica l'utente di Esegui come. Immissione di un * per la password produce una richiesta per la password. La password non viene visualizzata quando si digita.|  
|-m < [avvio] [stop] [[start] [stop] [...]] >|modificare per l'avvio manuale o arrestare anziché un'ora di inizio e di fine pianificata.|  
|-rf < [[hh:] mm:] ss >|Eseguire l'agente di raccolta dati per il periodo di tempo specificato.|  
|-b < g/aaaa h:mm: ss [AM &#124; PM] >|Avviare la raccolta dei dati all'ora specificata.|  
|-e < g/aaaa h:mm: ss [AM &#124; PM] >|Terminare la raccolta dei dati all'ora specificata.|  
|-si < [[hh:] mm:] ss >|Specifica l'intervallo di campionamento per raccolta dati dei contatori delle prestazioni.|  
|-o < percorso &#124; dsn! log >|Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL.|  
|-[-]r|Ripetere l'agente di raccolta dati ogni giorno alle ore di inizio specificato e di fine.|  
|-[-]a|aggiungere a un file di log esistente.|  
|-[-] Mostra|Sovrascrivere un file di log esistente.|  
|-v [-] < nnnnnn &#124; mmgghhmm >|collegare le informazioni di controllo delle versioni dei file alla fine del nome file di log.|  
|-[-]rc <task>|Eseguire il comando specificato ogni volta che il log viene chiuso.|  
|-max [-] <value>|Dimensioni massime in MB o numero massimo di record di log di SQL.|  
|-cnf [-] < [[hh:] mm:] ss >|Quando viene specificata l'ora, creare un nuovo file quando è trascorso il tempo specificato. Quando non viene specificata l'ora, creare un nuovo file quando viene superata la dimensione massima.|  
|-y|Rispondere Sì a tutte le domande senza chiedere conferma.|  
|-cf <filename>|Specifica il file di elenco di contatori delle prestazioni da raccogliere. Il file deve contenere un nome di contatore delle prestazioni per ogni riga.|  
|-c <path [path [ ]]>|Specifica i contatori delle prestazioni per raccogliere.|  
|-sc <value>|Specifica il numero massimo di campioni da raccogliere con un agente di raccolta dati del contatore delle prestazioni.|  
## <a name="remarks"></a>Note  
Dove [-] è elencato, un ulteriore - Nega l'opzione.  
## <a name="BKMK_examples"></a>Esempi  
Il comando seguente crea un contatore denominato perf_log utilizzando il contatore % Tempo processore dalla categoria del contatore processore (totale).  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time"  
```  
Il comando seguente crea un contatore denominato perf_log utilizzando il contatore % Tempo processore dalla categoria del contatore processore (totale), la creazione di un file di log con dimensioni massime pari a 10 MB e la raccolta dei dati per 1 minuto e 0 secondi.  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time" -max 10 -rf 01:00  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
