---
title: Distribuire Network File System
description: Descrive come distribuire Network File System.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976687"
---
# Distribuire Network File System

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Network File System (NFS) offre una soluzione che ti consente di trasferire file tra i computer che eseguono Windows Server e sistemi operativi UNIX mediante il protocollo NFS di condivisione file. In questo argomento descrive i passaggi da seguire per distribuire NFS.

## Novità in Network File System

Ecco cosa è cambiato per NFS in Windows Server 2012:

- **Supporto per NFS versione 4.1**. Questa versione del protocollo include i miglioramenti seguenti.
  - Lo spostamento firewall è più facile, migliorando l'accessibilità.
  - Supporta il protocollo RPCSEC\_GSS, fornendo una maggiore protezione, consentendo la negoziazione della protezione client e server.
  - Supporta la semantica di file UNIX e Windows.
  - Sfrutta le distribuzioni di server di cluster di file.
  - Supporta le procedure composte WAN adatto.

- **Modulo NFS per Windows PowerShell**. La disponibilità di cmdlet integrati NFS rende più semplice automatizzare varie operazioni. I nomi di cmdlet sono coerenti con altri cmdlet di Windows PowerShell (con verbi, ad esempio "Ottenere" e "Set"), rendendo più semplice per gli utenti già familiarità con Windows PowerShell per informazioni su come utilizzare i cmdlet di nuovo.
- **Miglioramenti della gestione NFS**. Una nuova console di gestione centralizzata basata su dell'interfaccia utente semplifica la configurazione e gestione di SMB e NFS condivisioni, le quote, screening dei file e la classificazione, oltre alla gestione dei file server in cluster.
- **Miglioramenti di Mapping di identità**. Il nuovo supporto dell'interfaccia utente e i cmdlet di Windows PowerShell basata su attività per la configurazione di mapping di identità, che consente agli amministratori di configurare rapidamente un'origine di mapping di identità e quindi creare identità mappata singole per gli utenti. Miglioramenti rendono molto semplice per gli amministratori di configurare una condivisione per l'accesso a più protocollo su NFS e SMB.
- **Modello di risorsa cluster ristrutturare**. Questo miglioramento offre la coerenza tra il modello di risorsa cluster per NFS Windows e i server di protocollo SMB e semplifica l'amministrazione. Per i server NFS che dispongono di molte condivisioni, la rete di risorse e il numero di chiamate WMI richiesto il failover un volume che contiene che un numero elevato di condivisioni NFS è inferiori.
- **Integrazione con gestione di chiavi di ripresa**. Il gestore di ripresa chiave è un componente che tiene traccia di file server e stato del file system e consente di abilitare il server di protocollo SMB di Windows e NFS eseguire il failover senza interrompere i client o server applicazioni che archiviano i dati nel file server. Questo miglioramento è un componente fondamentale della funzionalità disponibilità continua del file server che eseguono Windows Server 2012.

## Scenari di utilizzo di Network File System

NFS supporta un ambiente misto dei sistemi operativi basati su Windows e UNIX-based. Esempi di come è possibile distribuire un server di file di Windows Server 2012 continuamente disponibile con NFS sono i seguenti scenari di distribuzione.

### Condivisioni di file di effettuare il provisioning in ambienti eterogenei

Questo scenario si applica alle organizzazioni con ambienti eterogenei costituiti da Windows e altri sistemi operativi, ad esempio UNIX o basato su Linux client i computer. Con questo scenario, puoi fornire multi-protocollo di accesso alla condivisione file stesso su SMB sia NFS protocolli. In genere, quando si distribuisce un file server di Windows in questo scenario, che vuoi agevolare la collaborazione tra gli utenti in Windows e i computer basati su UNIX. Quando viene configurata una condivisione file, vengono condivise con i protocolli SMB sia NFS, con accesso ai loro file tramite il protocollo SMB, gli utenti di Windows e gli utenti nei computer basati su UNIX in genere accedono ai file tramite il protocollo NFS.

Per questo scenario, è necessario disporre di una configurazione di origine mapping identità valida. Windows Server 2012 supporta i seguenti archivi di mapping di identità:

- File di mapping
- Servizi di dominio Active Directory
- RFC 2307 conforme LDAP Archivia, ad esempio Active Directory Lightweight Directory Services (AD LDS)
- Server Name Mapping (UNM) utente

### Condivisioni di file di effettuare il provisioning in ambienti UNIX

In questo scenario, file server Windows vengono distribuiti in un ambiente prevalentemente basate su UNIX per consentire l'accesso alle condivisioni di file NFS per i computer client basati su UNIX. Un'opzione di accesso di utente non mappato UNIX (UUUA) è stata implementata inizialmente per le condivisioni NFS in Windows Server 2008 R2 in modo che Windows Server possono essere usati per archiviare dati NFS senza creare UNIX a Windows di account mapping. UUUA consente agli amministratori di effettuare il provisioning e distribuire NFS senza la necessità di configurare il mapping di account rapidamente. Quando è abilitata per NFS, UUUA Crea identificatori di sicurezza personalizzate (SID) per rappresentare gli utenti non mappati. Gli account utente mappati utilizzano identificatori di sicurezza Windows standard (SID) e gli utenti non mappati SID NFS personalizzati.

## Requisiti di sistema

Server per NFS può essere installato in qualsiasi versione di Windows Server 2012. È possibile utilizzare NFS con basate su UNIX i computer che eseguono un client per NFS o un server NFS se queste implementazioni NFS server e client conformi con una delle specifiche del protocollo seguenti:

