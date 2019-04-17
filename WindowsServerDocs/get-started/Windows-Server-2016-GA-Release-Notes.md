---
title: 'Note sulla versione: problemi importanti di Windows Server 2016'
description: Riepilogano i problemi critici che richiedono soluzioni alternative per evitare l'arresto anomalo del sistema, i blocchi, gli errori di installazione o la perdita di dati.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 5a25a9152298a38ad77a377a87b71917ee586947
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507714"
---
# Note sulla versione: problemi importanti di Windows Server 2016

>Si applica a: Windows Server 2016

Queste note sulla versione presentano una sintesi dei problemi più critici del sistema operativo Windows Server&reg; 2016, incluse le soluzioni per evitare o risolvere i problemi, se noti. Per informazioni sulle modifiche da progettazione, le nuove funzionalità e le correzioni apportate in questa versione, vedi [Novità di Windows Server 2016](what-s-new-in-windows-server-2016.md) e gli annunci dei team addetti alle funzionalità specifiche. Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server 2016.  

Questo documento viene continuamente aggiornato. Non appena vengono rilevati problemi critici, le informazioni correlate vengono aggiunte immediatamente insieme alle soluzioni e alle correzioni disponibili.  

## Express gli aggiornamenti disponibili a partire da novembre 2018 (novità)

A partire da novembre 2018 "Aggiornare martedì" aggiornamento, Windows verrà nuovamente pubblicare [aggiornamenti Express](express-updates.md) per Windows Server 2016. Se si utilizza WSUS e System Center Configuration Manager (SCCM) ancora una volta, vedrai due pacchetti per l'aggiornamento di Windows Server 2016: un aggiornamento completo e un aggiornamento Express. Se vuoi usare Express per gli ambienti di server, è necessario verificare che il server ha eseguito un aggiornamento completo dal novembre 2017 (KB # 4048953) per garantire che l'aggiornamento Express viene installato correttamente. Se si tenta di un aggiornamento Express in un server che non è ancora aggiornato dopo che l'aggiornamento 11B 2017 (KB # 4048953), vedrai errori ripetuti che usano la larghezza di banda e le risorse di CPU in un ciclo infinito. Se ricevi in questo scenario, interrompere l'aggiornamento Express il push e push invece un aggiornamento recente completo per interrompere il ciclo di errore.  

## Opzione di installazione dei componenti di base del server
[comment]: # (ID: 370; inviante: amason; stato: approvato)  
Quando si installa Windows Server 2016 usando l'opzione di installazione Server Core, lo spooler di stampa viene installato e avviato per impostazione predefinita, anche quando non è installato il ruolo Server di stampa.

Per evitare questo problema, dopo il primo avvio, disabilita lo spooler di stampa.


## Contenitori  

[comment]: # (ID: 371; inviante: taylorb; stato: approvato)  
- Prima di usare i contenitori, installa l'[aggiornamento dello stack di manutenzione per Windows 10 versione 1607: 23 agosto 2016](https://support.microsoft.com/en-us/kb/3176936) o eventuali aggiornamenti successivi disponibili. Se non esegui questa installazione, può verificarsi una serie di problemi, inclusi errori durante la creazione, l'avvio o l'esecuzione di contenitori ed errori simili a "CreateProcess non riuscito in Win32: Server RPC non disponibile."

[comment]: # (ID: 373; inviante: plang; stato: approvato)  
- Il provider OneGet NanoServerPackage non funziona nei contenitori di Windows. Per risolvere il problema, usa Find-NanoServerPackage e Save-NanoServerPackage in un altro computer (non un contenitore) per scaricare il pacchetto necessario. Copia quindi i pacchetti nel contenitore e installali.

## Device Guard
[comment]: # (ID: 369; inviante: nirb; stato: approvato)
Se usi la protezione basata su virtualizzazione dell'integrità del codice o macchine virtuali schermate (che usano la protezione basata su virtualizzazione dell'integrità del codice), tieni presente che queste tecnologie potrebbero non essere compatibili con alcuni dispositivi e applicazioni. Dovrai testare queste configurazioni nel lab prima di abilitare le funzionalità nei sistemi di produzione. In caso contrario, potrebbero verificarsi errori di arresto o la perdita imprevista di dati.

