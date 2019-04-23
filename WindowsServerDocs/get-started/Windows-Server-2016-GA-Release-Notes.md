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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878652"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>Note sulla versione: Problemi importanti in Windows Server 2016

>Si applica a: Windows Server 2016

Queste note sulla versione presentano una sintesi dei problemi più critici del sistema operativo Windows Server&reg; 2016, incluse le soluzioni per evitare o risolvere i problemi, se noti. Per informazioni sulle modifiche da progettazione, le nuove funzionalità e le correzioni apportate in questa versione, vedere [Novità di Windows Server 2016](what-s-new-in-windows-server-2016.md) e gli annunci dei team addetti alle funzionalità specifiche. Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server 2016.  

Questo documento viene continuamente aggiornato. Non appena vengono rilevati problemi critici, le informazioni correlate vengono aggiunte immediatamente insieme alle soluzioni e alle correzioni disponibili.  

## <a name="express-updates-available-starting-in-november-2018-new"></a>Disponibile a partire da novembre 2018 (nuovo) degli aggiornamenti rapidi

A partire da novembre 2018 update "Aggiornare Tuesday", Windows nuovamente pubblicherà [degli aggiornamenti rapidi](express-updates.md) per Windows Server 2016. Se si usa Windows Server Update Services e System Center Configuration Manager (SCCM) si noterà anche in questo caso due pacchetti per l'aggiornamento di Windows Server 2016: un aggiornamento completo e un aggiornamento rapido. Se si desidera usare l'installazione rapida per gli ambienti server, è necessario verificare che il server ha avuto un aggiornamento completo partire da novembre 2017 (KB # 4048953) per garantire che l'aggiornamento di Express viene installato correttamente. Se si tenta un aggiornamento rapido in un server che non è stato aggiornato dopo l'aggiornamento di 11 ter 2017 (KB # 4048953), si noterà errori ripetuti che utilizzano la larghezza di banda e le risorse della CPU in un ciclo infinito. Se viene visualizzato in questo scenario, arrestare il push dell'aggiornamento rapido e push un recente aggiornamento completo per interrompere il ciclo di errore.  

## <a name="server-core-installation-option"></a>Opzione di installazione dei componenti di base del server
[comment]: # (ID: 370; Autore: amason; stato: ha approvato)  
Quando si installa Windows Server 2016 usando l'opzione di installazione Server Core, lo spooler di stampa viene installato e avviato per impostazione predefinita, anche quando non è installato il ruolo Server di stampa.

Per evitare questo problema, dopo il primo avvio, disabilitare lo spooler di stampa.


## <a name="containers"></a>Contenitori  

[comment]: # (ID: 371; Autore: taylorb; stato: ha approvato)  
- Prima di usare i contenitori, installare [aggiornamento dello stack di manutenzione per Windows 10 versione 1607: 23 agosto 2016](https://support.microsoft.com/en-us/kb/3176936) o eventuali aggiornamenti successivi disponibili. In caso contrario, può verificarsi una serie di problemi, inclusi errori durante la compilazione, avvio o l'esecuzione dei contenitori ed errori simili a "CreateProcess non riuscito in Win32: Il server RPC non disponibile."

[comment]: # (ID: 373; Autore: plang; stato: ha approvato)  
- Il provider OneGet NanoServerPackage non funziona nei contenitori di Windows. Per risolvere il problema, usare Find-NanoServerPackage e Save-NanoServerPackage in un altro computer (non un contenitore) per scaricare il pacchetto necessario. Copiare quindi i pacchetti nel contenitore e installarli.

## <a name="device-guard"></a>Device Guard
[comment]: # (ID: 369; Autore: nirb; stato: ha approvato)
Se usi la protezione basata su virtualizzazione dell'integrità del codice o macchine virtuali schermate (che usano la protezione basata su virtualizzazione dell'integrità del codice), tieni presente che queste tecnologie potrebbero non essere compatibili con alcuni dispositivi e applicazioni. Dovrai testare queste configurazioni nel lab prima di abilitare le funzionalità nei sistemi di produzione. In caso contrario, potrebbero verificarsi errori di arresto o la perdita imprevista di dati.

## <a name="microsoft-exchange"></a>Microsoft Exchange
[comment]: # (ID: 375; Autore: wgries; stato: ha approvato)
Se provi a eseguire Microsoft Exchange 2016 CU3 in Windows Server 2016, riscontrerai errori nel processo host IIS W3WP.exe. Al momento non sono disponibili soluzioni alternative. Rimanda la distribuzione di Exchange 2016 CU3 in Windows Server 2016 fino a quando non sarà disponibile una correzione supportata.

## <a name="remote-server-administration-tools-rsat"></a>Strumenti di amministrazione remota del server
[comment]: # (ID: 374; Autore: ryanpu; stato: ha approvato)
Se si esegue una versione di Windows 10 meno recente rispetto all'aggiornamento dell'anniversario, si usano macchine virtuali e Hyper-V con un modulo TPM (Trusted Platform Module) virtuale abilitato (incluse macchine virtuali schermate) e quindi si installa la versione di Strumenti di amministrazione remota del server per Windows Server 2016, i tentativi di avviare le macchine virtuali avranno esito negativo.

Per evitare questo problema, aggiornare il computer client alla versione Aggiornamento dell'anniversario di Windows 10 (o successiva) prima di installare Strumenti di amministrazione remota del server. Se il problema si è già verificato, disinstallare Strumenti di amministrazione remota del server, aggiornare il client alla versione Aggiornamento dell'anniversario di Windows 10 e quindi reinstallare Strumenti di amministrazione remota del server.


## <a name="shielded-virtual-machines"></a>Macchine virtuali schermate
[comment]: # (ID: 369; Autore: nirb; stato: ha approvato)  
- Assicurati di aver installato tutti gli aggiornamenti disponibili prima di distribuire le macchine virtuali schermate nell'ambiente di produzione.

- Se usi la protezione basata su virtualizzazione dell'integrità del codice o macchine virtuali schermate (che usano la protezione basata su virtualizzazione dell'integrità del codice), tieni presente che queste tecnologie potrebbero non essere compatibili con alcuni dispositivi e applicazioni. Dovrai testare queste configurazioni nel lab prima di abilitare le funzionalità nei sistemi di produzione. In caso contrario, potrebbero verificarsi errori di arresto o la perdita imprevista di dati.


