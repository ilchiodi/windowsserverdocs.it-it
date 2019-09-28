---
title: Considerazioni sull'utilizzo della memoria nell'ottimizzazione delle prestazioni di Active Directory
description: Utilizzo della memoria da parte del processo Lsass. exe nei controller di dominio che eseguono Windows Server 2012 R2, 2016 e 2019.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 55ac47d835874ddb8e160603f08cbafa985aad2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370294"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Considerazioni sull'utilizzo della memoria per l'ottimizzazione delle prestazioni di Active Directory

In questo articolo vengono descritte alcune nozioni di base del Local Security Authority Subsystem Service (LSASS, noto anche come processo Lsass. exe), le procedure consigliate per la configurazione di LSASS e le aspettative per l'utilizzo della memoria. Questo articolo deve essere usato come guida per l'analisi delle prestazioni e dell'utilizzo della memoria di LSASS nei controller di dominio. Le informazioni contenute in questo articolo possono essere utili in caso di domande su come ottimizzare e configurare i server e i controller di dominio per ottimizzare questo motore.  

LSASS è responsabile della gestione dell'autenticazione del dominio LSA (Local Security Authority) e della gestione Active Directory. LSASS gestisce l'autenticazione sia per il client che per il server e governa anche il motore di Active Directory. LSASS è responsabile dei componenti seguenti:  

- Autorità di sicurezza locale
- Servizio NetLogon
- Servizio Gestione account di sicurezza (SAM)
- Servizio Server LSA
- Secure Sockets Layer (SSL)
- Protocollo di autenticazione Kerberos V5
- Protocollo di autenticazione NTLM
- Altri pacchetti di autenticazione caricati in LSA

I servizi di database Active Directory (NTDSAI. dll) funzionano con Extensible Storage Engine (ESE, ESENT. dll).

Di seguito è riportato un diagramma visivo dell'utilizzo della memoria LSASS in un controller di dominio:

![Diagramma dei componenti che utilizzano la memoria LSASS](media/domain-controller-lsass-memory-usage.png)  

La quantità di memoria utilizzata da LSASS in un controller di dominio aumenta in base all'utilizzo Active Directory. Quando viene eseguita una query sui dati, questo viene memorizzato nella cache. Di conseguenza, è normale vedere LSASS con una quantità di memoria superiore alle dimensioni del file di database Active Directory (NTDS. dit).

Come illustrato nel diagramma, l'utilizzo della memoria LSASS può essere suddiviso in più parti, tra cui la cache del buffer del database ESE, l'archivio delle versioni ESE e altri. Nella parte restante di questo articolo vengono fornite informazioni dettagliate su ognuna di queste parti.

## <a name="ese-database-buffer-cache"></a>Cache del buffer del database ESE  
L'utilizzo più elevato della memoria variabile all'interno di LSASS è la cache del buffer del database ESE. La dimensione della cache può variare da meno di 1 MB alla dimensione dell'intero database. Poiché una cache di dimensioni maggiori migliora le prestazioni, il motore di database per Active Directory (ESENT) tenta di mantenendo la cache il più grande possibile. Mentre le dimensioni della cache variano in base al numero di richieste di memoria nel computer, le dimensioni massime della cache del buffer del database ESE sono limitate *solo* dalla RAM fisica installata nel computer. Fino a quando non si verificano altre richieste di memoria, la cache può raggiungere le dimensioni del file di database Active Directory NTDS. dit. Maggiore è il database che può essere memorizzato nella cache, migliori saranno le prestazioni del controller di dominio.  
  
> [!NOTE]
> A causa della modalità di funzionamento dell'algoritmo di memorizzazione nella cache del database, in un sistema a 64 bit in cui le dimensioni del database sono inferiori alla RAM disponibile, la cache del database può aumentare di dimensioni superiori a quelle del database da 30 a 40%.

## <a name="ese-version-store"></a>Archivio versioni ESE

L'archivio delle versioni ESE presenta una variabile utilizzo di memoria (la parte rossa nel diagramma precedente). La quantità di memoria utilizzata varia a seconda che si disponga di Windows Server 2019 o di versioni precedenti di Windows.

