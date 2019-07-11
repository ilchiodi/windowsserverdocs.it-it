---
title: Considerazioni sull'utilizzo di memoria nell'ottimizzazione delle prestazioni di Active Directory Domain Services
description: Utilizzo di memoria di processo Lsass.exe nei controller di dominio che eseguono Windows Server 2012 R2, 2016 e 2019.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: dd33124d7d480f3670fa2781f567eaaa28abbce6
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799783"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Considerazioni sull'utilizzo di memoria per ottimizzare le prestazioni di Active Directory Domain Services

Questo articolo descrive alcuni concetti di base di Local Security Authority Subsystem Service (LSASS, noto anche come processo Lsass.exe), procedure consigliate per la configurazione di LSASS e le aspettative per l'utilizzo di memoria. Questo articolo deve essere usato come guida durante l'analisi dell'utilizzo di memoria e prestazioni LSASS nei controller di dominio (DC). Le informazioni contenute in questo articolo possono essere utile se hai domande su come ottimizzare e configurare i server e i controller di dominio per ottimizzare questo motore.  

LSASS è responsabile della gestione dell'autenticazione di dominio di sicurezza locali authority (LSA) e la gestione di Active Directory. LSASS gestisce l'autenticazione per il client e il server e determina anche il modulo di Active Directory. LSASS è responsabile per i componenti seguenti:  

- Autorità di sicurezza locale
- Servizio NetLogon
- Servizio di gestione di account (SAM) di sicurezza
- Servizio Server LSA
- Secure Sockets Layer (SSL)
- Protocollo di autenticazione Kerberos v5
- Protocollo di autenticazione NTLM
- Altri pacchetti di autenticazione che viene caricato in LSA

I servizi di database di Active Directory (NTDSAI.dll) funzionano con Extensible Storage Engine (ESE, ESENT. dll).

Ecco un grafico visuale LSASS di utilizzo della memoria in un controller di dominio:

![Diagramma dei componenti che utilizzano memoria LSASS](media/domain-controller-lsass-memory-usage.png)  

Consente di aumentare la quantità di memoria utilizzata da LSASS su un controller di dominio in base sull'utilizzo di Active Directory. Quando viene eseguita una query dei dati, viene memorizzato nella cache in memoria. Di conseguenza, è normale che LSASS usando una quantità di memoria che è maggiore della dimensione del file di database di Active Directory (NTDS. dit).

Come illustrato nel diagramma, utilizzo della memoria LSASS può essere suddivisi in più parti, tra cui la cache buffer del database ESE, l'archivio versione ESE e altri. Il resto di questo articolo fornisce informazioni approfondite ognuna di queste parti.

## <a name="ese-database-buffer-cache"></a>Cache buffer del database ESE  
L'utilizzo della memoria variabile più grande LSASS è la cache buffer del database ESE. Le dimensioni della cache possono variare da inferiori a 1 MB per le dimensioni dell'intero database. Poiché una cache più grande migliora le prestazioni, il motore di database di Active Directory (ESE) tenta di mantenere la cache di dimensioni corrispondenti. Anche se le dimensioni della cache variano con utilizzo elevato di memoria del computer, è la dimensione massima della cache del buffer di database ESE *solo* limitate dalla RAM fisica installata nel computer. Finché sono presenti altre richieste di memoria, la cache possa crescere le dimensioni del file di database Ntds. dit di Active Directory. Maggiore è il numero di database che può essere memorizzati nella cache, migliori le prestazioni del controller di dominio sarà.  
  
> [!NOTE]
> A causa della modalità che la memorizzazione nella cache di funzionamento dell'algoritmo, in un sistema a 64 bit in cui le dimensioni del database sono inferiori alla memoria RAM disponibile, la cache del database di espansione del database supera le dimensioni del database del 30-40%.

## <a name="ese-version-store"></a>Archivio versione ESE

Utilizzo di memoria variabile è l'archivio versione ESE (la parte rosso nel diagramma precedente). La quantità di memoria usata dipende se si dispone di versioni precedenti di Windows o Windows Server 2019.

