---
title: Preparare le macchine virtuali per Desktop remoto
description: Prepara le macchine virtuali per i componenti di Desktop remoto
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: 5aec90275db1e09906e051419929086a40a34800
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80858134"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creare macchine virtuali per Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

È possibile installare i componenti di Servizi Desktop remoto in server fisici o in macchine virtuali. 

Il primo passaggio consiste nel [creare macchine virtuali Windows Server in Azure](/azure/virtual-machines/windows/quick-create-portal). Dovrai creare tre macchine virtuali: una per Host sessione Desktop remoto, una per Gestore connessione e una per Web Desktop Remoto e Gateway Desktop remoto. Per garantire la disponibilità della distribuzione di Servizi Desktop remoto, crea un set di disponibilità (in **Disponibilità elevata** nel processo di creazione della macchina virtuale) e raggruppa più macchine virtuali nel set di disponibilità.
 
Dopo aver creato le macchine virtuali, segui questa procedura per prepararle per Servizi Desktop Remoto.

1.  Connettiti alla macchina virtuale con il client Connessione Desktop remoto:  
    1.  Nel portale di Azure apri la visualizzazione Gruppi di risorse e quindi fa clic sul gruppo di risorse da usare per la distribuzione.  
    2.  Seleziona la nuova macchina virtuale di Host sessione Desktop remoto (ad esempio Contoso-Sh1).  
    3.  Fare clic su **Connetti > aprire** per aprire il client Desktop remoto.  
    4.  Nel client, fare clic su **Connect**, quindi fare clic su **utilizza un altro account utente**. Immetti nome utente e password per l'account di amministratore locale.  
    5.  Fare clic su **Sì** quando l'avviso sul certificato.  
2.  Abilita la gestione remota:  
    1.  In Server Manager fai clic su **Server locale > Impostazione corrente di Gestione remota (Disabilitato)** .  
    2.  Seleziona **Enable remote management for this server** (Abilita la gestione remota per questo server).  
    3.  Fare clic su **OK**.  
3.  Facoltativo: È possibile impostare temporaneamente Windows Update in modo da non scaricare e installare automaticamente gli aggiornamenti. In questo modo è possibile impedire modifiche e riavvii del sistema durante la distribuzione del server Host sessione Desktop remoto.  
    1.  In Server Manager fai clic su **Server locale > Impostazione corrente di Windows Update**.  
    2.  Seleziona **Opzioni avanzate > Ritarda aggiornamenti**.   
4.  Aggiungi il server al dominio:  
    1.  In Server Manager fai clic su **Server locale > Impostazione corrente del gruppo di lavoro**.  
    2.  Fai clic su **Cambia > Dominio** e quindi immetti il nome di dominio (ad esempio Contoso.com).  
    3.  Immetti le credenziali di amministratore di dominio.  
    4.  Riavviare la macchina virtuale.  
5.  Ripeti i passaggi da 1 a 4 per la macchina virtuale di Web e Gateway Desktop remoto.  
6.  Ripeti i passaggi da 1 a 4 per la macchina virtuale di Gestore connessione Desktop remoto.  
7.  Inizializza e formatta il disco collegato nella macchina virtuale di Gestore connessione Desktop remoto:  
    1.  Connettiti alla macchina virtuale di Gestore connessione Desktop remoto (passaggio 1 sopra).  
    2.  In Server Manager fai clic su **Strumenti > Gestione computer**.  
    3.  Fai clic su **Gestione disco**.  
    4.  Seleziona il disco collegato, fai clic su **MBR (Record di avvio principale, Master Boot Record)** e quindi su **OK**.  
    5.  Fai clic con il pulsante destro del mouse sul nuovo disco (contrassegnato come **Non allocato**) e quindi fai clic su **Nuovo volume semplice**.  
    6.  Nella procedura guidata **Nuovo volume semplice** accetta i valori predefiniti ma specifica un nome appropriato per **Etichetta volume** (ad esempio Condivisioni).  
8.  Nella macchina virtuale di Gestore connessione Desktop remoto crea le condivisioni file per i dischi dei profili utente e i certificati:   
    1.  Apri Esplora file, fai clic su **Questo PC** e apri il disco aggiunto per le condivisioni file.  
    2.  Fai clic su **Home** e quindi su **Nuova cartella**.  
    3.  Immetti un nome per la cartella dei dischi utente, ad esempio **DischiUtente**.  
    4.  Fai doppio clic sulla nuova cartella e quindi fai clic su **Proprietà > Condivisione > Condivisione avanzata**.  
    5.  Seleziona **Condividi la cartella** e fai clic su **Autorizzazioni**.  
    6.  Selezionare **Everyone**, quindi fare clic su **rimuovere**. Fare clic su **Aggiungi**, immettere **Domain Admins**, fare clic su **OK**.  
    7.  Selezionare **consentono il controllo completo**, quindi fare clic su **OK > OK > Chiudi**.  
    8.  Ripeti i passaggi da c. a g. per creare una cartella condivisa per i certificati.   


