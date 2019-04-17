---
title: Eseguire il backup dei server di Windows da Windows Admin Center con Azure Backup
description: Usa Windows Admin Center (Project Honolulu) per eseguire un Backup Windows Server con Azure Backup
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 3983d0b65bc69ef9fd40f3c8e196d40534b1b8f9
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296980"
---
# Eseguire il backup dei server di Windows da Windows Admin Center con Azure Backup

>Si applica a: Windows Admin Center Preview, Windows Admin Center

[Altre informazioni sull'integrazione di Azure con Windows Admin Center.](../plan/azure-integration-options.md)

Windows Admin Center semplifica il processo di backup i server Windows Azure e impedire eliminazione accidentale o dannosi, danneggiamento e persino ransomware. Per automatizzare l'installazione, puoi connettere il gateway di Windows Admin Center ad Azure.

Usa le informazioni seguenti per configurare il Backup è Windows Server e creare un criterio di Backup per eseguire il backup volumi del server e lo stato del sistema di Windows da Windows Admin Center.

## Che cos'è Azure Backup e come funziona con Windows Admin Center? 

**Backup di Azure** è il servizio basato su Azure, è possibile utilizzare per eseguire il backup (o proteggere) e il ripristino dei dati nel cloud Microsoft. Azure Backup sostituisce il locale esistente o una soluzione di backup fuori sede con una soluzione basata su cloud che è competitivo, sicuri e affidabili.
[Ulteriori informazioni su Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview).

Azure Backup offre più componenti che puoi scaricare e distribuire nel computer appropriato, server, o nel cloud. Il componente o agente, che distribuire dipende quello che vuoi proteggere. Tutti i componenti di Backup di Azure (indipendentemente dal fatto che stai proteggono i dati in locale o in Azure) possono essere usati per eseguire il backup dei dati a un insieme di credenziali di servizi di ripristino in Azure.

L'integrazione di Azure Backup in Windows Admin Center è ideale per backup di volumi e il sistema Windows stato Windows fisico o virtuale server locali. In questo modo per un meccanismo completo per il backup di File server, i controller di dominio e server Web IIS.

Windows Admin Center espone l'integrazione di Azure Backup tramite lo strumento nativo di **Backup** . Lo strumento di **Backup** fornisce di installazione, gestione e monitoraggio esperienze per avviare rapidamente il backup dei server, eseguire il backup comune e le operazioni di ripristino e monitor integrità backup complessiva dei server Windows.

## Prerequisiti e pianificazione

- Un Account di Azure con almeno una sottoscrizione
- La destinazione di Windows Server che si desidera backup deve avere accesso a Internet per Azure
- [Connetti il gateway Windows Admin Center di Azure](azure-integration.md)

Per avviare il flusso di lavoro per eseguire il backup di Windows Server, aprire una connessione al server, fai clic sullo strumento di **Backup** e segui i passaggi indicati di seguito.

## Configurare Azure Backup
Quando fai clic sullo strumento di **Backup** per una connessione al server in cui Azure Backup non è ancora abilitato, vedrai la schermata **iniziale per il Backup di Azure** . Fai clic sul pulsante **configurare Azure Backup** . Si aprirà il configurazione guidata del Backup di Azure. Segui i passaggi come indicato di seguito la procedura guidata per eseguire il backup del server.

Se è già configurato Azure Backup, facendo clic sullo strumento di **Backup** verrà aperto il **Dashboard di Backup**. Fai riferimento alla sezione ([Gestione e monitoraggio](#management-and-monitoring)) per individuare le operazioni e le attività che possono essere eseguite dal dashboard.

### Passaggio 1: Accesso a Microsoft Azure
Accedere al puoi Account Azure. 

> [!NOTE]
> Se si è connessi il gateway Windows Admin Center di Azure, è deve essere automaticamente l'accesso ad Azure. Puoi fare clic **disconnessione** a ulteriormente Accedi come altro utente.

### Passaggio 2: Configurare Azure Backup
Selezionare le impostazioni appropriate per il Backup di Azure, come descritto di seguito

 - **Id sottoscrizione:** La sottoscrizione di Azure che vuoi usare backup di Windows Server in Azure. Tutti gli asset di Azure, come il gruppo di risorse di Azure, verrà creato l'insieme di credenziali di servizi di ripristino nella sottoscrizione selezionata.
 - **Insieme di credenziali:** L'archivio di servizi di ripristino in cui verranno archiviati i backup dei server. È possibile selezionare dagli archivi esistenti o Windows Admin Center creerà un insieme di credenziali di nuovo.  
 - **Gruppo di risorse:** Il gruppo di risorse di Azure è un contenitore per una raccolta di risorse. L'insieme di credenziali di servizi di ripristino creato o contenuta nel gruppo di risorse specificato. È possibile selezionare da gruppi di risorse esistenti o Windows Admin Center verrà creata una nuova.
 - **Posizione:** L'area di Azure in cui verrà creato l'insieme di credenziali di servizi di ripristino. È consigliabile selezionare l'area di Azure più vicino a Windows Server.

### Passaggio 3: Selezionare gli elementi di Backup e pianificazione

- Selezionare si desidera eseguire il backup dal server. Windows Admin Center consente di selezionare una combinazione di **volumi** e lo **Stato del sistema Windows** , offrendo le dimensioni di dati selezionato stimata per il backup.

> [!NOTE]
> Il primo backup è un backup completo di tutti i dati selezionati. Tuttavia, i backup successivi sono incrementali in natura e trasferiscono solo le modifiche ai dati dopo il backup precedente.

- Seleziona più set di impostazioni **Delle pianificazioni dei Backup** per te lo stato del sistema e/o volumi.

### Passaggio 4: Immetti Passphrase di crittografia

- Immettere una **Crittografia Passphrase** di tua scelta (minimi 16 caratteri).  **Backup di Azure** protegge i dati di backup con una passphrase crittografia configurato dall'utente e gestita dall'utente. Per ripristinare i dati da Azure Backup è necessaria la passphrase di crittografia.

> [!NOTE]
> La passphrase deve essere archiviata in un luogo sicuro, ad esempio un altro server o l' [Insieme di credenziali di Azure chiave](https://docs.microsoft.com/azure/key-vault/quick-create-portal). Microsoft non archiviare la passphrase e non può recuperare o reimpostare la passphrase se viene perso o dimenticato.

- Esaminare tutte le impostazioni e fare clic su **Applica**

Windows Admin Center eseguirà le operazioni seguenti

1. Se non esiste già, creare un gruppo di risorse di Azure
2. Creare un insieme di credenziali servizi di ripristino di Azure come specificato
3. Installare e registrare l'agente di servizi di ripristino di Microsoft Azure nell'insieme di credenziali
4. Creare la pianificazione di Backup e la conservazione in base alle opzioni selezionate e associarli a Windows Server.

## Gestione e monitoraggio

Dopo aver creato correttamente il programma di installazione Azure Backup, si vedrà il **Backup Dashboard** quando si apre lo strumento di Backup per una connessione server esistente. È possibile eseguire le attività seguenti nel **Dashboard di Backup**

- **Accedere l'insieme di credenziali di Azure:** Puoi fare clic sul collegamento **Insieme di credenziali di servizi di ripristino** nella scheda **Panoramica** del **Dashboard di Backup** da adottare per l'insieme di credenziali di Azure per eseguire una [vasta gamma di operazioni di gestione](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **Eseguire un backup ad hoc:** Fai clic su **Backup ora** a eseguire un backup ad hoc. 
- **Monitorare i processi e configura le notifiche degli avvisi:** Passa alla scheda **processi** del dashboard per monitorare continuativa o oltre i processi e [configurare le notifiche di avviso](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) per ricevere messaggi di posta elettronica per qualsiasi non è riuscito processi o altri avvisi correlati a eseguire un backup.
- **Punti di ripristino di visualizzazione e recuperare dati:** Fare clic sulla scheda **Punti di ripristino** del dashboard per visualizzare i punti di ripristino e fai clic su **Recupero dati** per la procedura di ripristino di dati da Azure.