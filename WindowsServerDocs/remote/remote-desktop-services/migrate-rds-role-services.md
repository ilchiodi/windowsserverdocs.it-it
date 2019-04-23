---
title: Eseguire la migrazione della distribuzione di Servizi Desktop remoto a Windows Server 2016
description: Questo articolo descrive come eseguire la migrazione della distribuzione di Servizi Desktop remoto a nuovi server di Windows Server 2016.
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
ms.openlocfilehash: 73beb1539420f4b4aad818ffe0b0bdaabe901748
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870262"
---
# <a name="migrate-your-remote-desktop-services-deployment-to-windows-server-2016"></a>Eseguire la migrazione della distribuzione di Servizi Desktop remoto a Windows Server 2016

Se attualmente in esecuzione Servizi Desktop remoto in Windows Server 2012 R2, è possibile spostare in Windows Server 2016 per sfruttare i vantaggi delle nuove funzionalità come il supporto per SQL Azure e spazi di archiviazione diretta.

La migrazione di una distribuzione di Servizi Desktop remoto è supportata dai server di origine che esegue Windows Server 2016 a server di destinazione che eseguono Windows Server 2016. In altre parole, non vi è alcuna migrazione sul posto diretta da Servizi Desktop remoto in Windows Server 2012 R2 a Windows Server 2016. Invece per la maggior parte dei componenti Servizi Desktop remoto, eseguire prima l'aggiornamento a Windows Server 2016 e quindi eseguire la migrazione dei dati e le licenze. Gli unici componenti con una migrazione diretta sono Web desktop remoto, Gateway Desktop remoto e il server licenze.

