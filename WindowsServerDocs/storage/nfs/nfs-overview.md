---
title: Panoramica di Network File System
description: Spiega qual Network File System.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: bad0692ffd0773f61ac62bd140a421cb02c68acf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099140"
---
# Panoramica di Network File System

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive il servizio ruolo di Network File System e sulle funzionalità incluse con il ruolo server Servizi File e archiviazione in Windows Server. Network File System (NFS) offre una soluzione per le aziende con ambienti eterogenei che includono i computer Windows e non Windows di condivisione file.

## Descrizione delle caratteristiche

Mediante il protocollo NFS, è possibile trasferire file tra i computer che eseguono Windows e altri sistemi operativi non Windows, ad esempio Linux o UNIX.

NFS in Windows Server include Server per NFS e Client per NFS. Un computer che esegue Windows Server è possibile utilizzare Server per NFS per fungere da un file server NFS per altri computer client non Windows. Client per NFS consente a un computer basato su Windows che esegue Windows Server per accedere ai file archiviati su un server - Windows NFS.

## Versioni di Windows e Windows Server

Windows supporta più versioni del client per NFS e del server, a seconda della versione del sistema operativo e familiari. 

| Sistemi operativi | Versioni Server NFS |Versioni Client NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv4.1 NFSv2, NFSv3,  | NFSv2, NFSv3 |

## Applicazioni pratiche

Ecco alcuni modi per usare NFS:

- Usa un file server Windows NFS per consentire l'accesso multi-protocollo alla condivisione di file stessi su protocolli SMB sia NFS dai client multipiattaforma.
- Distribuire un file server Windows NFS in un ambiente del sistema operativo prevalentemente non Windows per fornire l'accesso alle condivisioni di file NFS i computer client non Windows.
- Eseguire la migrazione di applicazioni da un sistema operativo a un altro archiviando i dati nelle condivisioni di file accessibile tramite SMB sia NFS protocolli.

## Funzionalità nuove e modificate

Funzionalità nuove e modificate in Network File System include il supporto per la versione 4.1 NFS e la distribuzione avanzata e gestibilità. Per informazioni sulle funzionalità nuove o modificate di Windows Server 2012, esamina la tabella seguente:

|Caratteristica/funzionalità|Novità o aggiornamento|Descrizione|
|---|---|---|
|[NFS versione 4.1](#nfs-version-4.1)|Nuovo|Maggiore sicurezza, prestazioni e interoperabilità rispetto a NFS versione 3.|
|[Infrastruttura NFS](#nfs-infrastructure)|Aggiornato|Migliora la distribuzione e la gestibilità e aumenta la sicurezza.|
|[Disponibilità continua versione 3 NFS](#nfs-version-3-continuous-availability)|Aggiornato|Migliora la disponibilità continua in client NFS versione 3.|
|[Miglioramenti della distribuzione e la facilità di gestione](#deployment-and-manageability-improvements)|Aggiornato|Consente di distribuire e gestire NFS con nuovi cmdlet Windows PowerShell e un nuovo provider WMI.|

## NFS versione 4.1

NFS versione 4.1 implementa tutti gli aspetti necessari, oltre ad alcuni degli aspetti facoltativi, di [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Sistema di file Pseudo**, un file system che separa fisica e logica dello spazio dei nomi ed è compatibile con NFS versione 3 e NFS versione 2. Viene fornito un alias per il sistema esportato, che fa parte del file system pseudo.
- **RPC composta** combinare le operazioni pertinenti e ridurre la facilità di comunicazione.
- **Le sessioni e raggruppamento di sessione** consente solo una semantica e consente di disponibilità continua e prestazioni migliori durante l'utilizzo di più reti tra i client NFS 4.1 e il Server NFS.

## Infrastruttura NFS

Miglioramenti all'infrastruttura NFS complessivo in Windows Server 2012 sono descritte di seguito:

- Il **RPC Remote Procedure Call () / rappresentazione dati esterni (XDR)** infrastruttura di trasporto, basate sul protocollo di rete WinSock, è disponibile per entrambi i Server per NFS e Client per NFS. Questo sostituisce dispositivo TDI (Transport Interface), offre un supporto migliore e offre una migliore scalabilità e ricevere lato ridimensionamento (RSS).
- La funzionalità di **porta RPC multiplatore** è adatto firewall (meno porte per gestire) e semplifica la distribuzione di NFS.
- **Cache di ottimizzazione automatica e i pool di thread** sono funzionalità di gestione di risorse della nuova infrastruttura RPC/XDR che sono dinamici, automaticamente ottimizzazione cache e in base al carico di lavoro di pool di thread. Ciò elimina completamente l'ipotesi coinvolti quando parametri, offrendo prestazioni ottimali, non appena viene distribuito NFS per l'ottimizzazione.
- **Opzioni di implementazione e autenticazione privacy nuovo Kerberos** con l'aggiunta del supporto di privacy (Krb5p) Kerberos oltre al krb5 esistente e opzioni di autenticazione krb5i.
- I cmdlet di **modulo Windows PowerShell di identità Mapping** rendono più semplice gestire il mapping di identità, configurare Active Directory Lightweight Directory Services (AD LDS) e impostare i file flat e delle password UNIX e Linux.
- Consente a **un punto di montaggio volume** di accedere ai volumi montati in una condivisione NFS con NFS versione 4.1.
- La funzionalità di **Multiplexing porta** supporta il RPC multiplatore (porta 2049), che è adatto firewall e semplifica la distribuzione NFS.

## Disponibilità continua versione 3 NFS

NFS versione 3 client possono disporre veloce e trasparente failover pianificato maggiore disponibilità e la riduzione dei tempi di inattività. Il processo di failover è più veloce per i client versione 3 NFS perché:

- L'infrastruttura cluster consente ora di una risorsa per ogni nome di rete invece di una risorsa per ogni condivisione, che migliora notevolmente il tempo di failover delle risorse.
- I percorsi di failover all'interno di un server NFS sono ottimizzati per ottenere prestazioni migliori.
- Registrazione con caratteri jolly in un server NFS è più necessaria, e il failover sono più ottimizzate.
- Le notifiche di stato Monitor (NSM) di rete vengono inviate dopo un processo di failover e i client non devono più attendere TCP timeout riconnettere per il volume su server.

Tieni presente che il Server per NFS supporti failover trasparente solo quando è attivato manualmente, in genere durante la manutenzione pianificata. Se si verifica un failover non pianificato, i client NFS perdere le connessioni. Server per NFS anche non dispone di qualsiasi integrazione con il filtro di chiave di ripristino. Ciò significa che se un'app locali o una sessione SMB tenta di accedere al file stesso che accede a un client per NFS subito dopo un failover pianificato, il client per NFS potrebbero perdere le connessioni (trasparente failover non sarebbe esito negativo).

## Miglioramenti della distribuzione e la facilità di gestione

Distribuzione e gestione NFS è migliorato nei modi seguenti:

- Oltre a 40 nuovi cmdlet Windows PowerShell rendono più semplice configurare e gestire le condivisioni di file NFS. Per altre informazioni, vedi [NFS i cmdlet di Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Mapping di identità è stato migliorato con un archivio di mapping di file flat locale e i nuovi cmdlet Windows PowerShell per la configurazione di mapping di identità.
- L'interfaccia utente grafica di Server Manager è più facile da usare.
- Il nuovo provider WMI versione 2 è disponibile per una gestione più semplice.
- La porta multiplatore (porta RPC 2049) è adatto firewall e semplifica la distribuzione di NFS.

## Informazioni su Server Manager

In Server Manager - oppure il più recente di [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) - usare l'aggiunta guidata ruoli e funzionalità per aggiungere il Server per il servizio ruolo NFS (in File e iSCSI ruolo Servizi). Per informazioni generali sull'installazione delle funzionalità, vedi [l'installazione o disinstallazione di ruoli, servizi ruolo o funzionalità](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Server per gli strumenti NFS includono i servizi di snap-in di MMC di Network File System per gestire il Server per NFS e Client per i componenti NFS. Utilizzando lo snap-in, è possibile gestire il Server per i componenti NFS installati nel computer. Server per NFS contiene anche alcuni strumenti di amministrazione della riga di comando di Windows:

- **Montare** consente di montare una condivisione NFS remota (noto anche come un'esportazione) in locale e ne esegue il mapping di una lettera di unità locale nel computer client Windows.
- **Nfsadmin** gestisce le impostazioni di configurazione del Server per NFS e Client per i componenti NFS.
- **Nfsshare** Configura le impostazioni di condivisione NFS per le cartelle che vengono condivisi tramite Server per NFS.
- **Nfsstat** Visualizza o Reimposta statistiche di chiamate ricevute dal Server per NFS.
- **Showmount** Visualizza sistemi file montato esportati dal Server per NFS.
- **Umount** rimuove le unità montata NFS.

NFS in Windows Server 2012 introduce il modulo NFS per Windows PowerShell con diversi nuovi cmdlet in modo specifico per NFS. Questi cmdlet forniscono un modo semplice per automatizzare le attività di gestione NFS. Per altre informazioni, vedi [NFS i cmdlet di Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## Altre informazioni

La tabella seguente fornisce risorse aggiuntive per la valutazione NFS.

|Tipo di contenuto|Riferimenti|
|---|---|
|Distribuzione|[Distribuire Network File System](deploy-nfs.md)|
|Operazioni|[Cmdlet NFS in Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Tecnologie correlate|[Archiviazione in WindowsServer](../storage.md)|