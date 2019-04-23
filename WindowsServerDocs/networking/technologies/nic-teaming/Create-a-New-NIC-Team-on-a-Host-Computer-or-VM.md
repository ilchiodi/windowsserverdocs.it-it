---
title: Creare un nuovo gruppo NIC in una VM o computer host
description: In questo argomento, creare un nuovo gruppo NIC in un computer host o in una macchina virtuale di Hyper-V (VM) che esegue Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 3518436f08a0e7fe0ea8fed06724b11df6acccea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864632"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Creare un nuovo gruppo NIC in una VM o computer host

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento, creare un nuovo gruppo NIC in un computer host o in una macchina virtuale di Hyper-V (VM) che esegue Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Requisiti di configurazione di rete  
Prima di poter creare un nuovo gruppo NIC, è necessario distribuire un host Hyper-V con due schede di rete che si connettono a diversi commutatori fisici. È anche necessario configurare le schede di rete con indirizzi IP dallo stesso intervallo di indirizzi IP.  

Il commutatore fisico, il commutatore virtuale Hyper-V, rete locale (LAN) e i requisiti di gruppo NIC per la creazione di un gruppo NIC in una macchina virtuale sono:  
  
-   Il computer che esegue Hyper-V deve avere due o più schede di rete.  
  
-   Se ci si connette le schede di rete a più commutatori fisici, i commutatori fisici devono essere nella stessa subnet di livello 2.  
  
-   È necessario utilizzare Gestione Hyper-V o Windows PowerShell per creare due commutatori virtuali esterni di Hyper-V, ciascuna connessa a una scheda di rete fisiche diverse.  
  
-   La macchina virtuale deve connettersi a entrambe commutatori virtuali esterni che si crea.  
  
-   Gruppo NIC, in Windows Server 2016 supporta i team con due membri nelle macchine virtuali. È possibile creare team più grandi, ma non è supportata.
  
-   Se si configura un gruppo NIC in una macchina virtuale, è necessario selezionare una **modalità gruppo** dei _commutatore indipendente_ e un **modalità di bilanciamento del carico** di _Hash indirizzo_. 



## <a name="step-1-configure-the-physical-and-virtual-network"></a>Passaggio 1. Configurare la rete fisica e virtuale  
In questa procedura, creare due commutatori virtuali esterni di Hyper-V, la connessione di una macchina virtuale per i commutatori e quindi configurare le connessioni della macchina virtuale per i commutatori.  
 

### <a name="prerequisites"></a>Prerequisiti
  
È necessario appartenere **gli amministratori**, o equivalente.  
  
### <a name="procedure"></a>Routine
  
