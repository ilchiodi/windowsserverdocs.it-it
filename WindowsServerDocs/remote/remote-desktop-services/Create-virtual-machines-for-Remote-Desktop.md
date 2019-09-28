---
title: Creare macchine virtuali per Desktop remoto
description: Crea macchine virtuali per ospitare i componenti di Desktop remoto nel cloud.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0f62d6f-0915-44ca-afef-be44a922e20e
author: lizap
manager: dongill
ms.openlocfilehash: 6db9973402578736b2e1283dcfa405642591eccc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387979"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creare macchine virtuali per Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Usa la procedura seguente per creare le macchine virtuali nell'ambiente del tenant da usare per eseguire i servizi, le funzionalità e i ruoli di Windows Server 2016 necessari per una distribuzione dell'hosting di desktop.   
  
Per questo esempio di distribuzione di base, verrà creato il numero minimo di macchine virtuali, ovvero tre. Una macchina virtuale ospiterà i servizi ruolo Server licenze e Gestore connessione Desktop remoto e una condivisione file per la distribuzione. Una seconda macchina virtuale ospiterà i servizi ruolo Gateway Desktop remoto e Accesso Web.  Una terza macchina virtuale ospiterà il servizio ruolo Host sessione Desktop remoto. Per le distribuzioni di dimensioni molto piccole, è possibile ridurre i costi delle macchine virtuali usando Azure Active Directory Application Proxy per eliminare tutti gli endpoint pubblici dalla distribuzione e combinando tutti i servizi ruolo in una singola macchina virtuale. Per le distribuzioni più grandi, è possibile installare i diversi servizi ruolo in macchine virtuali distinte per consentire una migliore scalabilità.  
  
Questa sezione descrive i passaggi necessari per distribuire le macchine virtuali per ogni ruolo in base alle immagini di Windows Server in [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/). Se è necessario creare macchine virtuali da un'immagine personalizzata, che richiede PowerShell, vedi [Creare una macchina virtuale Windows in Azure con PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). Torna quindi a questo articolo per informazioni su come collegare i dischi dati di Azure per la condivisione file e immettere un URL esterno per la distribuzione.  
  
1. [Crea macchine virtuali Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) per ospitare Gestore connessione Desktop remoto, Server licenze Desktop remoto e il file server.  
  
   Per questa esercitazione sono state usate le convenzioni di denominazione seguenti:  
   - Gestore connessione Desktop remoto, Server licenze Desktop remoto e file server:   
       - VM: Contoso-Cb1  
       - Set di disponibilità: CbAvSet    
   - Accesso Web Desktop remoto e server Gateway Desktop remoto:   
       - VM: Contoso-WebGw1  
       - Set di disponibilità: WebGwAvSet  
          
   - Host sessione Desktop remoto:   
       - VM: Contoso-Sh1  
       - Set di disponibilità: ShAvSet  
          
   Ogni macchina virtuale usa lo stesso gruppo di risorse.  
2. Crea e collega un disco dati di Azure per la condivisione del disco profili utente:  
   1.  Nel portale di Azure fai clic su **Sfoglia > Gruppi di risorse**, fai clic sul gruppo di risorse per la distribuzione e quindi fai clic sulla macchina virtuale creata per Gestore connessione Desktop remoto (ad esempio, Contoso-Cb1).  
   2.  Fai clic su **Impostazioni > Dischi > Collega nuovo**.  
   3.  Accetta i valori predefiniti per nome e tipo.  
   4.  Immetti una dimensione (in GB) sufficientemente grande da contenere le condivisioni di rete per l'ambiente del tenant, inclusi i certificati e i dischi dei profili utente. Puoi calcolare approssimativamente 5 GB per utente previsto  
   5.  Accetta i valori predefiniti per posizione e memorizzazione nella cache dell'host e quindi fai clic su **OK**.  
