---
title: Scalare orizzontalmente la distribuzione di servizi desktop remoto mediante l'aggiunta di una farm Host sessione Desktop remoto
description: Aggiungere un secondo Host sessione Desktop remoto nell'ambiente di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: d243994a68c0bf4f0584f68475a185acb9cb73d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865492"
---
# <a name="scale-out-your-remote-desktop-services-deployment-by-adding-an-rd-session-host-farm"></a>Scalare orizzontalmente la distribuzione di Servizi Desktop remoto mediante l'aggiunta di una farm Host sessione Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile migliorare la disponibilità e la scala della distribuzione di servizi desktop remoto mediante l'aggiunta di una farm Host sessione Desktop remoto (RDSH).   
  
 
Per aggiungere un altro Host sessione Desktop remoto alla distribuzione, procedere come segue:  
  
1. Creare un server per ospitare il secondo Host sessione Desktop remoto. Se si usa macchine virtuali di Azure, assicurarsi di includere la nuova macchina virtuale nello stesso set di disponibilità che contiene il primo Host sessione Desktop remoto.
2. Abilitare la gestione remota nel nuovo server o macchina virtuale:
   1. In Server Manager fare clic su **Server locale > impostazione corrente di gestione remota (disabilitata)**. 
   2. Selezionare **abilitare la gestione remota per questo server**, quindi fare clic su **OK**. 
   3. Facoltativo: È possibile impostare temporaneamente Windows Update per scaricare e installare gli aggiornamenti automaticamente. In tal modo le modifiche e diversi riavvii del sistema mentre si distribuisce il server host sessione Desktop remoto. In Server Manager fare clic su **Server locale > impostazione corrente di Windows Update**. Fare clic su **Advanced options > rinviare gli aggiornamenti**. 
3. Aggiungere il server o macchina virtuale al dominio:
   1. In Server Manager fare clic su **Server locale > impostazione corrente del gruppo di lavoro**. 
   2. Fare clic su **Modifica > dominio**, quindi immettere il nome di dominio (ad esempio, Contoso.com). 
   3. Immettere le credenziali di amministratore di dominio. 
   4. Riavviare il server o macchina virtuale.
4. Aggiungere il nuovo Host sessione Desktop remoto alla farm:
>[!NOTE] 
> Passaggio 1, creazione di un indirizzo IP pubblico per la macchina virtuale RDBMS, è necessario solo se si usa una macchina virtuale per il RDBMS e se dispone già di un indirizzo IP assegnato.
   
   1. Creare un indirizzo IP pubblico per la macchina virtuale in esecuzione servizi di gestione Desktop remoto (RDBMS). La macchina virtuale RDBMS sarà in genere la macchina virtuale che esegue la prima istanza del ruolo di Gestore connessione desktop remoto.  
       1. Nel portale di Azure, fare clic su **Sfoglia > gruppi di risorse**, fare clic sul gruppo di risorse per la distribuzione e quindi fare clic su macchina virtuale RDBMS (ad esempio, Contoso-Cb1).  
       2. Fare clic su **Impostazioni > interfacce di rete**, quindi scegliere l'interfaccia di rete corrispondente.   
       3. Fare clic su **Impostazioni > indirizzo IP**.
       4. Per **indirizzo IP pubblico**, selezionare **Enabled**, quindi fare clic su **indirizzo IP**.   
       5. Se si dispone di un indirizzo IP pubblico esistente da utilizzare, selezionarlo dall'elenco. In caso contrario, fare clic su **Create new**, immettere un nome e quindi fare clic su **OK** e quindi **salvare**.   
   2. Accesso Sign-on di RDBMS.
   3. Aggiungere il nuovo server host sessione Desktop remoto a Server Manager:   
       1. Avviare Server Manager fare clic su **Gestisci > Aggiungi server**.   
       2. Nella finestra di dialogo Aggiungi server, fare clic su **Trova**.   
       3. Selezionare il server di cui si vuole usare per l'Host sessione Desktop remoto o la macchina virtuale appena creata (ad esempio, Contoso-Sh2) e fare clic su **OK**.
   4. Aggiungere il server host sessione Desktop remoto alla distribuzione
       1. Avviare Server Manager.  
       2. Fare clic su **Servizi Desktop remoto > Panoramica > server di distribuzione > attività > Aggiungi server Host sessione Desktop remoto**.   
       3. Selezionare il nuovo server (ad esempio, Contoso-Sh2) e quindi fare clic su **successivo**.  
       4. Nella pagina di conferma, selezionare **riavviare i computer remoti in base alle esigenze**, quindi fare clic su **Aggiungi**.   
   5. Aggiungere i server host sessione Desktop remoto per la farm di raccolta:
       1. Avvia Server Manager.   
       2. Fare clic su **Servizi Desktop remoto** e quindi fare clic sulla raccolta a cui si desidera aggiungere il server host sessione Desktop remoto appena creato (ad esempio, ContosoDesktop).   
       3. Sotto **i server Host**, fare clic su **attività > Aggiungi server Host sessione Desktop remoto**.   
       4. Selezionare il server appena creato (ad esempio, Contoso-Sh2) e quindi fare clic su **successivo**.   
       5. Nella pagina di conferma, fare clic su **Add**.   

