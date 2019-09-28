---
title: 'Note sulla versione: problemi importanti di Windows Server 2016'
description: Riepilogano i problemi critici che richiedono soluzioni alternative per evitare l'arresto anomalo del sistema, i blocchi, gli errori di installazione o la perdita di dati.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 11/13/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: 4e2f7cbaed42dd1c1b1884438467cf59f1529f0c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391545"
---
# <a name="release-notes-important-issues-in-windows-server-2016"></a>Note sulla versione: problemi importanti di Windows Server 2016

>Si applica a: Windows Server 2016

Queste note sulla versione presentano una sintesi dei problemi più critici del sistema operativo Windows Server 2016, incluse le soluzioni per evitare o risolvere i problemi, se noti. Per informazioni sulle modifiche da progettazione, le nuove funzionalità e le correzioni apportate in questa versione, vedere [Novità di Windows Server 2016](whats-new-in-windows-server-2016.md) e gli annunci dei team addetti alle funzionalità specifiche. Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server 2016.

Questo documento viene continuamente aggiornato. Non appena vengono rilevati problemi critici, le informazioni correlate vengono aggiunte immediatamente insieme alle soluzioni e alle correzioni disponibili.

## <a name="express-updates-available-starting-in-november-2018-new"></a>Aggiornamenti rapidi disponibili a partire da novembre 2018 (NUOVO)

A partire dall'aggiornamento di novembre 2018 previsto per il secondo martedì del mese, Windows pubblicherà nuovamente [aggiornamenti rapidi](express-updates.md) per Windows Server 2016. Se usi Windows Server Update Services e System Center Configuration Manager (SCCM), noterai ancora una volta che sono disponibili due pacchetti per l'aggiornamento di Windows Server 2016: un aggiornamento completo e un aggiornamento rapido. Se vuoi usare l'aggiornamento rapido per gli ambienti server, devi verificare che il server abbia eseguito un aggiornamento completo a partire da novembre 2017 (KB 4048953) per avere la certezza che l'aggiornamento rapido venga installato correttamente. Se provi a eseguire un aggiornamento rapido in un server che non è stato aggiornato dopo l'aggiornamento 11B 2017 (KB 4048953), noterai errori ripetuti che comportano l'utilizzo di larghezza di banda e risorse della CPU in un ciclo infinito. Se si verifica questo scenario, arresta l'esecuzione dell'aggiornamento rapido ed esegui in alternativa un aggiornamento completo recente per interrompere il ciclo di errore.

## <a name="server-core-installation-option"></a>Opzione di installazione dei componenti di base del server

[comment]: # (ID: 370; mittente: amason; stato: approvato)

Quando si installa Windows Server 2016 usando l'opzione di installazione Server Core, lo spooler di stampa viene installato e avviato per impostazione predefinita, anche quando non è installato il ruolo Server di stampa.

Per evitare questo problema, dopo il primo avvio, disabilitare lo spooler di stampa.

## <a name="containers"></a>Contenitori

[comment]: # (ID: 371; mittente: taylorb; stato: approvato)
- Prima di usare i contenitori, installa l'[aggiornamento dello stack di manutenzione per Windows 10 versione 1607: 23 agosto 2016](https://support.microsoft.com/en-us/kb/3176936) o eventuali aggiornamenti successivi disponibili. Se non esegui questa installazione, può verificarsi una serie di problemi, inclusi errori durante la creazione, l'avvio o l'esecuzione di contenitori ed errori simili al seguente: "CreateProcess non riuscito in Win32: server RPC non disponibile".

[comment]: # (ID: 373; mittente: plang; stato: approvato)
- Il provider OneGet NanoServerPackage non funziona nei contenitori di Windows. Per risolvere il problema, usare Find-NanoServerPackage e Save-NanoServerPackage in un altro computer (non un contenitore) per scaricare il pacchetto necessario. Copiare quindi i pacchetti nel contenitore e installarli.

## <a name="device-guard"></a>Device Guard

[comment]: # (ID: 369; mittente: nirb; stato: approvato)
Se usi la protezione basata su virtualizzazione dell'integrità del codice o macchine virtuali schermate (che usano la protezione basata su virtualizzazione dell'integrità del codice), tieni presente che queste tecnologie potrebbero non essere compatibili con alcuni dispositivi e applicazioni. Dovrai testare queste configurazioni nel lab prima di abilitare le funzionalità nei sistemi di produzione. In caso contrario, potrebbero verificarsi errori di arresto o la perdita imprevista di dati.