Per altre informazioni sul processo di aggiornamento e i requisiti, vedere [l'aggiornamento delle distribuzioni di Servizi Desktop remoto a Windows Server 2016](upgrade-to-rds-2016.md).

Usare la procedura seguente per eseguire la migrazione della distribuzione di Servizi Desktop remoto: 
- [Eseguire la migrazione di Gestore connessione desktop remoto](#migrate-rd-connection-broker-servers) 
- [Eseguire la migrazione di insiemi di sessioni](#migrate-session-collections)
- [Eseguire la migrazione di insiemi di desktop virtuali](#migrate-virtual-desktop-collections)
- [Eseguire la migrazione di server Accesso Web desktop remoto](#migrate-rd-web-access-servers)
- [Eseguire la migrazione di server Gateway Desktop remoto](#migrate-rd-gateway-servers)
- [Eseguire la migrazione di server licenze Desktop remoto](#migrate-rd-licensing-servers)
- [Eseguire la migrazione dei certificati](#migrate-certificates)

## <a name="migrate-rdconnection-broker-servers"></a>Eseguire la migrazione di Gestore connessione desktop remoto

Questo è il primo e più importante passaggio per la migrazione: migrazione di Gestore connessione desktop remoto ai server di destinazione che esegue Windows Server 2016.
> [!IMPORTANT] 
> Il server di origine Gestore connessione Desktop remoto (Gestore connessione desktop remoto) devono essere configurati per la disponibilità elevata supportare la migrazione. Per altre informazioni, vedere [distribuire un cluster di Gestore connessione Desktop remoto](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md).

1. Se la configurazione per la disponibilità elevata include più di un server Gestore connessione Desktop remoto, rimuovere tutti i server Gestore connessione Desktop remoto ad eccezione di quello attivo.
2. [Eseguire l'aggiornamento](upgrade-to-rds-2016.md) il server Gestore connessione desktop remoto rimanente nella distribuzione a Windows Server 2016.
3. Aggiungere i server Gestore connessione desktop remoto di Windows Server 2016 la distribuzione a disponibilità elevata.

> [!NOTE] 
> Una configurazione mista della disponibilità elevata con Windows Server 2016 e Windows Server 2012 R2 non è supportata per i server Gestore connessione desktop remoto. Un gestore connessione desktop remoto che esegue Windows Server 2016 può gestire insiemi di sessioni con server Host sessione Desktop remoto che esegue Windows Server 2012 R2 e può gestire insiemi di desktop virtuali con server Host di virtualizzazione Desktop remoto che esegue Windows Server 2012 R2.

## <a name="migrate-session-collections"></a>Eseguire la migrazione degli insiemi di sessioni

Seguire questi passaggi per eseguire la migrazione di un insieme di sessioni in Windows Server 2012 R2 a un insieme di sessioni di Windows Server 2016.
> [!IMPORTANT] 
> Eseguire la migrazione di insiemi di sessioni solo dopo aver completato il passaggio precedente, [eseguire la migrazione di Gestore connessione desktop remoto](#Migrate-RD-Connection-Broker-servers).

1. [Aggiornare l'insieme di sessioni](Upgrade-to-RDSH-2016.md) da Windows Server 2012 R2 a Windows Server 2016.
2. Aggiungere il nuovo server Host sessione Desktop remoto che esegue Windows Server 2016 all'insieme di sessioni.
3. Disconnettersi da tutte le sessioni nei server Host sessione Desktop remoto e rimuovere i server di cui è necessario eseguire la migrazione dall'insieme di sessioni. 
> [!NOTE]
> Se il modello UVHD (UVHD-template. vhdx) è abilitato nell'insieme di sessioni e il file server è stato migrato in un nuovo server, aggiornare i dischi dei profili utente: Proprietà della raccolta posizione con il nuovo percorso. Nel nuovo percorso, i dischi dei profili utente devono essere disponibili nello stesso percorso relativo del server di origine.
>
> Non è supportato un insieme di sessioni di server Host sessione Desktop remoto con una combinazione di server che eseguono Windows Server 2012 R2 e Windows Server 2016.

## <a name="migrate-virtual-desktop-collections"></a>Eseguire la migrazione degli insiemi di desktop virtuali

Seguire questi passaggi per eseguire la migrazione di un insieme di desktop virtuali da un server di origine esegue Windows Server 2012 R2 a un server di destinazione che esegue Windows Server 2016.

> [!IMPORTANT] 
> Eseguire la migrazione di insiemi di desktop virtuali solo dopo aver completato il passaggio precedente, [eseguire la migrazione di Gestore connessione desktop remoto](#Migrate-RD-Connection-Broker-servers).

1. [Aggiornare l'insieme di desktop virtuali](Upgrade-to-RDVH-2016.md) dal server che esegue Windows Server 2012 R2 a Windows Server 2016.
2. Aggiungere i nuovi server Host di virtualizzazione Desktop remoto di Windows Server 2016 all'insieme di desktop virtuali.
3. Eseguire la migrazione nei nuovi server di tutte le macchine virtuali nell'insieme di desktop virtuali corrente, eseguite in server Host di virtualizzazione Desktop remoto. 
4. Rimuovere tutti i server Host di virtualizzazione Desktop remoto per i quali è stata necessaria la migrazione dall'insieme di desktop virtuali nel server di origine.

> [!NOTE] 
> Se il modello UVHD (UVHD-template. vhdx) è abilitato nell'insieme di sessioni e il file server è stato migrato in un nuovo server, aggiornare i dischi dei profili utente: Proprietà della raccolta posizione con il nuovo percorso. Nel nuovo percorso, i dischi dei profili utente devono essere disponibili nello stesso percorso relativo del server di origine.
>
> Non è supportato un insieme di desktop virtuali dei server Host di virtualizzazione Desktop remoto con una combinazione di server che eseguono Windows Server 2012 R2 e Windows Server 2016.

## <a name="migrate-rdweb-access-servers"></a>Eseguire la migrazione dei server Accesso Web Desktop remoto
Seguire questi passaggi per la migrazione di server di accesso Web desktop remoto:
- Aggiungere i server di destinazione che esegue Windows Server 2016 per la distribuzione di Servizi Desktop remoto e installare il ruolo Web desktop remoto
- Uso [strumento di distribuzione Web IIS](https://www.iis.net/) per eseguire la migrazione delle impostazioni del sito Web di Web desktop remoto dai server Accesso Web desktop remoto corrente per i server di destinazione che eseguono Windows Server 2016.
- [Eseguire la migrazione di certificati](#Migrate-certificates) ai server di destinazione che esegue Windows Server 2016.
- Rimuovere i server di origine dalla distribuzione di Servizi Desktop remoto  

## <a name="migrate-rdgateway-servers"></a>Eseguire la migrazione dei server Gateway Desktop remoto
Seguire questi passaggi per eseguire la migrazione di server Gateway Desktop remoto:
- Aggiungere i server di destinazione che esegue Windows Server 2016 per la distribuzione di Servizi Desktop remoto e installare il ruolo Gateway Desktop remoto
- Uso [strumento di distribuzione Web IIS](https://www.iis.net/) per migrare le impostazioni di endpoint di Gateway Desktop remoto da correnti dei server Gateway Desktop remoto ai server di destinazione che esegue Windows Server 2016.
- [Eseguire la migrazione di certificati](#Migrate-certificates) ai server di destinazione che esegue Windows Server 2016.
- Rimuovere i server di origine dalla distribuzione di Servizi Desktop remoto  

## <a name="migrate-rdlicensing-servers"></a>Eseguire la migrazione dei server licenze Desktop remoto

Seguire questi passaggi per eseguire la migrazione di un server licenze Desktop remoto da un server di origine che esegue Windows Server 2012 o Windows Server 2012 R2 a un server di destinazione che esegue Windows Server 2016.

1. [Eseguire la migrazione le licenze di accesso client (CAL) di Servizi Desktop remoto](migrate-rds-cals.md) dal server di origine al server di destinazione.
2. Modificare il **le proprietà di distribuzione** nelle **Server Manager** nel server di gestione Desktop remoto (che in genere viene eseguito nel primo server Gestore connessione desktop remoto) per includere solo il nuovo servizio licenze Desktop remoto server che eseguono Windows Server 2016.
3. Disattivare il server licenze Desktop remoto di origine: Nelle **gestione licenze Desktop remoto**, fare clic sul server appropriato, passare il mouse su **Advanced** per selezionare **Disattiva Server**e quindi seguire i passaggi della procedura guidata .
4. Rimuovere i server licenze Desktop remoto di origine dalla distribuzione in **Server Manager** nel server di gestione Desktop remoto.

## <a name="migrate-certificates"></a>Eseguire la migrazione dei certificati

Migrazione dei certificati ha esito positivo richiede entrambi l'effettivo processo di migrazione di certificati e l'aggiornamento delle informazioni sul certificato nelle proprietà di distribuzione di Servizi Desktop remoto.

Migrazione dei certificati tipico include i passaggi seguenti:
- Esportare il certificato in un file PFX con la chiave privata.
- Importare il certificato da un file PFX.

Dopo la migrazione dei certificati appropriati, aggiornare i certificati necessari seguenti per la distribuzione di Servizi Desktop remoto in server manager o PowerShell: 
- Gestore connessione desktop remoto - accesso single sign-on
- Gestore connessione desktop remoto - pubblicazione del file RDP
- Gateway Desktop remoto - connessione HTTPS
- Accesso Web desktop remoto - connessione HTTPS e la sottoscrizione di connessione/desktop RemoteApp
