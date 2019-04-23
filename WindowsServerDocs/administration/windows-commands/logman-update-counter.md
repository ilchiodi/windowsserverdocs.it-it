---
title: Logman update contatore
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a6ab1f75892bf8473714e0201c2a6db25cad426
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866772"
---
# <a name="logman-update-counter"></a>Logman update contatore

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiornare le proprietà di un esistente contatore agente di raccolta dati.  
  
## <a name="syntax"></a>Sintassi  
```  
logman update counter <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parametri  
|Parametro|Descrizione|  
|-------|--------|  
|/?|Vengono visualizzate sensibile al contesto della Guida.|  
|-s <computer name>|Eseguire il comando nel computer remoto specificato.|  
|-config <value>|Specifica il file di impostazioni che contiene le opzioni di comando.|  
|[-n] <name>|Nome dell'oggetto di destinazione.|  
|-f < bin & #124; bincirc & #124; csv & #124; tsv & #124; sql >|Specifica il formato di log per l'agente di raccolta dati.|  
|-u [-] < utente [password] >|Specifica l'utente di Esegui come. Immissione di un * per la password produce una richiesta per la password. La password non viene visualizzata quando si digita.|  
|-m < [avvio] [stop] [[start] [stop] [...]] >|modificare per l'avvio manuale o arrestare anziché un'ora di inizio e di fine pianificata.|  
|-rf < [[hh:] mm:] ss >|Eseguire l'agente di raccolta dati per il periodo di tempo specificato.|  
|-b < g/aaaa h:mm: ss [AM & #124; PM] >|Avviare la raccolta dei dati all'ora specificata.|  
|-e < g/aaaa h:mm: ss [AM & #124; PM] >|Terminare la raccolta dei dati all'ora specificata.|  
|-si < [[hh:] mm:] ss >|Specifica l'intervallo di campionamento per raccolta dati dei contatori delle prestazioni.|  
|-o < percorso & #124; dsn! log >|Specifica che il file di log di output o DSN e di log impostare il nome in un database SQL.|  
|-[-]r|Ripetere l'agente di raccolta dati ogni giorno alle ore di inizio specificato e di fine.|  
|-[-]a|aggiungere a un file di log esistente.|  
|-[-] Mostra|Sovrascrivere un file di log esistente.|  
|-v [-] < nnnnnn & #124; mmgghhmm >|collegare le informazioni di controllo delle versioni dei file alla fine del nome file di log.|  
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
Il seguente comando Aggiorna perf_log l'agente di raccolta dati, modificare l'intervallo di campionamento su 10 e il formato di log in formato CSV e controllo delle versioni aggiunta al nome del file di log in mmgghhmm il formato.  
```  
logman update perf_log -si 10 -f csv -v mmddhhmm  
```  
#### <a name="additional-references"></a>Riferimenti aggiuntivi  
[logman](logman.md)  
[Logman creazione del contatore](logman-create-counter.md)  