1. NFS versione 4.1 specifica del protocollo (come definito in RFC [5661](https://tools.ietf.org/html/rfc5661))
2. NFS versione 3 specifica del protocollo (come definito in RFC [1813](https://tools.ietf.org/html/rfc1813))
3. NFS versione 2 specifica del protocollo (come definito in RFC [1094](https://tools.ietf.org/html/rfc1094))

## Distribuzione dell'infrastruttura NFS

È necessario distribuire i computer seguenti e connetterli in una rete locale (LAN):

- Uno o più computer che eseguono Windows Server 2012 in cui installi i servizi principali due componenti NFS: Server per NFS e Client per NFS. È possibile installare questi componenti nello stesso computer o in computer diversi.
- Uno o più basate su UNIX i computer che eseguono server NFS e software client NFS. Il computer basati su UNIX che esegue server NFS ospita una condivisione file NFS o l'esportazione, che è accessibile da un computer che esegue Windows Server 2012 come client usando Client per NFS. È possibile installare il software client e server NFS nello stesso computer basati su UNIX o nei diversi computer basati su UNIX, in base alle esigenze.
- Un controller di dominio che esegue a livello funzionale di Windows Server 2008 R2. Il controller di dominio offre informazioni di autenticazione utente e il mapping per l'ambiente di Windows.
- Quando un controller di dominio non è stato distribuito, è possibile utilizzare un server di rete NIS (Information Service) per fornire informazioni di autenticazione utente per l'ambiente UNIX. In alternativa, se preferisci, puoi usare la Password e gruppo di file che vengono archiviati nel computer che esegue il servizio di Mapping nomi utente.

### Installare Network File System nel server con Server Manager

1. Se non è già stato installato dall'Aggiunta guidata ruoli e funzionalità, in ruoli del Server, seleziona **servizi File e archiviazione** .
2. In **File e iSCSI Services**, seleziona **File Server** e **Server per NFS**. Seleziona **Aggiungi funzionalità** da includere funzionalità NFS selezionata.
3. Seleziona **installare** per installare i componenti NFS nel server.

### Installare Network File System nel server con Windows PowerShell

1. Avviare Windows PowerShell. Pulsante destro del mouse sull'icona di PowerShell nella barra delle applicazioni e scegliere **Esegui come amministratore**.
2. Eseguire i comandi di Windows PowerShell seguenti:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## Configurare l'autenticazione NFS

Quando usi la versione NFS 4.1 e i protocolli versione 3.0 NFS, hai le seguenti opzioni di sicurezza e autenticazione.

- RPCSEC\_GSS
  - **Krb5**. Usa il protocollo Kerberos versione 5 per autenticare gli utenti prima di concedere l'accesso alla condivisione file.
  - **Krb5i**. Usa il protocollo Kerberos versione 5 per l'autenticazione (checksum), verifica dell'integrità che verifica che i dati non è stati alterati.
  - **Krb5p** Utilizza il protocollo versione 5 di Kerberos, che autentica il traffico NFS con la crittografia per la privacy.
- AUTH\_SYS

Puoi anche scegliere di non utilizzare l'autorizzazione di server (AUTH\_SYS), che ti offre la possibilità di abilitare l'accesso non mappato. Quando si utilizza l'accesso degli utenti non mappato, è possibile specificare per consentire l'accesso dell'utente non mappato da UID / GID, che è il valore predefinito o consentire l'accesso anonimo.

Istruzioni per la configurazione dell'autenticazione NFS su descritto nella sezione seguente.

## Creare una condivisione di file NFS

È possibile creare una condivisione di file NFS tramite i cmdlet di Server Manager o Windows PowerShell NFS.

### Creare una condivisione di file NFS con Server Manager

1. Accedi al server come un membro del gruppo Administrators locale.
2. Server Manager verrà avviata automaticamente. Se non viene automaticamente avviato, seleziona **Start**, digita **servermanager.exe**e quindi seleziona **Server Manager**.
3. A sinistra, seleziona **servizi File e archiviazione**e quindi seleziona **le condivisioni**.
4. Seleziona **per creare una condivisione file, avviare la creazione guidata nuova condivisione**.
5. Nella pagina **Seleziona profilo** , seleziona **Condivisione NFS-Quick** o **condivisione NFS - Advanced**e quindi seleziona **successivo**.
6. Nella pagina **Percorso di condivisione** , selezionare un server e un volume e seleziona **successivo**.
7. Nella pagina **Nome della condivisione** , specifica un nome per la condivisione di nuovo e seleziona **successivo**.
8. Nella pagina di **autenticazione** , specificare il metodo di autenticazione che si desidera utilizzare per la condivisione.
9. Nella pagina **Delle autorizzazioni di condivisione** , scegli **Aggiungi**e quindi specificare l'host, gruppo client o per netgroup che vuoi concedere l'autorizzazione per la condivisione.
10. **Le autorizzazioni**, configurare il tipo di controllo di accesso che degli utenti di avere e seleziona **OK**.
11. Nella pagina di **Conferma** , rivedere la configurazione e selezionare **Crea** per creare la condivisione di file NFS.

### Comandi equivalenti di Windows PowerShell

Il seguente cmdlet Windows PowerShell può inoltre creare una condivisione di file NFS (in cui `nfs1` è il nome della condivisione e `C:\\shares\\nfsfolder` è il percorso del file):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### Problema noto
NFS versione 4.1 consente i nomi dei file essere creato o copiato utilizzando caratteri non validi. Se si tenta di aprire i file in editor vi, Mostra come danneggiati. Non è possibile salvare il file da vi, rinominare, spostarlo o modificare le autorizzazioni. Evita di usare illigal caratteri.