## <a name="start-menu"></a>Menu Start
[comment]: # (ID: 372; Autore: samli; stato: ha approvato)
Questo problema interessa la versione di Windows Server 2016 installata con l'opzione Server con Esperienza desktop.

Se si installano applicazioni che aggiungono collegamenti in una cartella del menu Start, i collegamenti non funzionano finché non ci si disconnette e quindi si accede nuovamente al sistema.



Tornare all'hub principale di [Windows Server 2016](Windows-Server-2016.md).

## <a name="storport-performance"></a>Prestazioni di Storport
Alcuni sistemi possono presentare prestazioni di archiviazione ridotte durante l'esecuzione di una nuova installazione di Windows Server 2016 rispetto a Windows Server 2012 R2.  Sono state apportate varie modifiche durante lo sviluppo di Windows Server 2016 per migliorare la sicurezza e l'affidabilità della piattaforma. Alcune di queste modifiche, ad esempio l'abilitazione di Windows Defender per impostazione predefinita, comportano percorsi di I/O più lunghi che possono ridurre le prestazioni di I/O in determinati carichi di lavoro e modelli. Microsoft consiglia di non disabilitare Windows Defender, in quanto è un livello di protezione importante per i sistemi.  

## <a name="copyright"></a>Copyright  
Questo documento viene fornito "così com'è". Le informazioni e le opinioni espresse nel presente documento, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifiche senza preavviso.  

Il presente documento non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft. Sono consentiti la copia e l'uso del presente documento a fini di riferimento interno.  

&copy;2016 Microsoft Corporation. Tutti i diritti sono riservati.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server sono marchi o marchi registrati di Microsoft Corporation negli Stati Uniti e/o negli altri paesi.  

Questo prodotto contiene software per filtri grafici, parte del quale è basato sul lavoro dell'Independent JPEG Group.  


1.0  
