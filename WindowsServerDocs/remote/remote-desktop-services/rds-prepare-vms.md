---
title: Preparare le macchine virtuali per Desktop remoto
description: Preparare le macchine virtuali per i componenti di Desktop remoto
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 07/21/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fc39dff-61ca-4eba-81ab-52289081bead
author: lizap
manager: dongill
ms.openlocfilehash: cb06963c3ae51fb9337827c7b29b93b8c2736c16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871882"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creare macchine virtuali per Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile installare i componenti Servizi Desktop remoto in server fisici o in macchine virtuali. 

Il primo passaggio consiste nel [creare le macchine virtuali Windows Server in Azure](/azure/virtual-machines/windows/quick-create-portal). È opportuno creare tre macchine virtuali: uno per l'Host sessione Desktop remoto, una per il gestore di connessione e uno per il Web desktop remoto e Gateway Desktop remoto. Per garantire la disponibilità della distribuzione di servizi desktop remoto, creare un disponibilità imposta (sotto **disponibilità elevata** nel processo di creazione della macchina virtuale) e raggruppare più macchine virtuali, in quanto i set di disponibilità.
 
Dopo aver creato le macchine virtuali, attenersi alla seguente procedura per prepararle per Servizi Desktop remoto.

1.  Connettersi alla macchina virtuale usando il client connessione Desktop remoto (RDC):  
    1.  Nel portale di Azure aprire la visualizzazione di gruppi di risorse e quindi fare clic sul gruppo di risorse da usare per la distribuzione.  
    2.  Selezionare la nuova macchina virtuale host sessione Desktop remoto (ad esempio, Contoso-Sh1).  
    3.  Fare clic su **Connetti > aprire** per aprire il client Desktop remoto.  
    4.  Nel client, fare clic su **Connect**, quindi fare clic su **utilizza un altro account utente**. Immettere il nome utente e la password per l'account amministratore locale.  
    5.  Fare clic su **Sì** quando l'avviso sul certificato.  
2.  Abilitare la gestione remota:  
    1.  In Server Manager fare clic su **Server locale > impostazione corrente di gestione remota (disabilitata)**.  
    2.  Selezionare **abilitare la gestione remota per questo server**.  
    3.  Fare clic su **OK**.  
3.  Facoltativo: È possibile impostare temporaneamente Windows Update per scaricare e installare gli aggiornamenti automaticamente. In tal modo le modifiche e diversi riavvii del sistema mentre si distribuisce il server host sessione Desktop remoto.  
    1.  In Server Manager fare clic su **Server locale > impostazione corrente di Windows Update**.  
    2.  Selezionare **Advanced options > rinviare gli aggiornamenti**.   
4.  Aggiungere il server al dominio:  
    1.  In Server Manager fare clic su **Server locale > impostazione corrente del gruppo di lavoro**.  
    2.  Fare clic su **Modifica > dominio**, quindi immettere il nome di dominio (ad esempio, Contoso.com).  
    3.  Immettere le credenziali di amministratore di dominio.  
    4.  Riavviare la macchina virtuale.  
5.  Ripetere i passaggi da 1 a 4 per la macchina virtuale Web desktop remoto e gateway.  
6.  Ripetere i passaggi da 1 a 4 per la macchina virtuale di Gestore connessione desktop remoto.  
7.  Inizializzare e formattare il disco collegato nella macchina virtuale di Gestore connessione desktop remoto:  
    1.  Connettersi alla macchina virtuale di Gestore connessione desktop remoto (passaggio 1 precedente).  
    2.  In Server Manager fare clic su **strumenti > Gestione Computer**.  
    3.  Fare clic su **Gestione disco**.  
    4.  Selezionare il disco collegato, quindi **MBR (Master Boot Record)**, quindi fare clic su **OK**.  
    5.  Pulsante destro del mouse del nuovo disco (contrassegnata come **non allocata**) e fare clic su **nuovo Volume semplice**.  
    6.  Nel **nuovo Volume semplice** procedura guidata, accettare i valori predefiniti, ma consente di specificare un nome applicabile per il **etichetta di Volume** (ad esempio condivisioni).  
8.  Creare condivisioni file per i dischi dei profili utente e i certificati nella macchina virtuale di Gestore connessione desktop remoto:   
    1.  Aprire Esplora File, fare clic su **computer**e aprire il disco che è stato aggiunto per le condivisioni file.  
    2.  Fare clic su **casa** e **nuova cartella**.  
    3.  Immettere un nome per la cartella utente dischi, ad esempio, **UserDisks**.  
    4.  Fare doppio clic su nuova cartella e fare clic su **proprietà > condivisione > Condivisione avanzata**.  
    5.  Selezionare **Condividi questa cartella** e fare clic su **autorizzazioni**.  
    6.  Selezionare **Everyone**, quindi fare clic su **rimuovere**. Fare clic su **Aggiungi**, immettere **Domain Admins**, fare clic su **OK**.  
    7.  Selezionare **consentono il controllo completo**, quindi fare clic su **OK > OK > Chiudi**.  
    8.  Ripetere i passaggi da c. -g. Per creare una cartella condivisa per i certificati.   