1.  Nell'host Hyper-V, aprire Gestione Hyper-V e nel riquadro azioni, fare clic su **gestione commutatori virtuali**.  
  
    ![Gestione commutatori virtuali](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  
  
2.  In Gestione commutatori virtuali, assicurarsi che **esterne** sia selezionata e quindi fare clic su **crea commutatore virtuale**.  
  
    ![Crea commutatore virtuale](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  
  
3.  Nella proprietà commutatore virtuale, digitare un **Name** virtuale passare e aggiungere **note** in base alle esigenze.  
  
4.  Nelle **tipo di connessione**, nel **rete esterna**, selezionare la scheda di rete fisica a cui si desidera collegare il commutatore virtuale.  
  
5.  Configurare le proprietà del commutatore aggiuntive per la distribuzione e quindi fare clic su **OK**.  
  
6.  Creare un commutatore virtuale esterno secondo ripetendo i passaggi precedenti. Connettere il secondo commutatore esterno a una scheda di rete diverso.  
  
7.  Nella console di gestione Hyper-V in **macchine virtuali**, fare doppio clic su macchina virtuale che si desidera configurare e quindi fare clic su **impostazioni**.<p>La macchina virtuale **impostazioni** verrà visualizzata la finestra di dialogo.  

8.  Assicurarsi che la macchina virtuale non è stata avviata. Se è stato avviato, eseguire una chiusura prima di configurare la macchina virtuale.  
  
8.  Nelle **Hardware**, fare clic su **scheda di rete**.  
  
    ![Scheda di rete](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  
  
9. Nelle **scheda di rete** delle proprietà, selezionare uno dei commutatori virtuali che è stato creato nei passaggi precedenti e quindi fare clic su **applica**.  
  
    ![Selezionare un commutatore virtuale](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  
  
10. Nelle **Hardware**, fare clic per espandere il segno più (+) accanto a **scheda di rete**. 

11. Fare clic su **Advanced Features** a Abilita gruppo NIC usando l'interfaccia utente grafica. 

    >[!TIP]
    >È anche possibile abilitare il gruppo NIC con un comando Windows PowerShell: 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```
   
    a. Selezionare **dinamica** per l'indirizzo MAC. 

    b. Fare clic per selezionare **rete protetta**. 

    c. Fare clic per selezionare **abilita questa scheda di rete da parte di un gruppo nel sistema operativo guest**. 

    d. Fare clic su **OK**.  
  
    ![Aggiungere una scheda di rete a un team](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  
  
13. Per aggiungere una seconda scheda di rete, nella console di gestione Hyper-V **macchine virtuali**, fare doppio clic su nella stessa macchina virtuale e quindi fare clic su **impostazioni**.<p>La macchina virtuale **impostazioni** verrà visualizzata la finestra di dialogo.  
  
14. Nelle **Aggiungi Hardware**, fare clic su **scheda di rete**, quindi fare clic su **Aggiungi**.  
  
    ![Aggiungere una scheda di rete](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  
  
15. Nelle **scheda di rete** delle proprietà, selezionare il commutatore virtuale secondo creato nei passaggi precedenti e quindi fare clic su **applica**.  
  
    ![Applicare un commutatore virtuale](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  
  
16. Nelle **Hardware**, fare clic per espandere il segno più (+) accanto a **scheda di rete**. 

17. Fare clic su **Advanced Features**, scorrere verso il basso **NIC Teaming**e fare clic per selezionare **abilitare la scheda di rete da parte di un gruppo nel sistema operativo guest**. 

18. Fare clic su **OK**.  
  
_**Congratulazioni!**_  È stata configurata la rete fisica e virtuale.  A questo punto è possibile procedere alla creazione di un nuovo gruppo NIC.  

## <a name="step-2-create-a-new-nic-team"></a>Passaggio 2. Creare un nuovo gruppo NIC
  
Quando si crea un nuovo gruppo NIC, è necessario configurare le proprietà di gruppo NIC:  
  
-   Nome del team  
  
-   Schede membro  
  
-   Modalità gruppo  
  
-   Modalità di bilanciamento del carico  
  
-   Scheda in standby  
  
È anche possibile configurare l'interfaccia gruppo primaria e configurare un numero di LAN (VLAN) virtuali.  
  
Per altre informazioni su queste impostazioni, vedere [le impostazioni di gruppo NIC](nic-teaming-settings.md).

### <a name="prerequisites"></a>Prerequisiti

È necessario appartenere **gli amministratori**, o equivalente.  
  
### <a name="procedure"></a>Routine
  
1.  In Server Manager fare clic su **Server locale**.  
  
2.  Nel **delle proprietà** riquadro, nella prima colonna, individuare **NIC Teaming**e quindi fare clic sui **disabilitato** collegamento.<p>Il **NIC Teaming** verrà visualizzata la finestra di dialogo.<p>![Finestra di dialogo gruppo NIC](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)  
  
3.  Nelle **schede e le interfacce**, selezionare le schede di rete di uno o più che si desidera aggiungere a un gruppo NIC.  
  
4.  Fare clic su **attività**, fare clic su **aggiungere al nuovo Team**.<p>Il **nuovo team** nella finestra di dialogo apre e visualizza le schede di rete e i membri del team.  
  
5.  Nelle **nome del Team**, digitare un nome per il nuovo gruppo NIC e quindi fare clic su **proprietà aggiuntive**.  
  
6.  Nelle **proprietà aggiuntive**, selezionare i valori per:

    - **Modalità gruppo**. Le opzioni per la modalità gruppo vengono **commutatore indipendente** e **commutatore dipendenti**. Include la modalità dipendenti commutatore **Creazione gruppi statica** e **protocollo LACP (Link Aggregation controllo Protocol)**. 
      
      - **Commutatore indipendente.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]
      
      - **Dipendenti dal commutatore.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 
      
        | | |
        |---|---|
        |**Creazione gruppi statica** | È necessario configurare manualmente il commutatore sia l'host per identificare quali collegamenti formano il gruppo. Poiché si tratta di una soluzione configurata staticamente, non vi è nessun protocollo aggiuntivo per facilitare l'opzione e l'host per identificare in modo non corretto collegato cavi o altri errori che potrebbero compromettere il team a non riuscire a eseguire. Questa modalità è in genere supportata dai commutatori di classe server. |
        |**Link Aggregation Control Protocol (LACP)** |A differenza di creazione gruppi statica, la modalità di raggruppamento LACP identifica in modo dinamico i collegamenti connessi tra l'host e il commutatore. Questa connessione dinamica consente la creazione automatica di un team e, in teoria, ma raramente in pratica, l'espansione e riduzione di un gruppo semplicemente mediante la trasmissione o ricezione dei pacchetti di LACP dall'entità peer. Tutti i commutatori di classe server supportano LACP tutti richiedono l'operatore di rete da abilitare a livello amministrativo LACP sulla porta di commutazione. Quando si configura una modalità gruppo di LACP, NIC Teaming viene sempre eseguito in modalità attiva del LACP con un timer breve.  Opzione non è attualmente disponibile per la modifica del timer o modificare la modalità di LACP. |
        ---
    
    - **Modalità di bilanciamento del carico**. Le opzioni per la modalità di distribuzione del bilanciamento del carico sono **Hash indirizzo**, **porta Hyper-V**, e **dinamica**.
    
       - **Hash indirizzo.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
       
       - **Porta Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]
       
       - **Dynamic.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
    
    - **Scheda in standby**. Le opzioni per Adapter di Standby **Nessuno (tutte le schede attive)** o la selezione di una scheda di rete specifici nel gruppo NIC che agisce come una scheda di Standby.
   
   > [!TIP]  
   > Se si configura un gruppo NIC in una macchina virtuale (VM), è necessario selezionare una **modalità gruppo** dei _commutatore indipendente_ e un **modalità di bilanciamento del carico** di  _Hash indirizzo_.  
  
7.  Se si desidera configurare il nome dell'interfaccia gruppo primaria o assegnare un numero di VLAN per il gruppo NIC, fare clic sul collegamento a destra della **interfaccia gruppo primaria**.<p>Il **nuova interfaccia team** verrà visualizzata la finestra di dialogo.<p>![Nuova interfaccia del team](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)  
  
8.  A seconda delle esigenze, eseguire una delle operazioni seguenti:  
  
    -   Specificare un nome di interfaccia tNIC.  
  
    -   Configurare l'appartenenza VLAN: fare clic su **determinata VLAN** e digitare le informazioni VLAN. Ad esempio, se si desidera aggiungere il gruppo NIC per la contabilità VLAN numero 44, 44 Accounting tipo - VLAN.   
  
9. Fare clic su **OK**.  

_**Congratulazioni!**_  È stato creato un nuovo gruppo NIC in una VM o computer host.

## <a name="related-topics"></a>Argomenti correlati
- [Gruppo NIC](NIC-Teaming.md): In questo argomento viene fornita è una panoramica del gruppo di schede di interfaccia di rete (NIC) in Windows Server 2016. Gruppo NIC consente di raggruppare tra 1 e 32 schede di rete Ethernet fisiche in una o più schede di rete virtuale basata su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.   

- [Gestione e uso di indirizzi MAC di NIC Teaming](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando si configura un gruppo NIC con hash indirizzo o la distribuzione del carico dinamico e Cambia modalità indipendente, l'accesso ai supporti il team utilizza controllo indirizzo (MAC) del membro del Team di interfaccia di rete primario sul traffico in uscita. Membro del Team di interfaccia di rete primario è una scheda di rete selezionata per il sistema operativo del set iniziale di membri del team.

- [Le impostazioni di gruppo NIC](nic-teaming-settings.md): In questo argomento è fornire una panoramica delle proprietà del Team di interfaccia di rete, ad esempio gruppo NIC e modalità di bilanciamento del carico. È inoltre offrono informazioni dettagliate sull'impostazione della scheda di Standby e la proprietà dell'interfaccia gruppo primaria. Se si dispone di almeno due schede di rete in un gruppo NIC, non devi designare una scheda di Standby per la tolleranza di errore.

- [Risoluzione dei problemi di gruppo NIC](Troubleshooting-NIC-Teaming.md): In questo argomento vengono illustrati modi per risolvere i problemi gruppo NIC, ad esempio hardware, titoli commutatore fisico e l'abilitazione o disabilitazione schede di rete tramite Windows PowerShell. 

---