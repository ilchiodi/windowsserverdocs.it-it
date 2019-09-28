---
title: Panoramica di Network File System
description: Viene illustrato il file System di rete.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 72f71bc6605103f8240bcd531da3a5b58d470181
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403048"
---
# <a name="network-file-system-overview"></a>Panoramica di Network File System

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive il servizio ruolo file System di rete e le funzionalità incluse nel ruolo server Servizi file e archiviazione in Windows Server. NFS (Network File System) fornisce una soluzione di condivisione file per le aziende che dispongono di ambienti eterogenei che includono computer Windows e non Windows.

## <a name="feature-description"></a>Descrizione delle caratteristiche

Con il protocollo NFS è possibile trasferire file tra computer che eseguono Windows e altri sistemi operativi non Windows, ad esempio Linux o UNIX.

NFS in Windows Server include server per NFS e client per NFS. Un computer che esegue Windows Server può utilizzare server per NFS per fungere da file server NFS per altri computer client non Windows. Client per NFS consente a un computer basato su Windows che esegue Windows Server di accedere ai file archiviati in un server NFS non Windows.

## <a name="windows-and-windows-server-versions"></a>Versioni di Windows e Windows Server

Windows supporta più versioni del client e del server NFS, a seconda della versione e della famiglia del sistema operativo. 

| Sistemi operativi | Versioni del server NFS |Versioni client NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/D | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv 4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Applicazioni pratiche

Di seguito sono riportati alcuni modi in cui è possibile utilizzare NFS:

- Usare un file server NFS di Windows per fornire l'accesso a più protocolli alla stessa condivisione file tramite i protocolli SMB e NFS dei client multipiattaforma.
- Distribuire un file server NFS di Windows in un ambiente di sistema operativo non Windows prevalentemente per fornire ai computer client non Windows l'accesso alle condivisioni file NFS.
- Eseguire la migrazione di applicazioni da un sistema operativo a un altro archiviando i dati nelle condivisioni file accessibili tramite protocolli SMB e NFS.

## <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

Le funzionalità nuove e modificate nel file System di rete includono il supporto per NFS versione 4,1 e una migliore distribuzione e gestibilità. Per informazioni sulle funzionalità nuove o modificate in Windows Server 2012, vedere la tabella seguente:

|Caratteristica/funzionalità|Novità o aggiornamento|Descrizione|
|---|---|---|
|[NFS versione 4,1](#nfs-version-41)|Nuova|Maggiore sicurezza, prestazioni e interoperabilità rispetto a NFS versione 3.|
|[Infrastruttura NFS](#nfs-infrastructure)|Aggiornamento|Migliora la distribuzione e la gestibilità e aumenta la sicurezza.|
|[Disponibilità continua di NFS versione 3](#nfs-version-3-continuous-availability)|Aggiornamento|Migliora la disponibilità continua nei client NFS versione 3.|
|[Miglioramenti della distribuzione e della gestibilità](#deployment-and-manageability-improvements)|Aggiornamento|Consente di distribuire e gestire facilmente NFS con i nuovi cmdlet di Windows PowerShell e con un nuovo provider WMI.|

## <a name="nfs-version-41"></a>NFS versione 4,1

NFS versione 4,1 implementa tutti gli aspetti richiesti, oltre ad alcuni aspetti facoltativi, di [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Pseudo file System**, file System che separa lo spazio dei nomi fisico e logico ed è compatibile con NFS versione 3 e NFS versione 2. Viene fornito un alias per il file system esportato, che fa parte dello pseudo file system.
- Le **RPC composte** combinano le operazioni rilevanti e riducono chattiness.
- Le **sessioni e il trunking** delle sessioni consentono una sola semantica e consentono una disponibilità continua e prestazioni migliori usando più reti tra i client NFS 4,1 e il server NFS.

## <a name="nfs-infrastructure"></a>Infrastruttura NFS

I miglioramenti apportati all'infrastruttura NFS complessiva in Windows Server 2012 sono descritti di seguito:

- L'infrastruttura di trasporto **XDR (Remote Procedure Call) (XDR)/External Data rappresentatività (XDR)** , basata sul protocollo di rete Winsock, è disponibile sia per server per NFS che per client per NFS. In questo modo viene sostituita la funzionalità TDI (Transport Device Interface), viene offerto un supporto migliore e viene garantita una migliore scalabilità e un RSS (Receive Side Scaling).
- La funzionalità del **multiplexer della porta RPC** è intuitiva (meno porte da gestire) e semplifica la distribuzione di NFS.
- Le **cache ottimizzate automaticamente e i pool di thread** sono funzionalità di gestione delle risorse della nuova infrastruttura RPC/XDR che sono dinamiche, ottimizzano automaticamente le cache e i pool di thread in base al carico di lavoro. In questo modo vengono rimosse completamente le ipotesi relative all'ottimizzazione dei parametri, garantendo prestazioni ottimali non appena viene distribuito NFS.
- **Nuove opzioni di implementazione e autenticazione della privacy Kerberos** con l'aggiunta del supporto per la privacy Kerberos (Krb5p) insieme alle opzioni di autenticazione krb5 e Krb5i esistenti.
- I cmdlet del **modulo di Windows PowerShell** per il mapping delle identità semplificano la gestione del mapping delle identità, la configurazione Active Directory Lightweight Directory Services (ad LDS) e la configurazione di file flat e passwd UNIX e Linux.
- Il **punto di montaggio del volume** consente di accedere ai volumi montati in una condivisione NFS con nfs versione 4,1.
- La funzionalità di **multiplexing delle porte** supporta il multiplexer della porta RPC (porta 2049), che è intuitivo per il firewall e semplifica la distribuzione NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilità continua di NFS versione 3

I client NFS versione 3 possono avere failover pianificati rapidi e trasparenti con maggiore disponibilità e tempi di inattività ridotti. Il processo di failover è più veloce per i client NFS versione 3 poiché:

- L'infrastruttura di clustering consente ora una risorsa per ogni nome di rete anziché una risorsa per condivisione, migliorando significativamente il tempo di failover delle risorse.
- I percorsi di failover in un server NFS sono ottimizzati per migliorare le prestazioni.
- La registrazione con caratteri jolly in un server NFS non è più necessaria e i failover sono ottimizzati.
- Le notifiche di Status Monitor di rete (NSM) vengono inviate dopo un processo di failover e i client non devono più attendere che i timeout TCP riconnettano al server di cui è stato eseguito il failover.

Si noti che Server per NFS supporta il failover trasparente solo quando viene avviato manualmente, in genere durante la manutenzione pianificata. Se si verifica di errore imprevisto, i client NFS perdono le connessioni. Anche il server per NFS non dispone di alcuna integrazione con il filtro della chiave di ripresa. Ciò significa che se un'app locale o una sessione di SMB tenta di accedere allo stesso file al quale ha eseguito l'accesso il client NFS subito dopo un failover pianificato, è possibile che il client NFS perda le connessioni e il failover trasparente non venga eseguito correttamente.

## <a name="deployment-and-manageability-improvements"></a>Miglioramenti della distribuzione e della gestibilità

La distribuzione e la gestione di NFS sono state migliorate nei modi seguenti:

- Oltre 40 nuovi cmdlet di Windows PowerShell semplificano la configurazione e la gestione delle condivisioni file NFS. Per altre informazioni, vedere [cmdlet per NFS in Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Il mapping delle identità è migliorato con un archivio di mapping di file flat locale e nuovi cmdlet di Windows PowerShell per la configurazione del mapping di identità.
- Il Server Manager interfaccia utente grafica è più semplice da usare.
- Il nuovo provider WMI versione 2 è disponibile per semplificare la gestione.
- Il multiplexer della porta RPC (porta 2049) è adatto al firewall e semplifica la distribuzione di NFS.

## <a name="server-manager-information"></a>Informazioni su Server Manager

In Server Manager o nell'interfaccia di [amministrazione di Windows](../../manage/windows-admin-center/understand/windows-admin-center.md) più recente usare l'aggiunta guidata ruoli e funzionalità per aggiungere il servizio ruolo server per NFS (nel ruolo Servizi file e iSCSI). Per informazioni generali sull'installazione delle funzionalità, vedere [Installare o disinstallare ruoli, servizi ruolo o funzionalità](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Gli strumenti server per NFS includono i servizi per lo snap-in MMC file System di rete per la gestione dei componenti server per NFS e client per NFS. Utilizzando lo snap-in è possibile gestire il server per i componenti NFS installati nel computer. Server per NFS contiene inoltre diversi strumenti di amministrazione della riga di comando di Windows:

- **Mount** monta una condivisione NFS remota (anche nota come esportazione) localmente e la associa a una lettera di unità locale nel computer client Windows.
- **Nfsadmin** gestisce le impostazioni di configurazione del server per i componenti NFS e client per NFS.
- **Nfsshare** configura le impostazioni di condivisione NFS per le cartelle condivise con server per NFS.
- **Nfsstat** Visualizza o Reimposta le statistiche delle chiamate ricevute da server per NFS.
- **Showmount** Visualizza i file system montati esportati da server per NFS.
- **Umount** rimuove le unità montate per NFS.

NFS in Windows Server 2012 introduce il modulo NFS per Windows PowerShell con diversi nuovi cmdlet specifici per NFS. Questi cmdlet forniscono un modo semplice per automatizzare le attività di gestione NFS. Per altre informazioni, vedere [cmdlet per NFS in Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Altre informazioni

Nella tabella seguente vengono fornite risorse aggiuntive per la valutazione di NFS.

|Tipo di contenuto|Riferimenti|
|---|---|
|Distribuzione|[Distribuire Network File System](deploy-nfs.md)|
|Operazioni|[Cmdlet di NFS in Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Tecnologie correlate|[Archiviazione in Windows Server](../storage.md)|