## Microsoft Exchange
[comment]: # (ID: 375; inviante: wgries; stato: approvato)
Se provi a eseguire Microsoft Exchange 2016 CU3 in Windows Server 2016, riscontrerai errori nel processo host IIS W3WP.exe. Al momento non sono disponibili soluzioni alternative. Rimanda la distribuzione di Exchange 2016 CU3 in Windows Server 2016 fino a quando non sarà disponibile una correzione supportata.

## Strumenti di amministrazione remota del server
[comment]: # (ID: 374; inviante: ryanpu; stato: approvato)
Se esegui una versione di Windows 10 meno recente rispetto all'aggiornamento dell'anniversario, usi macchine virtuali e Hyper-V con un modulo TPM (Trusted Platform Module) virtuale abilitato (incluse macchine virtuali schermate) e quindi installi la versione di Strumenti di amministrazione remota del server per Windows Server 2016, i tentativi di avviare le macchine virtuali avranno esito negativo.

Per evitare questo problema, aggiorna il computer client alla versione Aggiornamento dell'anniversario di Windows 10 (o successiva) prima di installare Strumenti di amministrazione remota del server. Se il problema si è già verificato, disinstalla Strumenti di amministrazione remota del server, aggiorna il client alla versione Aggiornamento dell'anniversario di Windows 10 e quindi reinstalla Strumenti di amministrazione remota del server.


## Macchine virtuali schermate
[comment]: # (ID: 369; inviante: nirb; stato: approvato)  
- Assicurati di aver installato tutti gli aggiornamenti disponibili prima di distribuire le macchine virtuali schermate nell'ambiente di produzione.

- Se usi la protezione basata su virtualizzazione dell'integrità del codice o macchine virtuali schermate (che usano la protezione basata su virtualizzazione dell'integrità del codice), tieni presente che queste tecnologie potrebbero non essere compatibili con alcuni dispositivi e applicazioni. Dovrai testare queste configurazioni nel lab prima di abilitare le funzionalità nei sistemi di produzione. In caso contrario, potrebbero verificarsi errori di arresto o la perdita imprevista di dati.


## Menu Start
[comment]: # (ID: 372; inviante: samli; stato: approvato)
Questo problema interessa la versione di Windows Server 2016 installata con l'opzione Server con Esperienza desktop.

Se si installano applicazioni che aggiungono collegamenti in una cartella del menu Start, i collegamenti non funzionano finché non ci si disconnette e quindi si accede nuovamente al sistema.



Tornare all'hub principale di [Windows Server 2016](Windows-Server-2016.md).

## Prestazioni di Storport
Alcuni sistemi possono presentare prestazioni di archiviazione ridotte durante l'esecuzione di una nuova installazione di Windows Server 2016 rispetto a Windows Server 2012 R2.Sono state apportate varie modifiche durante lo sviluppo di Windows Server 2016 per migliorare la sicurezza e l'affidabilità della piattaforma. Alcune di queste modifiche, ad esempio l'abilitazione di Windows Defender per impostazione predefinita, comportano percorsi di I/O più lunghi che possono ridurre le prestazioni di I/O in determinati carichi di lavoro e modelli. Microsoft consiglia di non disabilitare Windows Defender, in quanto è un livello di protezione importante per i sistemi.  

## Copyright  
Questo documento viene fornito "così com'è". Le informazioni e le opinioni espresse nel presente documento, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifiche senza preavviso.  

Il presente documento non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft. È possibile copiare e usare questo documento per fini di riferimento interno.  

&copy;2016 Microsoft Corporation. Tutti i diritti sono riservati.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server sono marchi o marchi registrati di Microsoft Corporation negli Stati Uniti e/o negli altri paesi.  

Questo prodotto contiene software per filtri grafici, parte del quale è basato sul lavoro dell'Independent JPEG Group.  


1.0  