- Nelle versioni di Windows Server anticipa 2019 Server Windows, per impostazione predefinita che Lsass può usare fino a circa 400 MB di memoria (in base al numero di CPU) in un computer a 64 bit per la versione ESE archiviare. Per altre informazioni sull'uso di archivio delle versioni vedere il blog ASKDS post Ryan Ries: [La versione Store chiamato e sono tutte all'esterno di bucket](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- In Windows Server 2019, questo processo viene semplificato e quando prima all'avvio del servizio NTDS, le dimensioni dell'archivio versione ESE ora sono pari a 10% della RAM fisica, con un minimo di 400MB e un massimo di 4GB. Per ottime informazioni dettagliate su questo e la risoluzione dei problemi di versione store, vedere un altro grande blog da Ryan Ries: [Deep Dive: Store versione ESE di Active Directory viene modificato nel Server 2019](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510).

## <a name="other-memory-use"></a>Altro utilizzo memoria

Infine, è presente codice, gli stack, gli heap e varie strutture di dati di dimensione fissa (ad esempio, la cache dello schema). La quantità di memoria utilizzata da LSASS può variare, a seconda del carico nel computer. Man mano che aumenta il numero di thread in esecuzione, aumenta il numero di stack di memoria. In Media, LSASS utilizza 100 MB a 300 MB di memoria per questi componenti fissi. Quando si installa una maggiore quantità di RAM, LSASS può utilizzare più RAM e una minore quantità di memoria virtuale.

**Limitare o ridurre al minimo il numero di programmi nel controller di dominio o aggiungere altra RAM dove appropriato**

Per ottenere prestazioni ottimali, LSASS richiede più RAM possibili in un controller di dominio specificato. LSASS cede RAM perché vengano richiesti altri processi. L'idea è ottimizzare le prestazioni di LSASS pur tenendo conto di altri processi che potrebbero essere eseguite in un computer. L'elenco dei programmi da prendere in considerazione include agenti di monitoraggio. Alcuni clienti hanno agenti separati per varie funzioni di server che può essere utilizzato una quantità notevole di risorse di memoria RAM. Alcuni possono eseguire molte query WMI, per cui si dispone di alcuni dettagli riportati di seguito.

Per questo motivo e per migliorare le prestazioni, è consigliabile limitare o ridurre al minimo il numero di programmi in un controller di dominio. Se non sono presenti richieste di memoria, tale memoria LSASS utilizza per memorizzare nella cache i database di Active Directory e quindi ottenere prestazioni ottimali.

Quando si nota che un controller di dominio ha problemi di prestazioni, anche di cercare processi con un utilizzo notevole quantità di memoria. Questi potrebbero avere un problema che è necessario risolvere i problemi. Possono includere componenti di Microsoft. Assicurarsi che rimanere al passo con gli aggiornamenti di manutenzione recenti&mdash;Microsoft include le soluzioni per l'utilizzo eccessivo della memoria durante gli aggiornamenti di qualità, che può essere utile anche le prestazioni del controller di dominio.

Esistono strutture del sistema operativo predefiniti che possono usare RAM significativo a seconda del profilo di utilizzo:

- **File server di**. I controller di dominio sono anche i file server per le condivisioni SYSVOL e Netlogon, criteri di gruppo e gli script di avvio o accesso e criteri di manutenzione.
  Tuttavia, noteremo i clienti di usare controller di dominio per altro contenuto di file del servizio. File server SMB quindi consentirebbe l'utilizzo di RAM per tenere traccia dei client attivi, ma tutto il contenuto del file renderebbero la cache del file del sistema operativo la crescita e competere con la cache del database ESE di RAM.  

- **Le query WMI**. Soluzioni di monitoraggio verificare spesso molte query WMI. Una singola query potrebbe essere conveniente per l'esecuzione. È spesso il volume di chiamate che comporta un sovraccarico, soprattutto quando le soluzioni di monitoraggio estrarre i nuovi eventi dai registri eventi diversi che gestisce Windows.  

  Il registro eventi che produce il maggior parte dei volume è in genere il registro eventi di sicurezza. E questo è anche nel registro eventi che gli amministratori della sicurezza da raccogliere, specialmente da controller di dominio.  

  Il servizio WMI utilizza uno schema di allocazione della memoria dinamica che consente di ottimizzare le query. Pertanto, il servizio WMI potrebbe allocare una grande quantità di memoria, in competizione nuovamente con la cache del database ESE.  
