---
title: Distribuire Network File System
description: Viene descritto come distribuire il file System di rete.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: cc02f0a82b4143b80fc1107a63d234b117502d2d
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544647"
---
# <a name="deploy-network-file-system"></a>Distribuire Network File System

>Si applica a Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

NFS (Network File System) fornisce una soluzione di condivisione file che consente di trasferire file tra computer che eseguono sistemi operativi Windows Server e UNIX usando il protocollo NFS. In questo argomento vengono descritti i passaggi da seguire per distribuire NFS.

## <a name="whats-new-in-network-file-system"></a>Novità di Network File System

Ecco cosa è cambiato per NFS in Windows Server 2012:

- **Supporto per NFS versione 4,1**. Questa versione del protocollo include i miglioramenti seguenti.
  - L'esplorazione di firewall è più semplice e migliora l'accessibilità.
  - Supporta il protocollo\_GSS RPCSEC, garantendo una sicurezza più avanzata e consentendo a client e server di negoziare la protezione.
  - Supporta la semantica di file UNIX e Windows.
  - Sfrutta le distribuzioni di file server cluster.
  - Supporta procedure composte per la rete WAN.

- **Modulo NFS per Windows PowerShell**. La disponibilità dei cmdlet predefiniti di NFS rende più semplice l'automazione di varie operazioni. I nomi dei cmdlet sono coerenti con gli altri cmdlet di Windows PowerShell (usando verbi quali "Get" e "set"), semplificando gli utenti che hanno familiarità con Windows PowerShell per imparare a usare i nuovi cmdlet.
- **Miglioramenti alla gestione NFS**. Una nuova console di gestione centralizzata basata sull'interfaccia utente semplifica la configurazione e la gestione di condivisioni SMB e NFS, quote, schermate e classificazione dei file, oltre alla gestione di file server in cluster.
- **Miglioramenti al mapping delle identità**. Nuovo supporto dell'interfaccia utente e cmdlet di Windows PowerShell basati su attività per la configurazione del mapping delle identità, che consente agli amministratori di configurare rapidamente un'origine del mapping di identità e quindi di creare singole identità mappate per gli utenti. I miglioramenti semplificano la configurazione di una condivisione per l'accesso a più protocolli tramite NFS e SMB.
- Ristrutturare il **modello di risorse cluster**. Questo miglioramento garantisce la coerenza tra il modello di risorse cluster per i server del protocollo SMB e NFS di Windows e semplifica l'amministrazione. Per i server NFS con molte condivisioni, viene ridotta la rete delle risorse e il numero di chiamate WMI necessarie per eseguire il failover di un volume contenente un numero elevato di condivisioni NFS.
- **Integrazione con la gestione delle chiavi di ripresa**. Resume Key Manager è un componente che tiene traccia file server e file system stato e consente di eseguire il failover dei server del protocollo SMB e NFS di Windows senza interrompere i client o le applicazioni server che archiviano i dati nel file server. Questo miglioramento è un componente chiave della funzionalità di disponibilità continua del file server che esegue Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Scenari per l'utilizzo di file System di rete

NFS supporta un ambiente misto di sistemi operativi basati su Windows e UNIX. Gli scenari di distribuzione seguenti sono esempi di come è possibile distribuire un file server Windows Server 2012 costantemente disponibile usando NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Provisioning di condivisioni file in ambienti eterogenei

Questo scenario si applica alle organizzazioni con ambienti eterogenei che sono costituiti da Windows e altri sistemi operativi, ad esempio computer client basati su UNIX o Linux. Con questo scenario è possibile fornire l'accesso a più protocolli alla stessa condivisione file tramite i protocolli SMB e NFS. In genere, quando si distribuisce un file server Windows in questo scenario, si desidera semplificare la collaborazione tra gli utenti di computer basati su Windows e UNIX. Quando una condivisione file viene configurata, viene condivisa con i protocolli SMB e NFS, con gli utenti di Windows che accedono ai file tramite il protocollo SMB e gli utenti in computer basati su UNIX in genere accedono ai file tramite il protocollo NFS.

Per questo scenario, è necessario disporre di una configurazione di origine del mapping di identità valida. Windows Server 2012 supporta i seguenti archivi di mapping delle identità:

- File di mapping
- Servizi di dominio Active Directory
- Archivi LDAP conformi a RFC 2307, ad esempio Active Directory Lightweight Directory Services (AD LDS)
- Server di mapping dei nomi utente (UNM)

### <a name="provision-file-shares-in-unix-based-environments"></a>Effettuare il provisioning di condivisioni file in ambienti basati su UNIX

In questo scenario, i file server Windows vengono distribuiti in un ambiente basato su UNIX prevalentemente per consentire l'accesso alle condivisioni file NFS per i computer client basati su UNIX. È stata inizialmente implementata un'opzione UUUA (unmapped UNIX User Access) per le condivisioni NFS in Windows Server 2008 R2, in modo che sia possibile utilizzare i server Windows per archiviare i dati NFS senza creare il mapping degli account da UNIX a Windows. UUUA consente agli amministratori di effettuare rapidamente il provisioning e distribuire NFS senza dover configurare il mapping degli account. Se abilitata per NFS, UUUA crea identificatori di sicurezza (SID) personalizzati per rappresentare gli utenti non mappati. Gli account utente mappati utilizzano gli identificatori di sicurezza (SID) di Windows standard e gli utenti non mappati utilizzano SID NFS personalizzati.

## <a name="system-requirements"></a>Requisiti di sistema

Server per NFS può essere installato in qualsiasi versione di Windows Server 2012. È possibile utilizzare NFS con computer basati su UNIX che eseguono un server NFS o un client NFS se queste implementazioni di server e client NFS sono conformi a una delle seguenti specifiche del protocollo:

1. Specifica del protocollo NFS versione 4,1 (come definito in RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Specifica del protocollo NFS versione 3 (come definito nella RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Specifica del protocollo NFS versione 2 (come definito nella RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Distribuire l'infrastruttura NFS

È necessario distribuire i computer seguenti e connetterli a una rete locale (LAN):

- Uno o più computer che eseguono Windows Server 2012 in cui si installeranno i due componenti principali di servizi per NFS: Server per NFS e client per NFS. È possibile installare questi componenti nello stesso computer o in computer diversi.
- Uno o più computer basati su UNIX che eseguono NFS server e il software client NFS. Il computer basato su UNIX che esegue NFS server ospita una condivisione file o un'esportazione NFS, a cui è possibile accedere da un computer che esegue Windows Server 2012 come client utilizzando client per NFS. È possibile installare il software client e il server NFS nello stesso computer basato su UNIX o in computer UNIX diversi, in base alle esigenze.
- Un controller di dominio in esecuzione al livello di funzionalità di Windows Server 2008 R2. Il controller di dominio fornisce le informazioni di autenticazione utente e il mapping per l'ambiente Windows.
- Quando un controller di dominio non viene distribuito, è possibile utilizzare un server di Network Information Service (NIS) per fornire informazioni di autenticazione utente per l'ambiente UNIX. In alternativa, se si preferisce, è possibile utilizzare i file di gruppo e di password archiviati nel computer in cui è in esecuzione il servizio di mapping dei nomi utente.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Installare Network File System nel server con Server Manager

1. Nell'Aggiunta guidata ruoli e funzionalità in Ruoli server selezionare **Servizi file e archiviazione** se è già stato installato.
2. In **Servizi file e iSCSI**selezionare **file server** e **Server per NFS**. Selezionare **Aggiungi funzionalità** per includere le funzionalità NFS selezionate.
3. Selezionare **Installa** per installare i componenti NFS nel server.

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>Installare il file System di rete nel server con Windows PowerShell

1. Avviare Windows PowerShell. Fare clic con il pulsante destro del mouse sull'icona di PowerShell sulla barra delle applicazioni e quindi scegliere **Esegui come amministratore**.
2. Eseguire i comandi di Windows PowerShell seguenti:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>Configurare l'autenticazione NFS

Quando si usano i protocolli NFS versione 4,1 e NFS versione 3,0, sono disponibili le opzioni di autenticazione e sicurezza seguenti.

- RPCSEC\_GSS
  - **Krb5**. Usa il protocollo Kerberos versione 5 per autenticare gli utenti prima di concedere l'accesso alla condivisione file.
  - **Krb5i**. Usa il protocollo Kerberos versione 5 per l'autenticazione con il controllo di integrità (checksum), che verifica che i dati non siano stati modificati.
  - **Krb5p** Usa il protocollo Kerberos versione 5, che autentica il traffico NFS con la crittografia per la privacy.
- AUTH\_(SYS)

È anche possibile scegliere di non usare l'autorizzazione del server\_(auth sys), che offre l'opzione per abilitare l'accesso utente non mappato. Quando si usa l'accesso utente non mappato, è possibile specificare per consentire l'accesso utente non mappato da UID/GID, che è l'impostazione predefinita, o consentire l'accesso anonimo.

Istruzioni per la configurazione dell'autenticazione NFS in descritta nella sezione seguente.

## <a name="create-an-nfs-file-share"></a>Creare una condivisione file NFS

È possibile creare una condivisione file NFS usando i cmdlet Server Manager o Windows PowerShell NFS.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Creare una condivisione file NFS con Server Manager

1. Accedere al server come membro del gruppo Administrators locale.
2. Server Manager verrà avviato automaticamente. Se non viene avviato automaticamente, selezionare **Start**, digitare **ServerManager. exe**, quindi selezionare **Server Manager**.
3. A sinistra selezionare **Servizi file e archiviazione**e quindi selezionare condivisioni.
4. Selezionare **questa impostazione per creare una condivisione file e avviare la creazione guidata nuova condivisione**.
5. Nella pagina **Seleziona profilo** selezionare **condivisione NFS-rapida** o **condivisione NFS-avanzate**, quindi fare clic su **Avanti**.
6. Nella pagina **percorso condivisione** selezionare un server e un volume e fare clic su **Avanti**.
7. Nella pagina **nome condivisione** specificare un nome per la nuova condivisione e fare clic su **Avanti**.
8. Nella pagina **autenticazione** specificare il metodo di autenticazione che si desidera utilizzare per questa condivisione.
9. Nella pagina **autorizzazioni di condivisione** selezionare **Aggiungi**, quindi specificare l'host, il gruppo di client o netgroup per cui si desidera concedere l'autorizzazione alla condivisione.
10. In **autorizzazioni**configurare il tipo di controllo di accesso che si vuole che gli utenti dispongano e selezionare **OK**.
11. Nella pagina **conferma** verificare la configurazione e selezionare **Crea** per creare la condivisione file NFS.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell

Il cmdlet di Windows PowerShell seguente può anche creare una condivisione file NFS ( `nfs1` dove è il nome della condivisione ed `C:\\shares\\nfsfolder` è il percorso del file):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problema noto
NFS versione 4,1 consente di creare o copiare i nomi di file utilizzando caratteri non validi. Se si tenta di aprire i file con l'editor vi, viene visualizzato come danneggiato. Non è possibile salvare il file da vi, rinominarlo, spostarlo o modificare le autorizzazioni. Evitare l'utilizzo di caratteri non validi.
