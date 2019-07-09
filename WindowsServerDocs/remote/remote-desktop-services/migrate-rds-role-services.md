---
title: Eseguire la migrazione della distribuzione di Servizi Desktop remoto a Windows Server 2016
description: Questo articolo descrive come eseguire la migrazione della distribuzione di Servizi Desktop remoto ai nuovi server Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b1fa833-4325-48a8-bf34-46265f40c001
author: christianmontoya
manager: scottman
ms.openlocfilehash: 0e4736f753fc0ad2ece6135de84d481eecb8b7a1
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812591"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Eseguire la migrazione della distribuzione di Servizi Desktop remoto a Windows Server 2016

Se attualmente Servizi Desktop remoto è in esecuzione in Windows Server 2012 R2, è possibile spostare questo ruolo in Windows Server 2016 per sfruttare i vantaggi delle nuove funzionalità, come il supporto per Azure SQL e Spazi di archiviazione diretta.

La migrazione di una distribuzione di Servizi Desktop remoto è supportata da server di origine che eseguono Windows Server 2016 a server di destinazione che eseguono Windows Server 2016. In altre parole, non è possibile eseguire una migrazione sul posto diretta da Servizi Desktop remoto in Windows Server 2012 R2 a Windows Server 2016. Per la maggior parte dei componenti di Servizi Desktop remoto è invece necessario eseguire prima l'aggiornamento a Windows Server 2016 e quindi eseguire la migrazione di dati e licenze. Gli unici componenti che consentono la migrazione diretta sono Accesso Web Desktop remoto, Gateway Desktop remoto e il server licenze.

Per altre informazioni sui requisiti e sui processi di aggiornamento, vedi [Aggiornamento delle distribuzioni di Servizi Desktop remoto a Windows Server 2016](upgrade-to-rds-2016.md).

Per eseguire la migrazione della distribuzione di Servizi Desktop remoto, segui questi passaggi:

- [Eseguire la migrazione dei server Gestore connessione Desktop remoto](#migrate-rdconnection-broker-servers)

- [Eseguire la migrazione degli insiemi di sessioni](#migrate-session-collections)

- [Eseguire la migrazione degli insiemi di desktop virtuali](#migrate-virtual-desktop-collections)

- [Eseguire la migrazione dei server Accesso Web Desktop remoto](#migrate-rdweb-access-servers)

- [Eseguire la migrazione dei server Gateway Desktop remoto](#migrate-rdgateway-servers)

- [Eseguire la migrazione dei server licenze Desktop remoto](#migrate-rdgateway-servers)

- [Eseguire la migrazione dei certificati](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Eseguire la migrazione dei server Gestore connessione Desktop remoto

La migrazione delle istanze di Gestore connessione Desktop remoto nei server di destinazione che eseguono Windows Server 2016 è il primo e il più importante passaggio per la migrazione.

> [!IMPORTANT]
> Per supportare la migrazione, i server di origine di Gestore connessione Desktop remoto devono essere configurati per la disponibilità elevata. Per altre informazioni, vedi [Distribuire un cluster di Gestore connessione Desktop remoto](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md).

1. Se la configurazione per la disponibilità elevata include più di un server Gestore connessione Desktop remoto, rimuovere tutti i server Gestore connessione Desktop remoto ad eccezione di quello attivo.

2. [Aggiorna](upgrade-to-rds-2016.md) il server Gestore connessione Desktop remoto rimanente nella distribuzione a Windows Server 2016.

3. Aggiungi i server Gestore connessione Desktop remoto Windows Server 2016 alla distribuzione a disponibilità elevata.

> [!NOTE] 
> Una configurazione a disponibilità elevata mista con Windows Server 2016 e Windows Server 2012 R2 non è supportata per i server Gestore connessione Desktop remoto. Un'istanza di Gestore connessione Desktop remoto che esegue Windows Server 2016 può gestire insiemi di sessioni con server Host sessione Desktop remoto che eseguono Windows Server 2012 R2, nonché insiemi di desktop virtuali con server Host di virtualizzazione Desktop remoto che eseguono Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Eseguire la migrazione degli insiemi di sessioni

Per eseguire la migrazione di un insieme di sessioni in Windows Server 2012 R2 a un insieme di sessioni in Windows Server 2016, segui questa procedura.

> [!IMPORTANT]
> Esegui la migrazione degli insiemi di sessioni solo dopo aver completato correttamente il passaggio precedente [Eseguire la migrazione dei server Gestore connessione Desktop remoto](#migrate-rdconnection-broker-servers).

1. [Aggiorna l'insieme di sessioni](Upgrade-to-RDSH-2016.md) da Windows Server 2012 R2 a Windows Server 2016.

2. Aggiungi il nuovo server Host sessione Desktop remoto che esegue Windows Server 2016 all'insieme di sessioni.

3. Disconnettersi da tutte le sessioni nei server Host sessione Desktop remoto e rimuovere i server di cui è necessario eseguire la migrazione dall'insieme di sessioni.

   > [!NOTE]
   > Se il modello UVHD (UVHD-template.vhdx) è abilitato nell'insieme di sessioni ed è stata completata la migrazione del file server in un nuovo server, aggiorna la proprietà relativa alla posizione dei dischi dei profili utente per l'insieme specificando il nuovo percorso. Nel nuovo percorso, i dischi dei profili utente devono essere disponibili nello stesso percorso relativo del server di origine.
   >
   > Non è supportato un insieme di sessioni di server Host sessione Desktop remoto con una combinazione di server che eseguono Windows Server 2012 R2 e Windows Server 2016.

## <a name="migrate-virtual-desktop-collections"></a>Eseguire la migrazione degli insiemi di desktop virtuali

Per eseguire la migrazione di un insieme di desktop virtuali da un server di origine che esegue Windows Server 2012 R2 a un server di destinazione che esegue Windows Server 2016, segui questa procedura.

> [!IMPORTANT]
> Esegui la migrazione degli insiemi di desktop virtuali solo dopo aver completato il passaggio precedente, [Eseguire la migrazione dei server Gestore connessione Desktop remoto](#migrate-rdconnection-broker-servers).

1. [Aggiorna l'insieme di desktop virtuali](Upgrade-to-RDVH-2016.md) dal server che esegue Windows Server 2012 R2 a Windows Server 2016.

2. Aggiungi i nuovi server Host di virtualizzazione Desktop remoto Windows Server 2016 all'insieme di desktop virtuali.

3. Eseguire la migrazione nei nuovi server di tutte le macchine virtuali nell'insieme di desktop virtuali corrente, eseguite in server Host di virtualizzazione Desktop remoto.

4. Rimuovere tutti i server Host di virtualizzazione Desktop remoto per i quali è stata necessaria la migrazione dall'insieme di desktop virtuali nel server di origine.

> [!NOTE]
> Se il modello UVHD (UVHD-template.vhdx) è abilitato nell'insieme di sessioni ed è stata completata la migrazione del file server in un nuovo server, aggiorna la proprietà relativa alla posizione dei dischi dei profili utente per l'insieme specificando il nuovo percorso. Nel nuovo percorso, i dischi dei profili utente devono essere disponibili nello stesso percorso relativo del server di origine.
>
> Non è supportato un insieme di desktop virtuali di server Host di virtualizzazione Desktop remoto con una combinazione di server che eseguono Windows Server 2012 R2 e Windows Server 2016.

## <a name="migrate-rdweb-access-servers"></a>Eseguire la migrazione dei server Accesso Web Desktop remoto

Pr eseguire la migrazione dei server Accesso Web Desktop remoto, segui questa procedura:

- Aggiungi i server di destinazione che eseguono Windows Server 2016 alla distribuzione di Servizi Desktop remoto e installa il ruolo Accesso Web Desktop remoto

- Usa lo [strumento di distribuzione Web di IIS](https://www.iis.net/) per eseguire la migrazione delle impostazioni del sito Web Accesso Web Desktop remoto dai server Accesso Web desktop remoto correnti ai server di destinazione che eseguono Windows Server 2016.

- [Esegui la migrazione dei certificati](#migrate-certificates) nei server di destinazione che eseguono Windows Server 2016.

- Rimuovi i server di origine dalla distribuzione di Servizi Desktop remoto.

## <a name="migrate-rdgateway-servers"></a>Eseguire la migrazione dei server Gateway Desktop remoto

Per eseguire la migrazione dei server Gateway Desktop remoto, segui questa procedura:

- Aggiungi i server di destinazione che eseguono Windows Server 2016 alla distribuzione di Servizi Desktop remoto e installa il ruolo Gateway Desktop remoto

- Usa lo [strumento di distribuzione Web di IIS](https://www.iis.net/) per eseguire la migrazione delle impostazioni degli endpoint di Gateway Desktop remoto dai server Gateway Desktop remoto correnti ai server di destinazione che eseguono Windows Server 2016.

- [Esegui la migrazione dei certificati](#migrate-certificates) nei server di destinazione che eseguono Windows Server 2016.

- Rimuovi i server di origine dalla distribuzione di Servizi Desktop remoto.

## <a name="migrate-rdlicensing-servers"></a>Eseguire la migrazione dei server licenze Desktop remoto

Per eseguire la migrazione di un server licenze Desktop remoto da un server di origine che esegue Windows Server 2012 o Windows Server 2012 R2 a un server di destinazione che esegue Windows Server 2016, segui questa procedura.

1. [Esegui la migrazione delle licenze CAL Servizi Desktop remoto](migrate-rds-cals.md) dal server di origine al server di destinazione.

2. Modifica i valori di **Proprietà distribuzione** in **Server Manager** nel server di gestione Desktop remoto (in genere eseguito nel primo server Gestore connessione Desktop remoto) per includere solo i nuovi server licenze Desktop remoto che eseguono Windows Server 2016.

3. Disattiva il server licenze Desktop remoto di origine: In **Gestione licenze Desktop remoto** fai clic con il pulsante destro del mouse sul server appropriato, passa il puntatore su **Avanzate** per selezionare **Disattiva server** e quindi segui i passaggi nella procedura guidata.

4. Rimuovi i server licenze Desktop remoto di origine dalla distribuzione in **Server Manager** nel server di gestione Desktop remoto.

## <a name="migrate-certificates"></a>Eseguire la migrazione dei certificati

Per la migrazione dei certificati sono necessari sia il processo effettivo di migrazione dei certificati che l'aggiornamento delle informazioni sui certificati nelle proprietà di distribuzione di Servizi Desktop remoto.

Una tipica operazione di migrazione dei certificati include i passaggi seguenti:

- Esportare il certificato in un file PFX con la chiave privata.

- Importare il certificato da un file PFX.

Dopo la migrazione dei certificati appropriati, aggiorna i certificati richiesti seguenti per la distribuzione di Servizi Desktop remoto in Server Manager o in PowerShell:

- Gestore connessione Desktop remoto - Single Sign-On

- Gestore connessione Desktop remoto - Pubblicazione file RDP

- Gateway Desktop remoto - Connessione HTTPS

- Accesso Web Desktop remoto - Connessione HTTPS e sottoscrizione di connessioni RemoteApp/desktop
