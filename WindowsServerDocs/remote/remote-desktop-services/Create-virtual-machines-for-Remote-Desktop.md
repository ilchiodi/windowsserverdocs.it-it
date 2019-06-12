---
title: Creare macchine virtuali per Desktop remoto
description: Creare macchine virtuali per ospitare i componenti di Desktop remoto nel cloud.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5c61d9f08cb799d6a63a004bedab924a6ba37fdc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446735"
---
# <a name="create-virtual-machines-for-remote-desktop"></a>Creare macchine virtuali per Desktop remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016

Usare la procedura seguente per creare le macchine virtuali nell'ambiente del tenant che verrà utilizzato per eseguire i ruoli di Windows Server 2016, servizi e le funzionalità necessarie per un distribuzione dell'hosting del desktop.   
  
Per questo esempio di una distribuzione di base, verrà creato il valore minimo di 3 macchine virtuali. Una macchina virtuale verranno ospitati i servizi ruolo Gestore connessione Desktop remoto (RD) e Server licenze e una condivisione file per la distribuzione. Una seconda macchina virtuale verranno ospitati i servizi ruolo Gateway Desktop remoto e accesso Web.  Una terza macchina virtuale host del servizio ruolo Host sessione Desktop remoto. Per distribuzioni di piccole dimensioni, è possibile ridurre i costi della macchina virtuale tramite il Proxy applicazione AAD per eliminare tutti gli endpoint pubblici dalla distribuzione e la combinazione di tutti i servizi ruolo in una singola macchina virtuale. Per distribuzioni più grandi, è possibile installare i vari servizi ruolo nelle singole macchine virtuali per consentire una migliore scalabilità.  
  
