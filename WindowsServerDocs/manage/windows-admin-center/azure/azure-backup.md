---
title: Eseguire il backup dei server Windows dall'interfaccia di amministrazione di Windows con backup di Azure
description: Usare l'interfaccia di amministrazione di Windows (Project Honolulu) per eseguire il backup dei server Windows con backup di Azure
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server
ms.openlocfilehash: 77171e638fa1eb8c612bdc168868d8286ed23bb6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357398"
---
# <a name="backup-your-windows-servers-from-windows-admin-center-with-azure-backup"></a>Eseguire il backup dei server Windows dall'interfaccia di amministrazione di Windows con backup di Azure

>Si applica a: Windows Admin Center Preview, interfaccia di amministrazione di Windows

[Altre informazioni sull'integrazione di Azure con l'interfaccia di amministrazione di Windows.](../plan/azure-integration-options.md)

L'interfaccia di amministrazione di Windows semplifica il processo di backup dei server Windows in Azure e la protezione da eliminazioni accidentali o dannose, dal danneggiamento e persino dal ransomware. Per automatizzare l'installazione, puoi connettere il gateway di Windows Admin Center ad Azure.

Usare le informazioni seguenti per configurare il backup per Windows Server e creare un criterio di backup per eseguire il backup dei volumi del server e dello stato del sistema Windows dall'interfaccia di amministrazione di Windows.

## <a name="what-is-azure-backup-and-how-does-it-work-with-windows-admin-center"></a>Che cos'è backup di Azure e come funziona con l'interfaccia di amministrazione di Windows? 

