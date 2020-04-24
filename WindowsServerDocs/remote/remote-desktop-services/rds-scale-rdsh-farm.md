---
title: Aumentare una distribuzione di Servizi Desktop remoto aggiungendo una farm host sessione Desktop remoto
description: Aggiungere un secondo host sessione Desktop remoto al proprio ambiente di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 61d5569c44d7c7ea300b85bf635fccf86275423d
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80859054"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Aumentare la propria distribuzione di Servizi Desktop remoto aggiungendo una farm host sessione Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

È possibile migliorare la disponibilità e ridimensionare la distribuzione di Servizi Desktop remoto aggiungendo una farm host sessione Desktop remoto.   
  
 
Per aggiungere un altro host sessione Desktop remoto alla distribuzione, usare la procedura seguente:  
  
1. Creare un server per ospitare il secondo host sessione Desktop remoto. Se si usano macchine virtuali di Azure, assicurarsi di includere la nuova macchina virtuale nello stesso set di disponibilità che contiene il primo host sessione Desktop remoto.
2. Abilitare la gestione remota sul nuovo server o sulla macchina virtuale:
   1. In Server Manager fare clic su **Server locale >Remote management current setting (disabled)** (Impostazione corrente di gestione remota (disabilitata)). 
   2. Selezionare **Enable remote management for this server** (Abilita la gestione remota per questo server), quindi fare clic su **OK**. 
   3. Facoltativo: È possibile impostare temporaneamente Windows Update in modo da non scaricare e installare automaticamente gli aggiornamenti. In questo modo è possibile impedire modifiche e riavvii del sistema durante la distribuzione del server Host sessione Desktop remoto. In Server Manager fare clic su **Server locale > Impostazione corrente di Windows Update**. Fare clic su **Opzioni avanzate > Ritarda aggiornamenti**. 
3. Aggiungere il server o la macchina virtuale al dominio:
   1. In Server Manager fare clic su **Server locale > Workgroup current setting** (Impostazione corrente di gruppo di lavoro). 
   2. Fai clic su **Cambia > Dominio** e quindi immetti il nome di dominio (ad esempio Contoso.com). 
   3. Immettere le credenziali di amministratore del dominio. 
   4. Riavviare il server o la macchina virtuale.
4. Aggiungere il nuovo host sessione Desktop remoto alla farm:
   >[!NOTE] 
   > Il passaggio 1, la creazione di un indirizzo IP pubblico per la macchina virtuale RDBMS, è necessario solo se si usa una macchina virtuale per i servizi di gestione Desktop remoto e se non si dispone di un indirizzo IP assegnato.
   
   1. Creare un indirizzo IP pubblico per la macchina virtuale in esecuzione servizi di gestione Desktop remoto (RDBMS). La macchina virtuale RDBMS sarà in genere la macchina virtuale che esegue la prima istanza del ruolo di Gestore connessione desktop remoto.  
       1. Nel portale di Azure, fare clic su **Sfoglia > gruppi di risorse**, fare clic sul gruppo di risorse per la distribuzione e quindi fare clic su macchina virtuale RDBMS (ad esempio, Contoso-Cb1).  
       2. Fare clic su **Impostazioni > interfacce di rete**, quindi scegliere l'interfaccia di rete corrispondente.   
       3. Fare clic su **Impostazioni > indirizzo IP**.
       4. Per **indirizzo IP pubblico**, selezionare **Enabled**, quindi fare clic su **indirizzo IP**.   
       5. Se si dispone di un indirizzo IP pubblico esistente da utilizzare, selezionarlo dall'elenco. In caso contrario, fare clic su **Create new**, immettere un nome e quindi fare clic su **OK** e quindi **salvare**.   
   2. Accedere ai servizi di gestione Desktop remoto.
   3. Aggiungere il nuovo server host sessione Desktop remoto per Server Manager:   
       1. Avviare Server Manager fare clic su **Gestisci > Aggiungi server**.   
       2. Nella finestra di dialogo Aggiungi server, fare clic su **Trova**.   
       3. Selezionare il server che si vuole usare per l'host sessione Desktop remoto o la macchina virtuale appena creata (ad esempio, Contoso-Sh2) e fare clic su **OK**.
   4. Aggiungere il server host sessione Desktop remoto per la distribuzione
       1. Avviare Server Manager.  
       2. Fare clic su **Servizi Desktop remoto > Panoramica > server di distribuzione > attività > Aggiungi server Host sessione Desktop remoto**.   
       3. Selezionare il nuovo server (ad esempio, Contoso-Sh2) e quindi fare clic su **Avanti**.  
       4. Nella pagina di conferma, selezionare **riavviare i computer remoti in base alle esigenze**, quindi fare clic su **Aggiungi**.   
   5. Aggiungere il server host sessione Desktop remoto per la farm di raccolta:
       1. Avvia Server Manager.   
       2. Fare clic su **Servizi Desktop remoto** e quindi fare clic sulla raccolta in cui si vuole aggiungere il server host sessione Desktop remoto appena creato (ad esempio, ContosoDesktop).   
       3. In **Server Host** fare clic su **Attività > Aggiungi server Host sessione Desktop remoto**.   
       4. Selezionare il server appena creato (ad esempio, Contoso-Sh2) e quindi fare clic su **Avanti**.   
       5. Nella pagina di conferma fare clic su **Aggiungi**.   

