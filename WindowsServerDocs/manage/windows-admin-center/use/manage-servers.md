---
title: Gestire i server con Windows Admin Center
description: Gestire i server con Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 04ade4a14272c7840b5036ca6ad5a3bf3d09bcf9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891152"
---
# <a name="manage-servers-with-windows-admin-center"></a>Gestire i server con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

> [!Tip]
> Novità di Windows Admin Center
> [Ulteriori informazioni su Windows Admin Center](../understand/windows-admin-center.md) o [Scarica ora](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>Gestione delle macchine Windows Server

È possibile aggiungere singoli server che esegue Windows Server 2012 o versione successiva a Windows Admin Center per gestire il server con un set completo di strumenti inclusi i certificati, i dispositivi, gli eventi, processi, ruoli e funzionalità, gli aggiornamenti, le macchine virtuali e altro ancora.

![Schermata di panoramica di connessione server](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>Aggiunta di un server a Windows Admin Center

Per aggiungere un server a Windows Admin Center:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere un **connessione al Server**.
3. Digitare il nome del server e, se richiesto, le credenziali da utilizzare.
4. Fare clic su **Submit** alla fine.

Il server verrà aggiunto all'elenco di connessione nella pagina di panoramica. Fare clic per connettersi al server.

> [!NOTE]
> È anche possibile aggiungere [i cluster di failover](manage-failover-clusters.md) oppure [cluster iperconvergente](manage-hyper-converged.md) come una connessione separata in Windows Admin Center.

## <a name="tools"></a>Strumenti

Gli strumenti seguenti sono disponibili per le connessioni al server:

| Strumento | Descrizione |
| ---- | ----------- |
| [Panoramica](#overview) | Visualizzare i dettagli del server e controllare lo stato del server |
| [Backup](#backup) | Visualizzare e configurare il Backup di Azure |  
| [Certificati](#certificates) | Visualizzare e modificare i certificati |
| [Contenitori](#containers) | Visualizza contenitori |
| [Dispositivi](#devices) | Visualizzare e modificare i dispositivi |
| [Eventi](#events) | Visualizza eventi |
| [File](#files) | Esplorare i file e cartelle |
| [Firewall](#firewall) | Visualizzare e modificare le regole del firewall |
| [App installate](#installed-apps) | Visualizzare e rimuovere le app installate |
| [Utenti e gruppi locali](#local-users-and-groups) | Visualizzare e modificare gruppi e utenti locali |
| [Rete](#network) | Visualizzare e modificare i dispositivi di rete |
| [PowerShell](#powershell) | Interagire con il server tramite PowerShell |
| [Processi](#processes) | Visualizzare e modificare i processi in esecuzione |
| [Registry](#registry) | Visualizzare e modificare le voci del Registro di sistema |
| [Desktop remoto](#remote-desktop) | Interagire con il server tramite Desktop remoto |
| [Ruoli e funzionalità](#roles-and-features) | Visualizzare e modificare i ruoli e funzionalità |
| [Attività pianificate](#scheduled-tasks) | Visualizzare e modificare le attività pianificate |
| [Servizi](#services) | Visualizzare e modificare i servizi |
| [Archiviazione](#storage) | Visualizzare e modificare i dispositivi di archiviazione |
| [Servizio di migrazione di archiviazione](#storage-migration-service) | Eseguire la migrazione di server e condivisioni file in Azure o in Windows Server 2019 |
| [Replica di archiviazione](#storage-replica) | Usare la Replica di archiviazione per gestire la replica di archiviazione da server a server |
| [Informazioni di sistema](#system-insights) | Sistema Insights offre un aumento approfondite sul funzionamento del server. |
| [aggiornamenti](#updates) | Visualizzazione installato e la ricerca di nuovi aggiornamenti |
| [Macchine virtuali](manage-virtual-machines.md) | Visualizzare e gestire le macchine virtuali |
| [Commutatori virtuali](#virtual-switches) | Visualizzare e gestire i commutatori virtuali |

## <a name="overview"></a>Panoramica

**Panoramica** consente di visualizzare lo stato corrente di CPU, memoria e prestazioni di rete, nonché come eseguire operazioni e modificare le impostazioni in un server o il computer di destinazione.

### <a name="features"></a>Funzionalità

In panoramica di Server Manager sono supportate le funzionalità seguenti:

- Visualizza i dettagli del server
- Attività vista CPU
- Visualizza le attività della memoria
- Visualizzazione attività di rete
- Riavvia server
- Chiusura del server
- Abilitare le metriche del disco nel server
- Modificare l'ID Computer nel server

[**Visualizzare feedback e alcune funzionalità proposte per Server Overview**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="backup"></a>Backup

**Backup** consente di proteggere il server di Windows da danneggiamenti, attacchi o emergenze eseguendo il backup del server direttamente in Microsoft Azure.
[Altre informazioni sui Backup di Azure.](https://aka.ms/windows-admin-center-backup)

[Fornire commenti e suggerimenti per il backup in Windows Admin Center](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Funzionalità

Nel Backup sono supportate le funzionalità seguenti:

- Visualizzare una panoramica del tuo stato di backup di Azure
- Configura pianificazione e gli elementi di backup
- Avviare o arrestare un processo di backup
- Visualizzare lo stato e la cronologia processo di backup
- Visualizzare i punti di ripristino e ripristinare i dati
- Elimina dati di backup

## <a name="certificates"></a>Certificati

**I certificati** consente di gestire gli archivi di certificati in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nei certificati:

- Esplorare e cercare i certificati esistenti
- Visualizzare i dettagli di certificato
- Esportare i certificati
- Rinnovare i certificati
- Richiedere nuovi certificati
- Eliminare i certificati

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per i certificati**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Contenitori

**I contenitori** consente di visualizzare i contenitori in un host contenitore di Windows Server. Nel caso di un contenitore in esecuzione Windows Server Core, è possibile visualizzare i registri eventi e l'interfaccia della riga di comando del contenitore di accesso.

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per contenitori**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Dispositivi

**Dispositivi** consente di gestire i dispositivi connessi in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nei dispositivi:

- Esplorazione e ricerca di dispositivi
- Visualizzare i dettagli dispositivo
- Disabilitare un dispositivo
- Aggiornare i driver in un dispositivo

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per i dispositivi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="events"></a>Eventi

**Gli eventi** consente di gestire i registri eventi in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate negli eventi:

- Esplorazione e ricerca gli eventi
- Visualizzare i dettagli evento
- Cancellato eventi dal log
- Esportare gli eventi dal log

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per gli eventi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>File

**File** consente di gestire file e cartelle in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in file:

- Esplorare i file e cartelle
- Eseguire la ricerca di un file o cartella
- Creare una nuova cartella
- Eliminare un file o cartella
- Scaricare un file o cartella
- Caricare un file o cartella
- Rinominare un file o cartella
- Estrarre un file zip
- Visualizzare le proprietà di file o cartella
- Gestire gestione risorse File Server [quote](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)
- Aggiungere, modificare o rimuovere condivisioni di file
- Modificare autorizzazioni utenti e gruppi nelle condivisioni file

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per i file**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Firewall

**Firewall** consente di gestire le impostazioni del firewall e regole in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nel Firewall:

- Visualizzare una panoramica delle impostazioni del firewall
- Visualizzare le regole del firewall in ingresso
- Visualizzare le regole del firewall in uscita
- Cerca regole di firewall
- Visualizza i dettagli della regola firewall
- Creare una nuova regola del firewall
- Abilitare o disabilitare una regola del firewall
- Eliminare una regola del firewall
- Modificare le proprietà di una regola del firewall

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per il Firewall**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>App installate

**Le app installate** consente di elencare e disinstallare applicazioni installate.

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per le app installate**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Utenti e gruppi locali

**Gli utenti e gruppi locali** consente di gestire i gruppi di sicurezza e gli utenti che esistono in locale in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in utenti e gruppi locali:

- Visualizzare e cercare utenti e gruppi
- Creare un nuovo utente o gruppo
- Gestire l'appartenenza al gruppo dell'utente
- Eliminare un gruppo o utente
- Modificare la password dell'utente
- Modificare le proprietà di un utente o gruppo

[**Visualizzare il feedback e alcune funzionalità proposte per utenti e gruppi locali**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Rete

**Rete** consente di gestire i dispositivi di rete e le impostazioni in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nella rete:

- Esplorare e trovare le schede di rete esistente
- Visualizzare i dettagli di una scheda di rete
- Modificare le proprietà di una scheda di rete
- Creare un [scheda di rete di Azure (funzionalità di anteprima)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Visualizzare il feedback e alcune funzionalità proposte per la rete**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell** consente di interagire con un computer o un server tramite una sessione di PowerShell.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in PowerShell:

- Creare una sessione di PowerShell interattiva sul server
- Disconnettersi dalla sessione di PowerShell nel server

[**Visualizzare il feedback e alcune funzionalità proposte per PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Processi

**Processi** consente di gestire i processi in esecuzione in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nei processi:

- Esplorare e cercare processi in esecuzione
- Visualizza dettagli processo
- Avviare un processo
- Terminare un processo
- Creare un dump del processo
- Trovare gli handle di processo

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per i processi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registro di sistema

**Registro di sistema** consente di gestire le chiavi del Registro di sistema e i valori in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nel Registro di sistema:

- Consente di esplorare i valori e chiavi del Registro di sistema
- Aggiungere o modificare i valori del Registro di sistema
- Eliminazione dei valori del Registro di sistema

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per registro**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Desktop remoto

**Desktop remoto** consente di interagire con un computer o un server tramite una sessione desktop interattiva.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in Desktop remoto:

- Avviare una sessione interattiva di desktop remota
- Disconnettersi da una sessione desktop remoto
- Inviare Ctrl + Alt + Canc a una sessione desktop remoto

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per il Desktop remoto**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Ruoli e funzionalità

**Ruoli e funzionalità** consente di gestire ruoli e funzionalità in un server.

### <a name="features"></a>Funzionalità

Ruoli e funzionalità sono supportate le funzionalità seguenti:

- Esplorare l'elenco di ruoli e funzionalità in un server
- Visualizzare i dettagli di ruolo o funzionalità
- Installare un ruolo o funzionalità
- Rimuovere un ruolo o funzionalità

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per i ruoli e funzionalità**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>ATTIVITÀ PIANIFICATE

**Attività pianificate** consente di gestire le attività pianificate in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate in operazioni pianificate:

- Esplorare la raccolta dell'utilità di pianificazione di attività
- Modificare le attività pianificate
- Abilitare e disabilitare le attività pianificate
- Avviare e arrestare le attività pianificate
- Creare le attività pianificate

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per attività pianificate**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Servizi

**Servizi** consente di gestire i servizi in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nei servizi:

- Esplorare e cercare i servizi in un server
- Visualizzare i dettagli di un servizio
- Avviare un servizio
- Sospendere un servizio
- Modificare le proprietà di un servizio

[**Visualizzare i commenti e suggerimenti e alcune funzionalità proposte per i servizi**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="storage"></a>Archiviazione

**Archiviazione** consente di gestire i dispositivi di archiviazione in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nello spazio di memorizzazione:

- Esplorare e trovare dischi esistenti in un server
- Visualizzare i dettagli del disco
- Creare un volume
- Inizializzare un disco
- Creare, collegare e scollegare un disco rigido virtuale (VHD)
- Disconnettere un disco
- Formattare un volume
- Ridimensiona un volume
- Modifica proprietà volume
- Eliminare un volume
- Installare Gestione delle quote

[**Visualizzare il feedback e alcune funzionalità proposte per l'archiviazione**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Servizio di migrazione della risorsa di archiviazione

**Il servizio di migrazione archiviazione** consente di eseguire la migrazione di server e file condivisi in Azure o in Windows Server 2019, ovvero senza richiedere agli utenti di modificare qualsiasi elemento o App.
[Ottenere una panoramica della migrazione del servizio di archiviazione](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Il servizio di migrazione di archiviazione richiede Windows Server 2019.

## <a name="storage-replica"></a>Replica archiviazione

Uso **Replica di archiviazione** per gestire la replica di archiviazione da server a server.
[Altre informazioni su Replica archiviazione](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Informazioni dettagliate di sistema

**Sistema Insights** introduce analitica predittiva in modo nativo in Windows Server per fornire è aumentato approfondite sul funzionamento del server.
[Ottenere una panoramica del sistema di Insights](http://aka.ms/systeminsights)

>[!NOTE]
>Sistema Insights richiede Windows Server 2019.

## <a name="updates"></a>Aggiornamenti

**Gli aggiornamenti** consente di gestire Microsoft e/o gli aggiornamenti di Windows in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate negli aggiornamenti:

- Visualizzare disponibile Windows o Microsoft Updates
- Visualizzare un elenco di cronologia degli aggiornamenti
- Installare gli aggiornamenti
- Controlla online per gli aggiornamenti da Microsoft Update
- Gestire [gestione degli aggiornamenti di Azure](https://docs.microsoft.com/azure/automation/automation-update-management) integrazione

[**Visualizzare il feedback e alcune funzionalità proposte per gli aggiornamenti**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Macchine virtuali

Vedere [gestione delle macchine virtuali con Windows Admin Center](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Commutatori virtuali

**Commutatori virtuali** consente di gestire i commutatori virtuali Hyper-V in un computer o server.

### <a name="features"></a>Funzionalità

Le funzionalità seguenti sono supportate nei commutatori virtuali:

- Esplorare e trovare commutatori virtuali in un server
- Creare un nuovo commutatore virtuale
- Rinomina un commutatore virtuale
- Elimina un commutatore virtuale esistente
- Modificare le proprietà di un commutatore virtuale

[**Visualizzare il feedback e alcune funzionalità proposte per i commutatori virtuali**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