**Backup di Azure** è il servizio basato su Azure che è possibile usare per eseguire il backup (o proteggere) e ripristinare i dati nel cloud Microsoft. Backup di Azure sostituisce la soluzione di backup locale o esterna esistente con una soluzione basata sul cloud affidabile, sicura e conveniente.
[Altre informazioni su backup di Azure](https://docs.microsoft.com/azure/backup/backup-overview).

Backup di Azure offre più componenti che è possibile scaricare e distribuire nel computer, nel server o nel cloud appropriato. Il componente, o Agent, distribuito dipende da ciò che si desidera proteggere. Tutti i componenti di backup di Azure, indipendentemente dal fatto che si proteggano i dati in locale o in Azure, possono essere usati per eseguire il backup dei dati in un insieme di credenziali di servizi di ripristino in Azure.

L'integrazione di backup di Azure nell'interfaccia di amministrazione di Windows è ideale per eseguire il backup dei volumi e dei server fisici o virtuali Windows locali dello stato del sistema Windows. Ciò rende possibile un meccanismo completo per il backup di file server, controller di dominio e server Web IIS.

L'interfaccia di amministrazione di Windows espone l'integrazione di backup di Azure tramite lo strumento di **backup** nativo. Lo strumento di **backup** offre esperienze di configurazione, gestione e monitoraggio per iniziare rapidamente a eseguire il backup dei server, eseguire operazioni comuni di backup e ripristino e monitorare l'integrità complessiva dei backup dei server Windows.

## <a name="prerequisites-and-planning"></a>Prerequisiti e pianificazione

- Un account Azure con almeno una sottoscrizione attiva
- I server Windows di destinazione di cui si vuole eseguire il backup devono avere accesso a Internet in Azure
- [Connettere il gateway dell'interfaccia di amministrazione di Windows ad Azure](azure-integration.md)

Per avviare il flusso di lavoro per il backup di Windows Server, aprire una connessione al server, fare clic sullo strumento di **backup** e seguire i passaggi indicati di seguito.

## <a name="setup-azure-backup"></a>Configurare backup di Azure
Quando si fa clic sullo strumento di **backup** per una connessione server in cui backup di Azure non è ancora abilitato, viene visualizzata la schermata **di backup di Azure** . Fare clic sul pulsante **Configura backup di Azure** . Verrà aperta l'installazione guidata di backup di Azure. Per eseguire il backup del server, seguire i passaggi elencati di seguito nella procedura guidata.

Se backup di Azure è già configurato, facendo clic sullo strumento di **backup** verrà aperto il **dashboard di backup**. Vedere la sezione ([gestione e monitoraggio](#management-and-monitoring)) per individuare le operazioni e le attività che possono essere eseguite dal dashboard.

### <a name="step-1-login-to-microsoft-azure"></a>Passaggio 1: Accedi a Microsoft Azure
Accedere all'account Azure. 

> [!NOTE]
> Se il gateway dell'interfaccia di amministrazione di Windows è stato connesso ad Azure, si dovrebbe accedere automaticamente ad Azure. È possibile fare clic su **Sign-out** per accedere ulteriormente come utente diverso.

### <a name="step-2-set-up-azure-backup"></a>Passaggio 2: Configurare backup di Azure
Selezionare le impostazioni appropriate per backup di Azure, come descritto di seguito

 - **ID sottoscrizione:** Sottoscrizione di Azure che si vuole usare per eseguire il backup di Windows Server in Azure. Tutte le risorse di Azure come il gruppo di risorse di Azure, l'insieme di credenziali di servizi di ripristino verrà creato nella sottoscrizione selezionata.
 - **Credenziali** Insieme di credenziali dei servizi di ripristino in cui verranno archiviati i backup dei server. È possibile selezionare un insieme di credenziali esistente o l'interfaccia di amministrazione di Windows creerà un nuovo insieme di credenziali.  
 - **Gruppo di risorse:** Il gruppo di risorse di Azure è un contenitore per una raccolta di risorse. L'insieme di credenziali di servizi di ripristino viene creato o contenuto nel gruppo di risorse specificato. È possibile scegliere tra gruppi di risorse esistenti o l'interfaccia di amministrazione di Windows ne creerà una nuova.
 - **Percorso:** Area di Azure in cui verrà creato l'insieme di credenziali di servizi di ripristino. Si consiglia di selezionare l'area di Azure più vicina a Windows Server.

### <a name="step-3-select-backup-items-and-schedule"></a>Passaggio 3: Selezione elementi di backup e pianificazione

- Selezionare gli elementi di cui si vuole eseguire il backup dal server. L'interfaccia di amministrazione di Windows consente di scegliere una combinazione di **volumi** e lo **stato del sistema di Windows** , fornendo al tempo stesso le dimensioni stimate dei dati selezionati per il backup.

> [!NOTE]
> Il primo backup è un backup completo di tutti i dati selezionati. Tuttavia, i backup successivi sono di natura incrementale e trasferiscono solo le modifiche apportate ai dati dopo il backup precedente.

- Consente di selezionare tra più **pianificazioni di backup** predefinite per lo stato del sistema e/o i volumi.

### <a name="step-4-enter-encryption-passphrase"></a>Passaggio 4: Immettere la passphrase di crittografia

- Immettere una **passphrase di crittografia** di propria scelta (minimo 16 caratteri).  **Backup di Azure** protegge i dati di backup con una passphrase di crittografia configurata dall'utente e gestita dall'utente. La passphrase di crittografia è necessaria per ripristinare i dati da backup di Azure.

> [!NOTE]
> La passphrase deve essere archiviata in una posizione esterna sicura, ad esempio un altro server o il [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal). Microsoft non archivia la passphrase e non può recuperare o reimpostare la passphrase se viene persa o dimenticata.

- Esaminare tutte le impostazioni e fare clic su **applica**

Il centro di amministrazione di Windows eseguirà quindi le operazioni seguenti

1. Creare un gruppo di risorse di Azure, se non esiste già
2. Creare un insieme di credenziali di servizi di ripristino di Azure come specificato
3. Installare e registrare l'agente di Servizi di ripristino di Microsoft Azure nell'insieme di credenziali
4. Creare il backup e la pianificazione di conservazione in base alle opzioni selezionate e associarle a Windows Server.

## <a name="management-and-monitoring"></a>Gestione e monitoraggio

Dopo aver configurato correttamente backup di Azure, il **dashboard di backup** verrà visualizzato quando si apre lo strumento di backup per una connessione server esistente. È possibile eseguire le attività seguenti dal **dashboard di backup**

- **Accedere all'insieme di credenziali in Azure:** È possibile fare clic sul collegamento dell'insieme di credenziali di **servizi di ripristino** nella scheda **Panoramica** del **dashboard di backup** da portare all'insieme di credenziali in Azure per eseguire un [set completo di operazioni di gestione](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **Eseguire un backup ad hoc:** Fare clic su **Esegui backup ora** per eseguire un backup ad hoc. 
- **Monitorare i processi e configurare le notifiche di avviso:** Passare alla scheda **processi** del dashboard per monitorare i processi in corso o in passato e [configurare le notifiche di avviso](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) per ricevere messaggi di posta elettronica per eventuali processi non riusciti o altri avvisi correlati al backup.
- **Visualizzare i punti di ripristino e ripristinare i dati:** Fare clic sulla scheda **punti di ripristino** del dashboard per visualizzare i punti di ripristino e fare clic su **Ripristina dati** per i passaggi per ripristinare i dati da Azure.