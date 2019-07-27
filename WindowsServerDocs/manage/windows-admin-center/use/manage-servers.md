---
title: Gestire i server con l'interfaccia di amministrazione di Windows
description: Gestire i server con l'interfaccia di amministrazione di Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 72524fcc71f722daeb8238bc3cffc6d38a611098
ms.sourcegitcommit: 9f955be34c641b58ae8b3000768caa46ad535d43
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/27/2019
ms.locfileid: "68590580"
---
# <a name="manage-servers-with-windows-admin-center"></a>Gestire i server con l'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>Gestione di computer Windows Server

È possibile aggiungere singoli server che eseguono Windows Server 2012 o versioni successive all'interfaccia di amministrazione di Windows per gestire il server con un set completo di strumenti, tra cui certificati, dispositivi, eventi, processi, ruoli e funzionalità, aggiornamenti, macchine virtuali e altro ancora.

![Schermata di panoramica della connessione al server](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>Aggiunta di un server all'interfaccia di amministrazione di Windows

Per aggiungere un server all'interfaccia di amministrazione di Windows:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **connessione al server**.
3. Digitare il nome del server e, se richiesto, le credenziali da utilizzare.
4. Fare clic su **Invia** per terminare.

Il server verrà aggiunto all'elenco di connessioni nella pagina panoramica. Fare clic su di esso per connettersi al server.

> [!NOTE]
> È anche possibile aggiungere [cluster di failover](manage-failover-clusters.md) o [cluster](manage-hyper-converged.md) iperconvergenti come una connessione separata nell'interfaccia di amministrazione di Windows.

## <a name="tools"></a>Strumenti

Per le connessioni server sono disponibili gli strumenti seguenti:

| Strumento | Descrizione |
| ---- | ----------- |
| [Panoramica](#overview) | Visualizza i dettagli del server e controlla lo stato del server |
| [Active Directory](#active-directory-preview) | Gestisci Active Directory |
| [Backup](#backup) | Visualizzare e configurare backup di Azure |  
| [Certificati](#certificates) | Visualizzare e modificare i certificati |
| [Contenitori](#containers) | Visualizza contenitori |
| [Dispositivi](#devices) | Visualizzare e modificare i dispositivi |
| [DHCP](#dhcp) | Visualizzare e gestire la configurazione del server DHCP |
| [DNS](#dns) | Visualizzare e gestire la configurazione del server DNS |
| [Eventi](#events) | Visualizza eventi |
| [File](#files) | Sfogliare file e cartelle |
| [Firewall](#firewall) | Visualizzare e modificare le regole del firewall |
| [App installate](#installed-apps) | Visualizzare e rimuovere le app installate |
| [Utenti e gruppi locali](#local-users-and-groups) | Visualizzare e modificare gruppi e utenti locali |
| [Network](#network) | Visualizzare e modificare i dispositivi di rete |
| [PowerShell](#powershell) | Interagire con il server tramite PowerShell |
| [Processi](#processes) | Visualizzare e modificare i processi in esecuzione |
| [Del registro](#registry) | Visualizzare e modificare le voci del registro di sistema |
| [Desktop remoto](#remote-desktop) | Interagire con il server tramite Desktop remoto |
| [Ruoli e funzionalità](#roles-and-features) | Visualizzazione e modifica di ruoli e funzionalità |
| [Attività pianificate](#scheduled-tasks) | Visualizzare e modificare le attività pianificate |
| [Servizi](#services) | Visualizzare e modificare i servizi |
| [Impostazioni](#settings) | Visualizzare e modificare i servizi |
| [Archiviazione](#storage) | Visualizzare e modificare i dispositivi di archiviazione |
| [Servizio migrazione archiviazione](#storage-migration-service) | Eseguire la migrazione di server e condivisioni file in Azure o Windows Server 2019 |
| [Replica di archiviazione](#storage-replica) | Usare replica archiviazione per gestire la replica di archiviazione da server a server |
| [Informazioni dettagliate di sistema](#system-insights) | System Insights offre una maggiore visibilità sul funzionamento del server. |
| [Aggiornamenti](#updates) | Visualizza installato e verifica la disponibilità di nuovi aggiornamenti |
| [Macchine virtuali](manage-virtual-machines.md) | Visualizzare e gestire le macchine virtuali |
| [Commutatori virtuali](#virtual-switches) | Visualizzare e gestire i commutatori virtuali |

## <a name="overview"></a>Panoramica

**Panoramica** consente di visualizzare lo stato corrente della CPU, della memoria e delle prestazioni di rete, nonché di eseguire operazioni e modificare le impostazioni in un computer o un server di destinazione.

### <a name="features"></a>Funzionalità

In Server Manager panoramica sono supportate le funzionalità seguenti:

- Visualizza dettagli server
- Visualizza attività CPU
- Visualizza attività memoria
- Visualizzare l'attività di rete
- Riavvia server
- Arresta server
- Abilita metrica disco nel server
- Modifica ID computer nel server
- Visualizzare l'indirizzo IP BMC con collegamento ipertestuale (richiede BMC compatibile con IPMI).

[**Visualizza commenti e suggerimenti e funzionalità proposte per la panoramica del server**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="active-directory-preview"></a>Active Directory (anteprima)

**Active Directory** è un'anteprima anticipata disponibile nel feed di [estensione](../configure/using-extensions.md).

### <a name="features"></a>Funzionalità

Sono disponibili le seguenti Active Directory gestione:

- Crea utente
- Crea gruppo
- Ricerca di utenti, computer e gruppi
- Riquadro dei dettagli per utenti, computer e gruppi se selezionato nella griglia
- Azioni griglia globale utenti, computer e gruppi (Disabilita/Abilita, Rimuovi)
- Reimpostare la password utente
- Oggetti utente: configurare le proprietà di base & le appartenenze ai gruppi
- Oggetti computer: configurare la delega in un singolo computer
- Oggetti gruppo: Gestisci appartenenza (Aggiungi/Rimuovi 1 utente alla volta)  

[**Visualizza commenti e suggerimenti e funzionalità proposte per Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## <a name="backup"></a>Backup

Il **backup** consente di proteggere il server Windows da danneggiamenti, attacchi o emergenze eseguendo il backup del server direttamente in Microsoft Azure.
[Altre informazioni su backup di Azure.](https://aka.ms/windows-admin-center-backup)

[Inviare commenti e suggerimenti per il backup nell'interfaccia di amministrazione di Windows](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nel backup:

- Visualizzare una panoramica dello stato di backup di Azure
- Configurare gli elementi e la pianificazione di backup
- Avviare o arrestare un processo di backup
- Visualizzare lo stato e la cronologia dei processi di backup
- Visualizzare i punti di ripristino e ripristinare i dati
- Elimina dati di backup

## <a name="certificates"></a>Certificati

I **certificati** consentono di gestire gli archivi certificati in un computer o in un server.

### <a name="features"></a>Funzionalità

Nei certificati sono supportate le funzionalità seguenti:

- Esplorare e cercare i certificati esistenti
- Visualizza i dettagli del certificato
- Esporta certificati
- Rinnova certificati
- Richiedi nuovi certificati
- Elimina certificati

[**Visualizza commenti e suggerimenti e funzionalità proposte per i certificati**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Contenitori

**Contenitori** consente di visualizzare i contenitori in un host contenitore di Windows Server. Nel caso di un contenitore Windows Server Core in esecuzione, è possibile visualizzare i registri eventi e accedere all'interfaccia della riga di comando del contenitore.

[**Visualizza commenti e suggerimenti e funzionalità proposte per i contenitori**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Dispositivi

I **dispositivi** consentono di gestire i dispositivi connessi in un computer o un server.

### <a name="features"></a>Funzionalità

Nei dispositivi sono supportate le funzionalità seguenti:

- Esplorare e cercare i dispositivi
- Visualizza i dettagli del dispositivo
- Disabilitare un dispositivo
- Aggiornare il driver in un dispositivo

[**Visualizza commenti e suggerimenti e funzionalità proposte per i dispositivi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="dhcp"></a>DHCP

**DHCP** consente di gestire i dispositivi connessi in un computer o un server.

### <a name="features"></a>Funzionalità

- Creazione/configurazione/visualizzazione degli ambiti IPV4 e IPV6
- Creare esclusioni di indirizzi e configurare l'indirizzo IP iniziale e finale
- Creare prenotazioni di indirizzi e configurare l'indirizzo MAC client (IPV4), DUID e IAID (IPV6)

[**Visualizza commenti e suggerimenti e funzionalità proposte per DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## <a name="dns"></a>DNS

**DNS** consente di gestire i dispositivi connessi in un computer o un server.

### <a name="features"></a>Funzionalità

- Visualizzare i dettagli delle zone di ricerca diretta DNS, delle zone di ricerca inversa e dei record DNS
- Creare zone di ricerca diretta (primaria, secondaria o stub) e configurare le proprietà della zona di ricerca diretta
- Crea host (A o AAAA), tipo CNAME o MX di record DNS
- Configurare le proprietà di record DNS
- Creare zone di ricerca inversa IPV4 e IPV6 (primario, secondario e Stub), configurare le proprietà della zona di ricerca inversa
- Crea il tipo PTR, CNAME di record DNS nella zona di ricerca inversa.

[**Visualizza commenti e suggerimenti e funzionalità proposte per DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## <a name="events"></a>Events

**Gli eventi** consentono di gestire i registri eventi in un computer o un server.

### <a name="features"></a>Funzionalità

Negli eventi sono supportate le funzionalità seguenti:

- Esplorare e cercare gli eventi
- Visualizza dettagli evento
- Cancella gli eventi dal log
- Esporta eventi dal log

[**Visualizza commenti e suggerimenti e funzionalità proposte per gli eventi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>File

**File** consente di gestire file e cartelle in un computer o un server.

### <a name="features"></a>Funzionalità

Nei file sono supportate le funzionalità seguenti:

- Sfogliare file e cartelle
- Ricerca di un file o di una cartella
- Crea una nuova cartella
- Eliminare un file o una cartella
- Scaricare un file o una cartella
- Caricare un file o una cartella
- Rinominare un file o una cartella
- Estrarre un file zip
- Visualizza proprietà file o cartella
- Aggiungere, modificare o rimuovere condivisioni file
- Modificare le autorizzazioni di utenti e gruppi per le condivisioni file

[**Visualizza commenti e suggerimenti e funzionalità proposte per i file**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Firewall

Il **Firewall** consente di gestire le impostazioni e le regole del firewall in un computer o un server.

### <a name="features"></a>Funzionalità

Nel firewall sono supportate le funzionalità seguenti:

- Visualizzazione di una panoramica delle impostazioni del firewall
- Visualizzare le regole del firewall in ingresso
- Visualizzare le regole del firewall in uscita
- Cerca regole firewall
- Visualizza i dettagli della regola del firewall
- Creare una nuova regola del firewall
- Abilitare o disabilitare una regola del firewall
- Eliminare una regola del firewall
- Modificare le proprietà di una regola del firewall

[**Visualizza commenti e suggerimenti e funzionalità proposte per il firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>App installate

**App installate** consente di elencare e disinstallare le applicazioni installate.

[**Visualizza commenti e suggerimenti e funzionalità proposte per le app installate**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Utenti e gruppi locali

**Utenti e gruppi locali** consentono di gestire i gruppi di sicurezza e gli utenti presenti localmente in un computer o un server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in utenti e gruppi locali:

- Visualizza e Cerca utenti e gruppi
- Creare un nuovo utente o gruppo
- Gestire l'appartenenza a un gruppo di un utente
- Eliminare un utente o un gruppo
- Modificare la password di un utente
- Modificare le proprietà di un utente o di un gruppo

[**Visualizza commenti e suggerimenti e funzionalità proposte per utenti e gruppi locali**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Rete

**Rete** consente di gestire i dispositivi e le impostazioni di rete in un computer o un server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in rete:

- Sfoglia e cerca le schede di rete esistenti
- Visualizzare i dettagli di una scheda di rete
- Modificare le proprietà di una scheda di rete
- Creare una [scheda di rete di Azure (funzionalità di anteprima)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Visualizza commenti e suggerimenti e funzionalità proposte per la rete**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell** consente di interagire con un computer o un server tramite una sessione di PowerShell.

### <a name="features"></a>Funzionalità

In PowerShell sono supportate le funzionalità seguenti:

- Creare una sessione di PowerShell interattiva sul server
- Disconnetti dalla sessione di PowerShell nel server

[**Visualizza commenti e suggerimenti e funzionalità proposte per PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Processi

**Processi** consente di gestire i processi in esecuzione in un computer o un server.

### <a name="features"></a>Funzionalità

Nei processi sono supportate le funzionalità seguenti:

- Sfoglia e Cerca processi in esecuzione
- Visualizza dettagli processo
- Avviare un processo
- Terminare un processo
- Creare un dump del processo
- Trova handle di processo

[**Visualizza commenti e suggerimenti e funzionalità proposte per i processi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registro di sistema

Il **Registro di sistema** consente di gestire le chiavi e i valori del registro di sistema in un computer o un server.

### <a name="features"></a>Funzionalità

Nel registro di sistema sono supportate le funzionalità seguenti:

- Sfoglia chiavi e valori del registro di sistema
- Aggiungere o modificare i valori del registro di sistema
- Elimina valori del registro di sistema

[**Visualizza commenti e suggerimenti e funzionalità proposte per il registro di sistema**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Desktop remoto

**Desktop remoto** consente di interagire con un computer o un server tramite una sessione desktop interattiva.

### <a name="features"></a>Funzionalità

In Desktop remoto sono supportate le funzionalità seguenti:

- Avviare una sessione di desktop remoto interattiva
- Disconnettersi da una sessione Desktop remoto
- Inviare CTRL + ALT + CANC a una sessione Desktop remoto

[**Visualizza commenti e suggerimenti e funzionalità proposte per desktop remoto**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Ruoli e funzionalità

**Ruoli e funzionalità** consente di gestire ruoli e funzionalità in un server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in ruoli e funzionalità:

- Sfogliare l'elenco dei ruoli e delle funzionalità in un server
- Visualizza dettagli ruolo o funzionalità
- Installare un ruolo o una funzionalità
- Rimuovere un ruolo o una funzionalità

[**Visualizza commenti e suggerimenti e funzionalità proposte per ruoli e funzionalità**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>ATTIVITÀ PIANIFICATE

Le **attività pianificate** consentono di gestire le attività pianificate in un computer o un server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nelle attività pianificate:

- Sfogliare la libreria dell'utilità di pianificazione
- Modificare le attività pianificate
- Abilita & Disabilita le attività pianificate
- Avviare & arrestare le attività pianificate
- Creazione di attività pianificate

[**Visualizza commenti e suggerimenti e funzionalità proposte per le attività pianificate**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Servizi

I **Servizi** consentono di gestire i servizi in un computer o in un server.

### <a name="features"></a>Funzionalità

Nei servizi sono supportate le funzionalità seguenti:

- Esplorare e cercare i servizi in un server
- Visualizzare i dettagli di un servizio
- Avviare un servizio
- Sospendere un servizio
- Modificare le proprietà di un servizio

[**Visualizza commenti e suggerimenti e funzionalità proposte per i servizi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="settings"></a>Impostazioni

**Settings** è una posizione centralizzata per la gestione delle impostazioni in un computer o in un server.

### <a name="features"></a>Funzionalità

- Visualizza e modifica le variabili di ambiente di sistema e utente
- Visualizzare la configurazione per il monitoraggio degli avvisi da [monitoraggio di Azure](azure-monitor.md)
- Visualizzare e modificare la configurazione di risparmio energia
- Visualizzare e modificare le impostazioni di Desktop remoto
- Visualizzare e modificare le impostazioni di controllo degli accessi in base al ruolo
- Visualizzare e modificare le impostazioni dell'host Hyper-V, se applicabile

## <a name="storage"></a>Archiviazione

**Archiviazione** consente di gestire i dispositivi di archiviazione in un computer o un server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nell'archiviazione:

- Esplorare e cercare dischi esistenti in un server
- Visualizza i dettagli del disco
- Creare un volume
- Inizializzare un disco
- Creazione, collegamento e scollegamento di un disco rigido virtuale (VHD)
- Portare un disco offline
- Formattare un volume
- Ridimensionare un volume
- Modificare le proprietà del volume
- Eliminare un volume
- Installare la gestione delle quote
- Gestire il file server Gestione risorse quote [di archiviazione-> la quota di creazione/aggiornamento](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)

[**Visualizza commenti e suggerimenti e funzionalità proposte per l'archiviazione**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Servizio di migrazione della risorsa di archiviazione

Il **servizio migrazione archiviazione** consente di eseguire la migrazione dei server e delle condivisioni file in Azure o in Windows Server 2019, senza richiedere alle app o agli utenti di modificare qualsiasi elemento.
[Ottenere una panoramica del servizio migrazione archiviazione](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Il servizio migrazione archiviazione richiede Windows Server 2019.

## <a name="storage-replica"></a>Replica archiviazione

Usare **replica archiviazione** per gestire la replica di archiviazione da server a server.
[Altre informazioni su replica archiviazione](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Informazioni dettagliate di sistema

**System Insights** introduce l'analisi predittiva in modo nativo in Windows Server per contribuire a migliorare le informazioni sul funzionamento del server.
[Ottenere una panoramica di System Insights](http://aka.ms/systeminsights)

>[!NOTE]
>System Insights richiede Windows Server 2019.

## <a name="updates"></a>Aggiornamenti

**Aggiornamenti** consente di gestire gli aggiornamenti di Microsoft e/o Windows in un computer o un server.

### <a name="features"></a>Funzionalità

Negli aggiornamenti sono supportate le funzionalità seguenti:

- Visualizzare gli aggiornamenti di Windows o Microsoft disponibili
- Visualizza un elenco di cronologia aggiornamenti
- Installare gli aggiornamenti
- Verificare la disponibilità di aggiornamenti online da Microsoft Update
- Gestire l'integrazione di [Azure Gestione aggiornamenti](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Visualizza commenti e suggerimenti e funzionalità proposte per gli aggiornamenti**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Macchine virtuali

Vedere [gestione delle macchine virtuali con l'interfaccia di amministrazione di Windows](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Commutatori virtuali

**Commutatori virtuali** consente di gestire i commutatori virtuali Hyper-V in un computer o un server.

### <a name="features"></a>Funzionalità

In commutatori virtuali sono supportate le funzionalità seguenti:

- Esplorare e cercare i commutatori virtuali in un server
- Crea un nuovo commutire virtuale
- Rinominare un commutire virtuale
- Elimina un commutire virtuale esistente
- Modificare le proprietà di un Commuter virtuale

[**Visualizza commenti e suggerimenti e funzionalità proposte per i commutatori virtuali**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