- Nelle versioni di Windows Server precedenti a Windows Server 2019, per impostazione predefinita LSASS può utilizzare fino a circa 400MB di memoria (a seconda del numero di CPU) in un computer a 64 bit per l'archivio delle versioni ESE. Per altre informazioni sull'uso dell'archivio versioni, vedere il post di Blog di nel seguente di Ryan Ries: [L'archivio versione ha chiamato e sono tutti bucket](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- In Windows Server 2019 questa operazione è semplificata e quando il servizio NTDS viene avviato per la prima volta, le dimensioni dell'archivio versioni ESE sono ora calcolate come 10% di RAM fisica, con un minimo di 400MB e un massimo di 4 GB. Per informazioni dettagliate su questa e sulla risoluzione dei problemi relativi all'archivio versioni, vedere un altro interessante Blog di Ryan Ries: [Approfondimento: Active Directory modifiche dell'archivio versioni ESE nel server 2019 @ no__t-0.

## <a name="other-memory-use"></a>Altro uso della memoria

Infine, vi sono codice, stack, heap e varie strutture di dati a dimensione fissa, ad esempio la cache dello schema. La quantità di memoria utilizzata da LSASS può variare a seconda del carico del computer. Con l'aumentare del numero di thread in esecuzione, viene eseguito il numero di stack di memoria. In media, LSASS USA da 100 MB a 300 MB di memoria per questi componenti fissi. Quando viene installata una quantità maggiore di RAM, LSASS può usare più RAM e meno memoria virtuale.

**Limitare o ridurre al minimo il numero di programmi sul controller di dominio o aggiungere ulteriore RAM laddove appropriato**

Per prestazioni ottimali, LSASS accetta il maggior quantità di RAM possibile in un determinato controller di dominio. LSASS cede tale RAM come richiesto da altri processi. L'idea è quella di ottimizzare le prestazioni di LSASS pur continuando a mantenere conto di altri processi che potrebbero essere eseguiti in un computer. L'elenco dei programmi da controllare include gli agenti di monitoraggio. Alcuni clienti dispongono di agenti distinti per varie funzioni server che possono utilizzare notevoli risorse di RAM. Alcuni possono eseguire molte query WMI, per le quali sono disponibili alcuni dettagli.

Per questo motivo e per migliorare le prestazioni, è consigliabile limitare o ridurre al minimo il numero di programmi in un controller di dominio. Se non sono presenti richieste di memoria, LSASS usa questa memoria per memorizzare nella cache il database Active Directory e quindi ottenere prestazioni ottimali.

Quando si nota che un controller di dominio presenta problemi di prestazioni, osservare anche i processi con un utilizzo di memoria significativo. È possibile che si verifichi un problema di cui è necessario risolvere i problemi. Possono includere componenti Microsoft. Assicurarsi di rimanere aggiornati sugli aggiornamenti recenti del servizio @ no__t-0Microsoft include soluzioni per un utilizzo eccessivo della memoria nell'ambito degli aggiornamenti qualitativi, che possono anche aiutare le prestazioni del controller di dominio.

Sono disponibili funzionalità predefinite del sistema operativo che possono utilizzare RAM significativa a seconda del profilo di utilizzo:

- **File server**. I controller di dominio sono anche file server per le condivisioni SYSVOL e Netlogon, per la manutenzione di criteri di gruppo e script per i criteri e gli script di avvio/accesso.
  Tuttavia, i clienti usano i controller di dominio per soddisfare altri contenuti di file. Il file server SMB utilizzerebbe quindi RAM per tenere traccia dei client attivi, ma in primo luogo il contenuto del file renderebbe più grande la cache dei file del sistema operativo e concorrerebbe alla cache del database ESE per la RAM.  

- **Query WMI**. Le soluzioni di monitoraggio spesso fanno molte query WMI. È possibile eseguire una singola query in modo economico. Spesso si tratta del volume di chiamate che comporta un sovraccarico, soprattutto quando le soluzioni di monitoraggio estraggono i nuovi eventi dai vari registri eventi gestiti da Windows.  

  Il registro eventi che produce il maggior volume è in genere il registro eventi di protezione. Questo è anche il registro eventi che gli amministratori della sicurezza vogliono raccogliere, soprattutto dai controller di dominio.  

  Il servizio WMI utilizza uno schema di allocazione della memoria dinamica che ottimizza le query. Il servizio WMI, pertanto, può allocare una grande quantità di memoria, in competizione con la cache del database ESE.  