## <a name="microsoft-exchange"></a>Microsoft Exchange

[comment]: # (ID: 375; mittente: wgries; stato: approvato)
Se provi a eseguire Microsoft Exchange 2016 CU3 in Windows Server 2016, riscontrerai errori nel processo host IIS W3WP.exe. Al momento non sono disponibili soluzioni alternative. Rimanda la distribuzione di Exchange 2016 CU3 in Windows Server 2016 fino a quando non sarà disponibile una correzione supportata.

## <a name="remote-server-administration-tools-rsat"></a>Strumenti di amministrazione remota del server

[comment]: # (ID: 374; mittente: ryanpu; stato: approvato)
Se esegui una versione di Windows 10 meno recente rispetto all'aggiornamento dell'anniversario, usi macchine virtuali e Hyper-V con un modulo TPM (Trusted Platform Module) virtuale abilitato (incluse macchine virtuali schermate) e quindi installi la versione di Strumenti di amministrazione remota del server per Windows Server 2016, i tentativi di avviare le macchine virtuali avranno esito negativo.

Per evitare questo problema, aggiornare il computer client alla versione Aggiornamento dell'anniversario di Windows 10 (o successiva) prima di installare Strumenti di amministrazione remota del server. Se il problema si è già verificato, disinstallare Strumenti di amministrazione remota del server, aggiornare il client alla versione Aggiornamento dell'anniversario di Windows 10 e quindi reinstallare Strumenti di amministrazione remota del server.

## <a name="shielded-virtual-machines"></a>Macchine virtuali schermate

[comment]: # (ID: 369; mittente: nirb; stato: approvato)  
- Assicurati di aver installato tutti gli aggiornamenti disponibili prima di distribuire le macchine virtuali schermate nell'ambiente di produzione.

- Se usi la protezione basata su virtualizzazione dell'integrità del codice o macchine virtuali schermate (che usano la protezione basata su virtualizzazione dell'integrità del codice), tieni presente che queste tecnologie potrebbero non essere compatibili con alcuni dispositivi e applicazioni. Dovrai testare queste configurazioni nel lab prima di abilitare le funzionalità nei sistemi di produzione. In caso contrario, potrebbero verificarsi errori di arresto o la perdita imprevista di dati.

## <a name="start-menu"></a>Menu Start

[comment]: # (ID: 372; mittente: samli; stato: approvato)
Questo problema interessa la versione di Windows Server 2016 installata con l'opzione Server con Esperienza desktop.

Se installi applicazioni che aggiungono collegamenti in una cartella del menu **Start**, i collegamenti non funzionano finché non ti disconnetti e quindi accedi nuovamente al sistema.

Tornare all'hub principale di [Windows Server 2016](Windows-Server-2016.md).

## <a name="storport-performance"></a>Prestazioni di Storport

Alcuni sistemi possono presentare prestazioni della memoria ridotte durante l'esecuzione di una nuova installazione di Windows Server 2016 rispetto a Windows Server 2012 R2.  Sono state apportate varie modifiche durante lo sviluppo di Windows Server 2016 per migliorare la sicurezza e l'affidabilità della piattaforma. Alcune di queste modifiche, ad esempio l'abilitazione di Windows Defender per impostazione predefinita, comportano percorsi di I/O più lunghi, che possono ridurre le prestazioni di I/O in determinati carichi di lavoro e modelli. Microsoft consiglia di non disabilitare Windows Defender, in quanto è un livello di protezione importante per i sistemi.  

## <a name="copyright"></a>Copyright

Questo documento viene fornito "così com'è". Le informazioni e le opinioni espresse nel presente documento, inclusi gli URL e altri riferimenti a siti Web Internet, sono soggette a modifiche senza preavviso.  

Il presente documento non implica la concessione di alcun diritto di proprietà intellettuale in relazione ai prodotti Microsoft. Sono consentiti la copia e l'uso del presente documento a fini di riferimento interno.  

&copy; 2016 Microsoft Corporation. Tutti i diritti sono riservati.  

Microsoft, Active Directory, Hyper-V, Windows e Windows Server sono marchi o marchi registrati di Microsoft Corporation negli Stati Uniti e/o negli altri paesi.  

Questo prodotto contiene software per filtri grafici, parte del quale è basato sul lavoro dell'Independent JPEG Group.  

1.0