Questa sezione vengono descritti i passaggi necessari per distribuire le macchine virtuali per ogni ruolo basate su immagini di Windows Server nel [Microsoft Azure Marketplace](https://azure.microsoft.com/marketplace/). Se è necessario creare le macchine virtuali da un'immagine personalizzata, che richiede PowerShell, consultare [creare una VM Windows con Resource Manager e PowerShell](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-ps-create/). Tornare quindi qui per collegare dischi dati di Azure per la condivisione file e immettere un URL esterno per la distribuzione.  
  
1. [Creare macchine virtuali Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/) per ospitare il server Gestore connessione desktop remoto, Server licenze Desktop remoto e File.  
  
   Per questa esercitazione sono state usate le convenzioni di denominazione seguente:  
   - Gestore connessione desktop remoto, Server licenze e File Server:   
       - VM: Contoso-Cb1  
       - Set di disponibilità: CbAvSet    
   - Accesso Web desktop remoto e Server Gateway Desktop remoto:   
       - VM: Contoso-WebGw1  
       - Set di disponibilità: WebGwAvSet  
          
   - Host sessione Desktop remoto:   
       - VM: Contoso-Sh1  
       - Set di disponibilità: ShAvSet  
          
   Ogni macchina virtuale utilizza lo stesso gruppo di risorse.  
2. Creare e collegare un disco dati di Azure per la condivisione di disco profili utente:  
   1.  Nel portale Azure fare clic **Sfoglia > gruppi di risorse**, fare clic sul gruppo di risorse per la distribuzione e quindi fare clic sulla macchina virtuale creata per il Gestore connessione desktop remoto (ad esempio, Contoso-Cb1).  
   2.  Fare clic su **Impostazioni > dischi > Collega nuovo**.  
   3.  Accettare le impostazioni predefinite per il nome e tipo.  
   4.  Immettere una dimensione sufficientemente grande da contenere le condivisioni di rete per ambiente del tenant, inclusi i certificati e i dischi dei profili utente (in GB). È possibile simulare 5 GB per utente che si intende disporre  
   5.  Accettare le impostazioni predefinite per la posizione e memorizzazione nella cache host e quindi fare clic su **OK**.  
3. Creare un bilanciamento del carico esterno per l'accesso dall'esterno della distribuzione:
   1. Nel portale Azure fare clic **Sfoglia > bilanciamenti del carico**, quindi fare clic su **Add**.
   2. Immettere un **Name**, selezionare **pubblico** come il **tipo** del servizio di bilanciamento del carico e selezionare l'appropriata **sottoscrizione**,  **Gruppo di risorse**, e **percorso**.
   3. Selezionare **scegliere un indirizzo IP pubblico**, **Crea nuovo**, immettere un nome e selezionare **Ok**.
   4. Selezionare **Create** per creare il servizio di bilanciamento del carico.
4. Configurare il bilanciamento del carico esterno per la distribuzione
   1. Nel portale Azure fare clic **Sfoglia > gruppi di risorse**, fare clic sul gruppo di risorse per la distribuzione e quindi fare clic su servizio di bilanciamento del carico creato per la distribuzione.
   2. Aggiungere un pool di back-end di bilanciamento del carico inviare il traffico per:
       1. Selezionare **pool back-end** e **Add**.
       2. Immettere un **Name** e selezionare  **\+ Aggiungi una macchina virtuale**.
       3. Selezionare **set di disponibilità** e **WebGwAvSet**.
       4. Selezionare **macchine virtuali**, **Contoso-WebGw1**, **selezionare**, **OK**, e **OK**.
   3. Aggiungere un probe, in modo che il servizio di bilanciamento del carico possa quali computer sono attivi:
       1. Selezionare **probe** e **aggiungere**.
       2. Immettere un **Name** (ad esempio, HTTPS), selezionare **TCP**, immettere **porta** 443 e selezionare **OK**.
   4. Immettere le regole per bilanciare il traffico in ingresso di bilanciamento del carico:
      1. Selezionare **regole di bilanciamento del carico** e **Add**
      2. Immettere un **Name** (ad esempio, HTTPS), selezionare **TCP**e 443 sia per il **porta** e il **porta back-end**.
          - Per Windows 10 e distribuzione di Windows Server 2016, lasciare **persistenza della sessione** come **None**, in caso contrario, selezionare **indirizzo IP del Client**.
      3. Selezionare **OK** per accettare la regola HTTPS.
      4. Creare una nuova regola selezionando **Add**.
      5. Immettere un **Name** (ad esempio, UDP), selezionare **UDP**e 3391 sia per il <strong>porta e il * * porta back-end</strong>.
          - Per una distribuzione di Windows 10 e Windows Server 2016, lasciare **persistenza della sessione** come **None**, in caso contrario, selezionare **indirizzo IP del Client**.
      6. Selezionare **OK** per accettare la regola UDP.
   5. Immettere una regola NAT in ingresso per connettersi direttamente a Contoso-WebGw1
       1. Selezionare **regole NAT in ingresso** e **Add**.
       2. Immettere un **Name** (ad esempio RDP-Contoso-WebGw1), selezionare **Customm** per il servizio **TCP** per il protocollo e immettere 14000 per il **porta**.
       3. Selezionare **scegliere una macchina virtuale** e Contoso-WebGw1.
       4. Selezionare **Custom** per il mapping della porta, immettere 3389 per il **porta di destinazione**e selezionare **OK**.
5. Immettere un nome DNS/URL esterno per la distribuzione di accedervi dall'esterno:  
   1.  Nel portale di Azure, fare clic su **Sfoglia > gruppi di risorse**, fare clic sul gruppo di risorse per la distribuzione e quindi fare clic su indirizzo IP pubblico creato per accesso Web desktop remoto e Gateway Desktop remoto.  
   2.  Fare clic su **Configuration**, immettere un'etichetta del nome DNS (ad esempio, contoso) e quindi fare clic su **salvare**. Questa etichetta del nome DNS (contoso.westus.cloudapp.azure.com) è il nome DNS che si userà per connettersi al server Accesso Web desktop remoto e Gateway Desktop remoto.  

