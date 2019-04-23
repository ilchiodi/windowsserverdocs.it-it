---
title: Panoramica di Network File System
description: Spiega che cos'è File System di rete.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876302"
---
# <a name="network-file-system-overview"></a>Panoramica di Network File System

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive il servizio ruolo File System di rete e sulle funzionalità incluse con il ruolo server Servizi File e archiviazione in Windows Server. File System NFS (Network) offre una soluzione per le aziende con ambienti eterogenei che includono sia Windows che ai computer non Windows di condivisione file.

## <a name="feature-description"></a>Descrizione delle caratteristiche

Usa il protocollo NFS, è possibile trasferire file tra computer che eseguono Windows e altri sistemi operativi non Windows, ad esempio Linux o UNIX.

NFS in Windows Server include Server per NFS e Client per NFS. Un computer che esegue Windows Server può utilizzare Server per NFS per agire come un file server NFS per altri computer client non Windows. Client per NFS consente a un computer basato su Windows che esegue Windows Server per accedere ai file archiviati in un server NFS di Windows non.

## <a name="windows-and-windows-server-versions"></a>Versioni di Windows e Windows Server

Windows supporta più versioni del client NFS e server, a seconda della versione del sistema operativo e della famiglia. 

| Sistemi operativi | Versioni di Server NFS |Versioni Client NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Applicazioni pratiche

Ecco alcuni modi per utilizzare NFS:

- Usare un file server NFS di Windows per fornire l'accesso multi-protocol alla stessa condivisione file tramite protocolli SMB e NFS dai client di multi-piattaforma.
- Distribuire un file server NFS Windows in un ambiente del sistema operativo prevalentemente non Windows per fornire accesso dei computer alle condivisioni file NFS client non Windows.
- Eseguire la migrazione delle applicazioni da un sistema operativo a un altro, archiviando i dati nelle condivisioni file accessibili tramite i protocolli SMB e NFS.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

Funzionalità nuove e modificate nel File System di rete include il supporto per NFS versione 4.1 e ottimizzazione di distribuzione e gestibilità. Per informazioni sulle funzionalità nuove o modificate in Windows Server 2012, esaminare la tabella seguente:

|Caratteristica/funzionalità|Novità o aggiornamento|Descrizione|
|---|---|---|
|[NFS versione 4.1](#nfs-version-4.1)|Nuova|Una maggiore sicurezza, prestazioni e l'interoperabilità rispetto a NFS versione 3.|
|[Infrastruttura NFS](#nfs-infrastructure)|Aggiornamento|Migliora la distribuzione e la gestibilità e aumenta la protezione.|
|[Disponibilità continua NFS versione 3](#nfs-version-3-continuous-availability)|Aggiornamento|Consente di migliorare la disponibilità continua sul client NFS versione 3.|
|[Miglioramenti della distribuzione e facilità di gestione](#deployment-and-manageability-improvements)|Aggiornamento|Consente di distribuire e gestire NFS con nuovi cmdlet di Windows PowerShell e un nuovo provider WMI facilmente.|

## <a name="nfs-version-41"></a>NFS versione 4.1

NFS versione 4.1 implementa tutti gli aspetti necessari, oltre a alcuni aspetti facoltativa, di [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **System file pseudo**, un file system che separa lo spazio dei nomi logico e fisico ed è compatibile con NFS versione 3 e NFS versione 2. Viene fornito un alias per il sistema di file esportato, che fa parte del file system pseudo.
- **Composta RPC** combinare delle relative operazioni e ridurre la frammentarietà.
- **Le sessioni e sessione trunking** consente solo una semantica e la disponibilità continua e prestazioni migliori durante l'utilizzo di più reti tra i client NFS 4.1 e il Server NFS.

## <a name="nfs-infrastructure"></a>Infrastruttura NFS

Miglioramenti all'infrastruttura complessiva NFS in Windows Server 2012 sono descritte di seguito:

- Il **RPC Remote Procedure Call () / dati rappresentazione XDR (External)** infrastruttura di trasporto, con tecnologia dal protocollo di rete WinSock, è disponibile per entrambi i Server per NFS e Client per NFS. Ciò sostituisce dispositivo interfaccia TDI (Transport), offre un supporto migliore e fornisce una migliore scalabilità e Receive-Side Scaling (RSS).
- Il **porta RPC multiplexer** funzionalità è adatto al firewall (numero inferiore di porte per la gestione) e semplifica la distribuzione di NFS.
- **Le cache ottimizzati automaticamente e i pool di thread** sono risorse di funzionalità di gestione della nuova infrastruttura RPC o dello schema XDR che sono dinamici, automaticamente l'ottimizzazione delle cache e il thread di pool in base al carico di lavoro. Questa operazione rimuove completamente le incognite coinvolti durante l'ottimizzazione dei parametri, offrendo prestazioni ottimali, non appena viene distribuita NFS.
- **Nuovo Kerberos implementazione e l'autenticazione di opzioni di privacy** con l'aggiunta del supporto di privacy (Krb5p) Kerberos insieme ai krb5 esistenti e le opzioni di autenticazione krb5i.
- **Modulo di PowerShell di Windows di Mapping delle identità** cmdlet rendono più semplice gestire i mapping delle identità, configurare Active Directory Lightweight Directory Services (AD LDS) e configurare i file flat e delle password UNIX e Linux.
- **Punto di montaggio del volume** consente di accedere ai volumi montati in una condivisione NFS con NFS versione 4.1.
- Il **Multiplexing porta** funzionalità supporta la porta RPC multiplexer (porta 2049), che è adatto al firewall e semplifica la distribuzione di NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilità continua NFS versione 3

Client NFS versione 3 può avere i failover pianificati veloci e trasparenti con maggiore disponibilità e tempo di inattività ridotto. Il processo di failover è più veloce per i client NFS versione 3 perché:

- L'infrastruttura di clustering consente ora una risorsa per ogni nome di rete invece di una risorsa per ogni condivisione, che migliora significativamente il tempo di failover delle risorse.
- I percorsi di failover all'interno di un server NFS sono ottimizzati per prestazioni migliori.
- Registrazione con caratteri jolly in un server NFS non è più necessaria, e i failover sono più sofisticati.
- Vengono inviate le notifiche di monitoraggio dello stato (Intrusion Detection System) di rete dopo un processo di failover e i client non è più necessitino attendere il timeout TCP per riconnettersi a è stato effettuato il failover di server.

Si noti che Server per NFS supporta il failover trasparente solo quando viene avviato manualmente, in genere durante la manutenzione pianificata. Se si verifica di errore imprevisto, i client NFS perdono le connessioni. Server per NFS non prevede anche l'integrazione con il filtro di chiavi di ripristino. Ciò significa che se un'app locale o una sessione di SMB tenta di accedere allo stesso file al quale ha eseguito l'accesso il client NFS subito dopo un failover pianificato, è possibile che il client NFS perda le connessioni e il failover trasparente non venga eseguito correttamente.

## <a name="deployment-and-manageability-improvements"></a>Miglioramenti della distribuzione e facilità di gestione

Distribuzione e la gestione di NFS sono migliorate nei modi seguenti:

- Nuovi cmdlet di Windows PowerShell oltre 40 rendono più semplice configurare e gestire condivisioni file NFS. Per altre informazioni, vedere [cmdlet per NFS in Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Mapping delle identità è stato migliorato con un archivio di mapping di file flat locale e nuovi cmdlet di Windows PowerShell per la configurazione di mapping delle identità.
- L'interfaccia utente grafica di Server Manager è più facile da usare.
- Il nuovo provider WMI versione 2 è disponibile per una gestione più semplice.
- La porta multiplexer (porta RPC 2049) è adatto al firewall e semplifica la distribuzione di NFS.

## <a name="server-manager-information"></a>Informazioni su Server Manager

In Server Manager - o la versione più recente [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -usare l'aggiunta guidata ruoli e funzionalità per aggiungere il Server per il servizio ruolo NFS (in File e iSCSI ruolo di servizi). Per informazioni generali sull'installazione delle funzionalità, vedere [Installare o disinstallare ruoli, servizi ruolo o funzionalità](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Server per gli strumenti NFS includono i servizi per lo snap-in di MMC di Network File System per gestire il Server per NFS e Client per i componenti NFS. Utilizzando lo snap-in, è possibile gestire il Server per i componenti NFS installato nel computer. Server per NFS contiene inoltre numerosi strumenti di amministrazione da riga di comando di Windows:

- **Montare** Monta una condivisione NFS remota (noto anche come un'esportazione) in locale e ne esegue il mapping a una lettera di unità locale nel computer client Windows.
- **Nfsadmin** gestisce le impostazioni di configurazione del Server per NFS e Client per i componenti NFS.
- **Nfsshare** Configura le impostazioni di condivisione NFS per cartelle condivise usano Server per NFS.
- **Nfsstat** consente di visualizzare o Reimposta le statistiche per le chiamate ricevute dal Server per NFS.
- **Showmount** consente di visualizzare i sistemi di file montati esportati dal Server per NFS.
- **Umount** rimuove le unità montate NFS.

NFS in Windows Server 2012 introduce il modulo NFS per Windows PowerShell con diversi nuovi cmdlet in modo specifico per NFS. Questi cmdlet forniscono un modo semplice per automatizzare le attività di gestione di NFS. Per altre informazioni, vedere [cmdlet per NFS in Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Altre informazioni

Nella tabella seguente elenca ulteriori risorse per la valutazione NFS.

|Tipo di contenuto|Riferimenti|
|---|---|
|Distribuzione|[Distribuire Network File System](deploy-nfs.md)|
|Operazioni|[Cmdlet per NFS in Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Tecnologie correlate|[Archiviazione in Windows Server](../storage.md)|