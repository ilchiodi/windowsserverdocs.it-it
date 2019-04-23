---
title: Distribuire Network File System
description: Viene descritto come distribuire File System di rete.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860772"
---
# <a name="deploy-network-file-system"></a>Distribuire Network File System

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

File System NFS (Network) offre una soluzione che consente di trasferire file tra computer che eseguono Windows Server e sistemi operativi UNIX mediante il protocollo NFS di condivisione file. In questo argomento vengono descritti i passaggi da seguire per distribuire NFS.

## <a name="whats-new-in-network-file-system"></a>Nuove funzionalità di Network File System

Ecco cosa è cambiato per NFS in Windows Server 2012:

- **Supporto per NFS versione 4.1**. Questa versione del protocollo include i miglioramenti seguenti.
  - Navigazione firewall diventa più semplice, miglioramento dell'accesso facilitato.
  - Supporta il RPCSEC\_protocollo GSS, fornendo una maggiore sicurezza e consentendo ai client e server di negoziare la protezione.
  - Supporta la semantica di file di Windows e UNIX.
  - Sfrutta le distribuzioni cluster file server.
  - Supporta procedure composte orientata alle WAN.

- **Modulo NFS per Windows PowerShell**. La disponibilità di cmdlet per NFS predefiniti rende più semplice automatizzare varie operazioni. I nomi dei cmdlet sono coerenti con altri Windows i cmdlet di PowerShell (utilizzano verbi come "Get" e "Set"), rendendo più semplice per gli utenti hanno familiarità con Windows PowerShell per imparare a usare nuovi cmdlet.
- **Miglioramenti alla gestione di NFS**. Una nuova console di gestione centralizzata basata sull'interfaccia utente semplifica la configurazione e gestione di SMB e NFS condivisioni, delle quote, screening dei file e la classificazione, oltre a gestire file server in cluster.
- **Miglioramenti di Mapping identità**. Nuovo supporto dell'interfaccia utente e i cmdlet di Windows PowerShell basato su attività per la configurazione di mapping delle identità, che consente agli amministratori di configurare rapidamente un'origine del mapping identità e quindi creare singole identità mappata per gli utenti. Miglioramenti rendono più semplice per gli amministratori di configurare una condivisione per l'accesso multi-protocol tramite SMB e NFS.
- **Ristrutturazione di modello di risorse del cluster**. Questo miglioramento offre la coerenza tra il modello di risorse cluster per NFS di Windows e i server di protocollo SMB e semplifica l'amministrazione. Per i server NFS con numero di condivisioni, la rete di risorse e il numero di chiamate WMI necessarie failover di un volume che contiene che un numero elevato di condivisioni NFS è ridotte.
- **Integrazione con gestione di chiavi di ripresa**. La gestione di chiavi Resume è un componente che tiene traccia di file server e dello stato del sistema di file e consente i server di protocollo Windows SMB e NFS eseguire il failover senza interrompere le applicazioni server che archiviano i dati nel file server o client. Questo miglioramento è un componente chiave della funzionalità di disponibilità continua del file server che esegue Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Scenari per l'uso di Network File System

NFS supporta un ambiente misto di sistemi operativi basati su UNIX e Windows. Scenari di distribuzione seguenti sono esempi di come è possibile distribuire un server di file di Windows Server 2012 continuamente disponibile tramite NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Condivisioni file di effettuare il provisioning in ambienti eterogenei

Questo scenario si applica alle organizzazioni con ambienti eterogenei costituiti da altri sistemi operativi, ad esempio client basati su Linux o UNIX sia Windows computer. Con questo scenario, è possibile fornire accesso multi-protocol alla stessa condivisione file tramite protocolli SMB e NFS. In genere, quando si distribuisce un file server Windows in questo scenario, si desidera agevola la collaborazione tra gli utenti di Windows e i computer basati su UNIX. Quando è configurata una condivisione file, che è condiviso con i protocolli SMB e NFS, con utenti di Windows che accedono ai propri file tramite il protocollo SMB e agli utenti dei computer basati su UNIX, in genere accedono ai file tramite il protocollo NFS.

Per questo scenario, è necessario disporre di una configurazione origine del mapping identità valida. Windows Server 2012 supporta gli archivi di mapping di identità seguenti:

- File di mapping
- Servizi di dominio Active Directory
- LDAP 2307 conforme a RFC Archivia come Active Directory Lightweight Directory Services (AD LDS)
- Server Name Mapping (UNM) utente

### <a name="provision-file-shares-in-unix-based-environments"></a>Condivisioni file di effettuare il provisioning in ambienti basati su UNIX

In questo scenario, i file server Windows vengono distribuiti in un ambiente prevalentemente basati su UNIX per fornire l'accesso alle condivisioni file NFS per i computer client basati su UNIX. Un'opzione di accesso di utenti non mappati UNIX (UNMAPPED) inizialmente è stata implementata per le condivisioni NFS in Windows Server 2008 R2 in modo che Windows Server possono essere utilizzati per archiviare i dati NFS senza creare UNIX-a-Windows dell'account del mapping. UNMAPPED consente agli amministratori di rapidamente il provisioning e distribuire NFS senza la necessità di configurare il mapping di account. Quando è abilitato per NFS, UNMAPPED crea gli identificatori di sicurezza personalizzati (SID) per rappresentare gli utenti non mappati. Gli account utente con mapping utilizzano standard identificatori di sicurezza di Windows (SID) e gli utenti non mappati SID NFS personalizzato.

## <a name="system-requirements"></a>Requisiti di sistema

