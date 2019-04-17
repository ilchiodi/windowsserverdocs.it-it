---
title: Gestire i server con Windows Admin Center
description: Gestire i server con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93a40345d05a6230596832b2d455d36eee2401b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296683"
---
# Gestire i server con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## La gestione di Windows Server

Puoi aggiungere singoli server che eseguono Windows Server 2012 o in un secondo momento per Windows Admin Center per gestire il server con un set completo di strumenti inclusi i certificati, i dispositivi, gli eventi, processi, ruoli e funzionalità, aggiornamenti, le macchine virtuali e altro ancora.

![Schermata di panoramica connessione server](../media/manage-servers/server-overview.png)

## Aggiunta di un server a Windows Admin Center

Per aggiungere un server a Windows Admin Center:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **Connessione al Server**.
3. Digita il nome del server e, se richiesto, le credenziali da utilizzare.
4. Fai clic su **Invia** alla fine.

Il server verrà aggiunto all'elenco di connessione nella pagina della panoramica. Fai clic per connettersi al server.

> [!NOTE]
> È anche possibile aggiungere [i cluster di failover](manage-failover-clusters.md) o [cluster iper-convergenti](manage-hyper-converged.md) come una connessione separata in Windows Admin Center.

## Strumenti

Gli strumenti seguenti sono disponibili per le connessioni server:

| Strumento | Descrizione |
| ---- | ----------- |
| [Panoramica](#overview) | Visualizzare i dettagli di server e controllare lo stato del server |
| [Active Directory](#active-directory-preview) | Gestione di Active Directory |
| [Backup](#backup) | Visualizzare e configurare il Backup di Azure |  
| [Certificati](#certificates) | Visualizzare e modificare i certificati |
| [Contenitori](#containers) | Contenitori di visualizzazione |
| [Dispositivi](#devices) | Visualizzare e modificare i dispositivi |
| [DHCP](#dhcp) | Visualizzazione e Gestione configurazione del server DHCP |
| [DNS](#dns) | Visualizzare e gestire la configurazione di server DNS |
| [Eventi](#events) | Eventi di visualizzazione |
| [File](#files) | Sfoglia i file e cartelle |
| [Firewall](#firewall) | Visualizzare e modificare le regole del firewall |
| [App installate](#installed-apps) | Visualizzare e rimuovere le app installate |
| [Utenti e gruppi locali](#local-users-and-groups) | Visualizzare e modificare utenti e gruppi locali |
| [Rete](#network) | Visualizzare e modificare i dispositivi di rete |
| [PowerShell](#powershell) | Interagire con server tramite PowerShell |
| [Processi](#processes) | Visualizzare e modificare i processi in esecuzione |
| [Registro di sistema](#registry) | Visualizzare e modificare le voci del Registro di sistema |
| [Desktop remoto](#remote-desktop) | Interagire con server tramite Desktop remoto |
| [Ruoli e funzionalità](#roles-and-features) | Visualizzare e modificare i ruoli e funzionalità |
| [Attività pianificate](#scheduled-tasks) | Visualizzare e modificare le attività pianificate |
| [Servizi](#services) | Visualizzare e modificare i servizi |
| [Impostazioni](#settings) | Visualizzare e modificare i servizi |
| [Archiviazione](#storage) | Visualizzare e modificare i dispositivi di archiviazione |
| [Servizio di migrazione della risorsa di archiviazione](#storage-migration-service) | Eseguire la migrazione di server e le condivisioni file per Azure o Windows Server 2019 |
| [Replica di archiviazione](#storage-replica) | Usare Replica di archiviazione per gestire la replica di archiviazione da server a server |
| [Informazioni dettagliate di sistema](#system-insights) | Fornisce informazioni dettagliate di sistema è aumentato approfondimenti il funzionamento del server. |
| [Aggiornamenti](#updates) | Visualizzazione installato e verificare la disponibilità di nuovi aggiornamenti |
| [Macchine virtuali](manage-virtual-machines.md) | Visualizzare e gestire le macchine virtuali |
| [Switch virtuali](#virtual-switches) | Visualizzazione e gestione commutatori virtuali |

## Panoramica

**Panoramica** consente di visualizzare lo stato corrente della CPU, memoria e prestazioni di rete, nonché eseguire le operazioni e modificare le impostazioni in un computer di destinazione o un server.

### Funzionalità

Le funzionalità seguenti sono supportate in Server Manager Panoramica:

- Visualizzazione dei dettagli di server
- Attività di visualizzazione della CPU
- Visualizzazione della memoria attività
- Visualizzazione attività di rete
- Riavviare il server
- Chiusura del server
- Abilitare le metriche del disco nel server
- Modificare l'ID del Computer nel server
- Visualizzare l'indirizzo IP BMC con collegamento ipertestuale (richiede compatibile con IPMI BMC).

[**Feedback di visualizzazione e le funzionalità proposta per la panoramica sul Server**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## Active Directory (anteprima)

**Active Directory** è un'anteprima anticipata che è disponibile nel [feed di estensione](../configure/using-extensions.md).

### Funzionalità

La gestione di Active Directory seguente sono disponibili:

- Creare utente
- Crea gruppo
- Ricerca per gli utenti, computer e gruppi
- Riquadro dei dettagli per gli utenti, computer e gruppi quando selezionata nella griglia
- Gli utenti di azioni globali griglia, computer e gruppi (Attiva/disattiva, rimuovere)
- Reimpostare la password utente
- Gli oggetti utente: configurare l'appartenenza al gruppo di proprietà di base &
- Gli oggetti computer: configurare la delega a un singolo computer
- Gli oggetti di gruppo: gestire l'appartenenza (1-aggiungere/rimuovere l'utente alla volta)  

[**Visualizzazione feedback e funzionalità proposta per Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## Backup

**Backup** consente di proteggere Windows server da danneggiamenti, attacchi o catastrofi backup del server direttamente a Microsoft Azure.
[Altre informazioni su Azure Backup.](https://aka.ms/windows-admin-center-backup)

[Fornire feedback per il backup in Windows Admin Center](https://aka.ms/backup-wac-feedback)

### Funzionalità

Le funzionalità seguenti sono supportate in Backup:

- Visualizza una panoramica di stato di backup di Azure
- Configurare la pianificazione e gli elementi di backup
- Avviare o interrompere un processo di backup
- Visualizzare la cronologia di processo di backup e stato
- Visualizzare punti di ripristino e ripristinare i dati
- Eliminare i dati di backup

## Certificati

**I certificati** consente di gestire archivi di certificati in un computer o server.

### Funzionalità

Le funzionalità seguenti sono supportate nei certificati:

- Sfoglia e cerca i certificati esistenti
- Visualizzazione dei dettagli di certificato
- Esportare i certificati
- Il rinnovo dei certificati
- Nuova richiesta di certificati
- Eliminare i certificati

[**Feedback di visualizzazione e le funzionalità proposta per i certificati**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## Contenitori

**I contenitori** consente di visualizzare i contenitori in un host contenitore di Windows Server. Nel caso di un contenitore in esecuzione Windows Server Core, è possibile visualizzare i registri eventi e accedere l'interfaccia CLI del contenitore.

[**Feedback di visualizzazione e le funzionalità proposta per i contenitori**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## Dispositivi

**I dispositivi** consente di gestire i dispositivi connessi in un computer o server.

### Funzionalità

Le funzionalità seguenti sono supportate nei dispositivi:

- Dispositivi di ricerca
- Visualizzazione dei dettagli di dispositivo
- Disabilitare un dispositivo
- Aggiorna driver in un dispositivo

[**Feedback di visualizzazione e le funzionalità proposta per i dispositivi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## DHCP

**DHCP** consente di gestire i dispositivi connessi in un computer o server.

### Funzionalità

- Ambiti di creare/configurare/visualizzazione IPV4 e IPV6
- Creare le esclusioni di indirizzo e configurare l'indirizzo IP di inizio e fine
- Creare le prenotazioni di indirizzo e configurare il client indirizzo MAC (IPV4), DUID e definito (IPV6)

[**Feedback di visualizzazione e le funzionalità proposta per DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## DNS

**DNS** consente di gestire i dispositivi connessi in un computer o server.

### Funzionalità

- Visualizzare i dettagli delle zone di ricerca diretta DNS, zone di ricerca inversa e record DNS
- Creare zone di ricerca diretta (primaria, secondaria o stub) e configurare le proprietà zona di ricerca diretta
- Creare Host (A o AAAA), CNAME o MX il tipo di record DNS
- Configurare le proprietà di record DNS
- Creare zone IPV4 e IPV6 ricerca inversa (primari, secondari e lo stub), configurare le proprietà zona di ricerca inversa
- Creare PTR, tipo di record CNAME di DNS registra in zona di ricerca inversa.

[**Feedback di visualizzazione e le funzionalità proposta per DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## Eventi

**Gli eventi** consente di gestire i registri eventi in un computer o server.

### Funzionalità

Negli eventi sono supportate le seguenti funzionalità:

- Eventi di ricerca
- Visualizzazione dei dettagli evento
- Cancella eventi dal Registro
- Esportare gli eventi dal Registro

[**Feedback di visualizzazione e le funzionalità proposta per gli eventi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## File

**I file** consente di gestire file e cartelle su un computer o un server.

### Funzionalità

Le funzionalità seguenti sono supportate nei file:

- Sfoglia i file e cartelle
- Ricerca di un file o una cartella
- Crea una nuova cartella
- Eliminare un file o cartella
- Scaricare un file o cartella
- Caricare un file o una cartella
- Rinominare un file o cartella
- Estrarre file zip
- Visualizzare le proprietà di file o una cartella
- Gestire [le quote](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management) di gestione risorse File Server
- Aggiungere, modificare o rimuovere le condivisioni di file
- Modificare le autorizzazioni di gruppo per le condivisioni di file e utente

[**Feedback di visualizzazione e le funzionalità proposta per i file**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## Firewall

**Firewall** consente di gestire le impostazioni del firewall e le regole in un computer o server.

### Funzionalità

Le funzionalità seguenti sono supportate in Firewall:

- Visualizza una panoramica delle impostazioni del firewall
- Visualizzare le regole del firewall in entrata
- Visualizzare le regole del firewall in uscita
- Regole del firewall ricerca
- Visualizzazione dei dettagli di regola firewall
- Creare una nuova regola firewall
- Abilitare o disabilitare una regola del firewall
- Eliminare una regola del firewall
- Modificare le proprietà di una regola del firewall

[**Funzionalità proposta per Firewall e feedback di visualizzazione**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## App installate

**Le app installate** consente di elencare e disinstallare applicazioni installate.

[**Feedback di visualizzazione e le funzionalità proposta per le app installate**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## Utenti e gruppi locali

**Utenti e gruppi locali** consente di gestire i gruppi di sicurezza e gli utenti esistenti in locale su un computer o un server.

### Funzionalità

Le funzionalità seguenti sono supportate in utenti e gruppi locali:

- Visualizzazione e la ricerca di utenti e gruppi
- Creare un nuovo utente o gruppo
- Gestire l'appartenenza al gruppo dell'utente
- Eliminare un utente o gruppo
- Modificare la password dell'utente
- Modificare le proprietà di un utente o gruppo

[**Visualizzare il feedback e le funzionalità proposta per utenti e gruppi locali**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## Rete

**Rete** consente di gestire le impostazioni in un computer o un server e i dispositivi di rete.

### Funzionalità

Le funzionalità seguenti sono supportate nella rete:

- Sfoglia e cerca le schede di rete esistente
- Visualizzazione dei dettagli di una scheda di rete
- Modificare le proprietà di una scheda di rete
- Creare una [Scheda di rete di Azure (funzionalità di anteprima)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Visualizzare il feedback e le funzionalità proposta per la rete**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## PowerShell

**PowerShell** consente di interagire con un computer o un server tramite una sessione di PowerShell.

### Funzionalità

Le funzionalità seguenti sono supportate in PowerShell:

- Creare una sessione interattiva di PowerShell nel server
- Scollegare dalla sessione di PowerShell nel server

[**Visualizzare il feedback e le funzionalità proposta per PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## Processi

**Processi** consente di gestire i processi in esecuzione su un computer o un server.

### Funzionalità

Le funzionalità seguenti sono supportate nei processi:

- Sfoglia e cerca i processi in esecuzione
- Visualizzazione dei dettagli di processo
- Avviare un processo
- Termina un processo
- Creare un dump di processo
- Trova gli handle di processo

[**Feedback di visualizzazione e le funzionalità proposta per i processi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## Registro di sistema

**Registro di sistema** consente di gestire le chiavi del Registro di sistema e i valori in un computer o server.

### Funzionalità

Le funzionalità seguenti sono supportate nel Registro di sistema:

- Sfoglia i valori e le chiavi del Registro di sistema
- Aggiungere o modificare i valori del Registro di sistema
- Eliminare i valori del Registro di sistema

[**Feedback di visualizzazione e le funzionalità proposta per registro di sistema**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## Desktop remoto

**Desktop remoto** consente di interagire con un computer o un server tramite una sessione interattiva desktop.

### Funzionalità

Le funzionalità seguenti sono supportate in Desktop remoto:

- Avviare una sessione interattiva di desktop remota
- Disconnettersi da una sessione di desktop remota
- Inviare Ctrl + Alt + Canc in una sessione di desktop remoto

[**Feedback di visualizzazione e le funzionalità proposta per Desktop remoto**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## Ruoli e funzionalità

**Ruoli e funzionalità** consente di gestire i ruoli e funzionalità in un server.

### Funzionalità

Ruoli e funzionalità sono supportate le seguenti funzionalità:

- Esamina l'elenco di ruoli e funzionalità in un server
- Visualizzazione dei dettagli di ruolo o funzionalità
- Installare un ruolo o funzionalità
- Rimuovere un ruolo o funzionalità

[**Feedback di visualizzazione e le funzionalità proposta per ruoli e funzionalità**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## Attività pianificate

**Attività pianificate** consente di gestire le attività pianificate su un computer o un server.

### Funzionalità

Le funzionalità seguenti sono supportate nelle attività pianificata:

- Sfoglia la libreria di utilità di pianificazione
- Modificare le attività pianificate
- Abilitare le attività di disabilitare pianificato &
- Avviare le attività pianificata l'arresto &
- Crea attività pianificate

[**Funzionalità proposta per le attività pianificate e feedback di visualizzazione**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## Servizi

Consente di **servizi di** gestione dei servizi in un computer o server.

### Funzionalità

Le funzionalità seguenti sono supportate nei servizi di:

- Sfoglia e Cerca servizi in un server
- Visualizzazione dei dettagli di un servizio
- Avviare un servizio
- Sospendere un servizio
- Modificare le proprietà di un servizio

[**Feedback di visualizzazione e le funzionalità proposta per i servizi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## Impostazioni

**Le impostazioni** è una posizione centrale per gestire le impostazioni in un computer o server.

### Funzionalità

- Visualizzare e modificare le variabili di ambiente utente e di sistema
- Visualizzare la configurazione per il monitoraggio degli avvisi dal [Monitor di Azure](azure-monitor.md)
- Visualizzare e modificare la configurazione del risparmio energia
- Visualizzare e modificare le impostazioni di Desktop remoto
- Visualizzare e modificare le impostazioni di controllo di accesso basato sui ruoli
- Visualizzare e modificare le impostazioni di host Hyper-V, se applicabile

## Archiviazione

**Archiviazione** consente di gestire i dispositivi di archiviazione in un computer o server.

### Funzionalità

Lo spazio di archiviazione sono supportate le seguenti funzionalità:

- Sfoglia e cerca i dischi in un server esistenti
- Visualizzazione dei dettagli del disco
- Crea un volume
- Inizializzare un disco
- Creare, collegare e scollegare un disco rigido virtuale (VHD)
- Disconnettere un disco
- Formattare un volume
- Ridimensionare un volume
- Modificare le proprietà dei contratti multilicenza
- Eliminare un volume
- Installare Gestione quote

[**Visualizzare il feedback e le funzionalità proposta per l'archiviazione**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## Servizio di migrazione della risorsa di archiviazione

**Servizio di migrazione di archiviazione** consente di eseguire la migrazione di server e condivisioni di file per Azure o Windows Server 2019, ovvero senza la necessità di App o agli utenti di apportare alcuna modifica.
[Ottenere una panoramica del servizio di migrazione di archiviazione](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Servizio di migrazione di archiviazione richiede Windows Server 2019.

## Replica di archiviazione

Usare **Replica di archiviazione** per gestire la replica di archiviazione da server a server.
[Altre informazioni su Replica di archiviazione](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## Informazioni dettagliate di sistema

**Informazioni dettagliate di sistema** introduce analitica predittiva in modo nativo in Windows Server per offrirti maggiore approfondimenti il funzionamento del server.
[Ottenere una panoramica delle informazioni dettagliate di sistema](http://aka.ms/systeminsights)

>[!NOTE]
>Informazioni dettagliate di sistema richiede Windows Server 2019.

## Aggiornamenti

**Gli aggiornamenti** consente di gestire Microsoft e/o gli aggiornamenti di Windows in un computer o server.

### Funzionalità

Gli aggiornamenti sono supportate le seguenti funzionalità:

- Visualizzare disponibili Windows o Microsoft Updates
- Visualizzare un elenco di cronologia degli aggiornamenti
- Installare gli aggiornamenti
- Controlla online per gli aggiornamenti da Microsoft Update
- Gestire l'integrazione di [Gestione degli aggiornamenti di Azure](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Visualizzare il feedback e le funzionalità proposta per gli aggiornamenti**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## Macchine virtuali

Vedi [la gestione di macchine virtuali con Windows Admin Center](manage-virtual-machines.md)

## Switch virtuali

**Commutatori virtuali** consente di gestire commutatori virtuali Hyper-V in un computer o server.

### Funzionalità

Le funzionalità seguenti sono supportate in commutatori virtuali:

- Sfoglia e Cerca commutatori virtuali in un server
- Creare un nuovo commutatore virtuale
- Rinomina un commutatore virtuale
- Eliminare un commutatore virtuale esistente
- Modificare le proprietà di un commutatore virtuale

[**Visualizzare il feedback e le funzionalità proposta per commutatori virtuali**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