3. Crea un servizio di bilanciamento del carico esterno per accedere alla distribuzione dall'esterno:
   1. Nel portale di Azure fai clic su **Sfoglia > Servizi di bilanciamento del carico** e quindi su **Aggiungi**.
   2. Immetti un valore in **Nome**, seleziona **Pubblico** come **Tipo** per il servizio di bilanciamento del carico e seleziona i valori appropriati per **Sottoscrizione**, **Gruppo di risorse** e **Posizione**.
   3. Seleziona **Scegliere un indirizzo IP pubblico**, **Crea nuovo**, immetti un nome e seleziona **OK**.
   4. Seleziona **Crea** per creare il servizio di bilanciamento del carico.
4. Configura il servizio di bilanciamento del carico esterno per la distribuzione
   1. Nel portale di Azure fai clic su **Sfoglia > Gruppi di risorse**, fai clic sul gruppo di risorse per la distribuzione e quindi fai clic sul servizio di bilanciamento del carico creato per la distribuzione.
   2. Aggiungi un pool back-end a cui il servizio di bilanciamento del carico può inviare il traffico:
       1. Seleziona **Pool back-end** e quindi **Aggiungi**.
       2. Immetti un valore in **Nome** e seleziona **\+ Aggiungi una macchina virtuale**.
       3. Seleziona **Set di disponibilità** e quindi **WebGwAvSet**.
       4. Seleziona **Macchine virtuali**, **Contoso-WebGw1**, **Seleziona**, **OK** e **OK**.
   3. Aggiungi un probe per consentire al servizio di bilanciamento del carico di sapere quali macchine sono attive:
       1. Seleziona **Probe** e quindi **Aggiungi**.
       2. Immetti un valore in **Nome** (ad esempio HTTPS), seleziona **TCP**, specifica 443 per **Porta** e seleziona **OK**.
   4. Immetti le regole per bilanciare il traffico in ingresso:
      1. Seleziona **Regole di bilanciamento del carico** e quindi **Aggiungi**
      2. Immetti un valore in **Nome** (ad esempio HTTPS), seleziona **TCP** e specifica 443 per **Porta** e per **Porta back-end**.
          - Per una distribuzione di Windows 10 e Windows Server 2016, lascia l'opzione **Salvataggio permanente sessione** impostata su **No** oppure seleziona **IP client**.
      3. Seleziona **OK** per accettare la regola HTTPS.
      4. Crea una nuova regola selezionando **Aggiungi**.
      5. Immetti un valore in **Nome** (ad esempio UDP), seleziona **UDP** e specifica 3391 per <strong>Porta e **Porta back-end</strong>.
          - Per una distribuzione di Windows 10 e Windows Server 2016, lascia l'opzione **Salvataggio permanente sessione** impostata su **No** oppure seleziona **IP client**.
      6. Seleziona **OK** per accettare la regola UDP.
   5. Immetti una regola NAT in ingresso per la connessione diretta a Contoso-WebGw1
       1. Seleziona **Regole NAT in ingresso** e quindi **Aggiungi**.
       2. Immetti un valore in **Nome** (ad esempio RDP-Contoso-WebGw1), seleziona **Personalizzato** per il servizio e **TCP** per il protocollo e specifica 14000 per **Porta**.
       3. Seleziona **Scegliere una macchina virtuale** e quindi Contoso-WebGw1.
       4. Seleziona **Personalizzato** per il mapping delle porte, specifica 3389 per **Porta di destinazione** e seleziona **OK**.
5. Immetti un nome DNS/URL esterno per la distribuzione per consentire l'accesso dall'esterno:  
   1.  Nel portale di Azure fai clic su **Sfoglia > Gruppi di risorse**, fai clic sul gruppo di risorse per la distribuzione e quindi fai clic sull'indirizzo IP pubblico creato per Accesso Web Desktop remoto e Gateway Desktop remoto.  
   2.  Fai clic su **Configurazione**, immetti un'etichetta di nome DNS (ad esempio contoso) e quindi fai clic su **Salva**. Questa etichetta di nome DNS (contoso.westus.cloudapp.azure.com) è il nome DNS che userai per la connessione al server Gateway Desktop remoto e Accesso Web Desktop remoto.  