Server per NFS può essere installato in qualsiasi versione di Windows Server 2012. È possibile usare NFS con i computer basati su UNIX che vengono eseguito un server NFS o client NFS se queste implementazioni di client e server NFS è conforme con una delle specifiche dei protocolli seguenti:

1. Specifica del protocollo NFS versione 4.1 (come definito in RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Specifica del protocollo NFS versione 3 (come definito in RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Specifica del protocollo NFS versione 2 (come definito in RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Distribuire l'infrastruttura NFS

È necessario distribuire i seguenti computer e connetterle in una rete locale (LAN):

- Uno o più computer che esegue Windows Server 2012 in cui si installerà i due servizi principali per i componenti NFS: Server per NFS e Client per NFS. È possibile installare questi componenti nello stesso computer o in computer diversi.
- Uno o più computer UNIX che eseguono il software client NFS e server NFS. I computer basati su UNIX che esegue server NFS ospitata in una condivisione NFS o un'esportazione, cui è possibile accedere da un computer che esegue Windows Server 2012 come un client usando un Client per NFS. È possibile installare software server NFS e client nello stesso computer basati su UNIX o in computer diversi basati su UNIX, in base alle esigenze.
- Un controller di dominio che esegue a livello funzionale di Windows Server 2008 R2. Il controller di dominio fornisce le informazioni di autenticazione utente e il mapping per l'ambiente di Windows.
- Quando un controller di dominio non è stato distribuito, è possibile utilizzare un server di rete informazioni Service (NIS) per fornire informazioni di autenticazione utente per l'ambiente UNIX. In alternativa, se si preferisce, è possibile usare i file di Password e gruppi che vengono archiviati nel computer che esegue il servizio di Mapping nomi utente.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Installare Network File System nel server con Server Manager

1. Nell'Aggiunta guidata ruoli e funzionalità in Ruoli server selezionare **Servizi file e archiviazione** se è già stato installato.
2. Sotto **File e iSCSI servizi**, selezionare **File Server** e **Server per NFS**. Selezionare **Aggiungi funzionalità** per includere le funzionalità selezionate NFS.
3. Selezionare **installare** per installare i componenti NFS nel server.

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>Installare Network File System nel server con Windows PowerShell

1. Avviare Windows PowerShell. Fare clic con il pulsante destro del mouse sull'icona di PowerShell sulla barra delle applicazioni e quindi scegliere **Esegui come amministratore**.
2. Eseguire i comandi di Windows PowerShell seguenti:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>Configurare l'autenticazione NFS

Quando si usano i protocolli di versione 3.0 di NFS e NFS versione 4.1, sono disponibili le opzioni seguenti di autenticazione e sicurezza.

- RPCSEC\_GSS
  - **Krb5**. Usa il protocollo Kerberos versione 5 per autenticare gli utenti prima di concedere l'accesso alla condivisione file.
  - **Krb5i**. Usa il protocollo Kerberos versione 5 per l'autenticazione tramite il controllo (checksum), dell'integrità che verifica che i dati non siano stati alterati.
  - **Krb5p** protocollo versione 5 utilizza Kerberos, che viene autenticato il traffico NFS con crittografia per la privacy.
- AUTH\_SYS

È anche possibile scegliere di non usare l'autorizzazione server (AUTH\_SYS), che ti offre la possibilità di abilitare l'accesso utente non mappato. Quando si usa l'accesso utente non mappato, è possibile specificare per consentire l'accesso utente non mappato tramite UID / GID, che è il valore predefinito o consentire l'accesso anonimo.

Istruzioni per la configurazione dell'autenticazione NFS in descritti nella sezione seguente.

## <a name="create-an-nfs-file-share"></a>Creare una condivisione di file NFS

È possibile creare una condivisione di file NFS tramite i cmdlet di Server Manager o Windows PowerShell NFS.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Creare una condivisione di file NFS con Server Manager

1. Accedere al server come membro del gruppo Administrators locale.
2. Server Manager verrà avviato automaticamente. Se non viene automaticamente avviato, selezionare **avviare**, digitare **servermanager.exe**e quindi selezionare **Server Manager**.
3. A sinistra, selezionare **servizi File e archiviazione**, quindi selezionare **condivisioni**.
4. Selezionare **per creare una condivisione file, avviare la creazione guidata nuova condivisione**.
5. Nel **selezionare il profilo** pagina, selezionare **condivisione NFS-rapida** o **condivisione NFS - avanzate**, quindi selezionare **Avanti**.
6. Nel **percorso di condivisione** pagina, selezionare un server e un volume e selezionare **successivo**.
7. Nel **nome condivisione** pagina, specificare un nome per la nuova condivisione e selezionare **successivo**.
8. Nel **autenticazione** , specificare il metodo di autenticazione da usare per questa condivisione.
9. Nel **autorizzazioni condivisione** pagina, selezionare **Add**e quindi specificare l'host, gruppo di client o netgroup si desidera concedere l'autorizzazione per la condivisione.
10. Nelle **le autorizzazioni**, configurare il tipo di controllo di accesso, si desidera che gli utenti, e selezionare **OK**.
11. Nel **conferma** pagina, rivedere la configurazione e selezionare **crea** per creare la condivisione di file NFS.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell

Il seguente cmdlet Windows PowerShell possono anche creare una condivisione di file NFS (dove `nfs1` è il nome della condivisione e `C:\\shares\\nfsfolder` è il percorso del file):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problema noto
NFS versione 4.1 consente i nomi dei file essere creato o copiato con caratteri non validi. Se si tenta di aprire i file con l'editor vi, verrà visualizzato come danneggiato. È possibile salvare il file da vi, rinominare, spostare o modificare le autorizzazioni. Evitare di usare illigal caratteri.
