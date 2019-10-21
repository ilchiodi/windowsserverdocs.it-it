---
title: Creare un nuovo gruppo NIC in un computer host o una macchina virtuale
description: In questo argomento viene creato un nuovo gruppo NIC in un computer host o in una macchina virtuale (VM) Hyper-V che esegue Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 1785b34741ce525a5bdd27b77a0e52fc2ca6c1b6
ms.sourcegitcommit: 9a6a692a7b2a93f52bb9e2de549753e81d758d28
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2019
ms.locfileid: "72591108"
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Creare un nuovo gruppo NIC in un computer host o una macchina virtuale

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene creato un nuovo gruppo NIC in un computer host o in una macchina virtuale (VM) Hyper-V che esegue Windows Server 2016.  

## <a name="network-configuration-requirements"></a>Requisiti di configurazione di rete  
Prima di poter creare un nuovo gruppo NIC, è necessario distribuire un host Hyper-V con due schede di rete che si connettono a commutatori fisici diversi. È inoltre necessario configurare le schede di rete con indirizzi IP provenienti dallo stesso intervallo di indirizzi IP.  

Il Commuter fisico, il commutire virtuale Hyper-V, la rete locale (LAN) e i requisiti per il gruppo NIC per la creazione di un gruppo NIC in una macchina virtuale sono:  

-   Il computer che esegue Hyper-V deve disporre di due o più schede di rete.  

-   Se si connettono le schede di rete a più commutatori fisici, i commutatori fisici devono trovarsi nella stessa subnet di livello 2.  

-   È necessario utilizzare la console di gestione di Hyper-V o Windows PowerShell per creare due commutatori virtuali Hyper-V esterni, ciascuno connesso a una scheda di rete fisica diversa.  

-   La macchina virtuale deve connettersi a entrambi i commutatori virtuali esterni creati.  

-   Gruppo NIC, in Windows Server 2016, supporta i team con due membri nelle VM. È possibile creare team di grandi dimensioni, ma non è disponibile alcun supporto.

-   Se si sta configurando un gruppo NIC in una macchina virtuale, è necessario selezionare una **modalità gruppo** di _commutazione indipendente_ e una **modalità di bilanciamento del carico** per _hash Indirizzo_. 

## <a name="step-1-configure-the-physical-and-virtual-network"></a>Passaggio 1. Configurare la rete fisica e virtuale  
In questa procedura vengono creati due commutatori virtuali Hyper-V esterni, si connette una macchina virtuale ai commutatori e quindi si configurano le connessioni della VM ai commutatori.  

### <a name="prerequisites"></a>Prerequisiti

È necessario appartenere al gruppo **Administrators**oppure a un gruppo equivalente.  

### <a name="procedure"></a>Procedura

1.  Nell'host Hyper-V aprire Console di gestione di Hyper-V, quindi in azioni fare clic su **gestione Commutiri virtuali**.  

   ![Gestione commutiri virtuali](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv.jpg)  

2.  In gestione commutiri virtuali assicurarsi che sia selezionata l'opzione **esterno** , quindi fare clic su **Crea commute virtuale**.  

   ![Crea Commuter virtuale](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hv_02.jpg)  

3.  In proprietà Commuter virtuale digitare un **nome** per il Commuter virtuale e aggiungere **note** in base alle esigenze.  

4.  In **tipo di connessione**, in **rete esterna**, selezionare la scheda di rete fisica a cui si desidera collegare il Commuter virtuale.  

5.  Configurare le proprietà aggiuntive per la distribuzione e quindi fare clic su **OK**.  

6.  Creare un secondo commutire virtuale esterno ripetendo i passaggi precedenti. Connettere il secondo switch esterno a una scheda di rete diversa.  

7.  Nella console di gestione di Hyper-V, in **macchine virtuali**, fare clic con il pulsante destro del mouse sulla macchina virtuale che si desidera configurare, quindi scegliere **Impostazioni**.  

   Verrà visualizzata la finestra di dialogo **Impostazioni** macchina virtuale.

8.  Assicurarsi che la macchina virtuale non sia stata avviata. Se viene avviato, eseguire un arresto prima di configurare la macchina virtuale.  

8.  In **hardware**fare clic su **scheda di rete**.  

   ![Scheda di rete](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_01.jpg)  

9. In proprietà **scheda di rete** selezionare uno dei commutatori virtuali creati nei passaggi precedenti e quindi fare clic su **applica**.  

    ![Selezionare un commutire virtuale](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_02.jpg)  

10. In **hardware**fare clic per espandere il segno più (+) accanto alla **scheda di rete**. 

11. Fare clic su **funzionalità avanzate** per abilitare il gruppo NIC utilizzando l'interfaccia utente grafica. 

    >[!TIP]
    >È anche possibile abilitare il gruppo NIC con un comando di Windows PowerShell: 
    >
    >```PowerShell
    >Set-VMNetworkAdapter -VMName <VMname> -AllowTeaming On
    >```

    a. Selezionare **dinamico** per indirizzo Mac. 

    b. Fare clic per selezionare la **rete protetta**. 

    c. Fare clic per selezionare **Consenti alla scheda di rete di far parte di un team nel sistema operativo guest**. 

    d. Fai clic su **OK**.  

    ![Aggiungere una scheda di rete a un team](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_05.jpg)  

13. Per aggiungere una seconda scheda di rete, nella console di gestione di Hyper-V, in **macchine virtuali**, fare clic con il pulsante destro del mouse sulla stessa macchina virtuale, quindi scegliere **Impostazioni**.  

    Verrà visualizzata la finestra di dialogo **Impostazioni** macchina virtuale.

14. In **Aggiungi hardware**fare clic su **scheda di rete**e quindi su **Aggiungi**.  

   ![Aggiungere una scheda di rete](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_06.jpg)  

15. In proprietà **scheda di rete** selezionare il secondo commutire virtuale creato nei passaggi precedenti e quindi fare clic su **applica**.  

   ![Applicare un commutire virtuale](../../media/Create-a-New-NIC-Team-in-a-VM/nict_hvs_07.jpg)  

16. In **hardware**fare clic per espandere il segno più (+) accanto alla **scheda di rete**. 

17. Fare clic su **funzionalità avanzate**, scorrere fino a **Gruppo NIC**e fare clic per selezionare **Abilita questa scheda di rete come parte di un team nel sistema operativo guest**. 

18. Fai clic su **OK**.  

_**Congratulazioni!**_  È stata configurata la rete fisica e virtuale.  A questo punto è possibile procedere alla creazione di un nuovo gruppo NIC.  

## <a name="step-2-create-a-new-nic-team"></a>Passaggio 2. Creare un nuovo gruppo NIC

Quando si crea un nuovo gruppo NIC, è necessario configurare le proprietà del gruppo NIC:  

-   Nome del team  

-   Adattatori membro  

-   Modalità gruppo  

-   Modalità di bilanciamento del carico  

-   Scheda standby  

È anche possibile configurare facoltativamente l'interfaccia del team principale e configurare un numero di LAN virtuale (VLAN).  

Per ulteriori informazioni su queste impostazioni, vedere [NIC gruppo impostazioni](nic-teaming-settings.md).

### <a name="prerequisites"></a>Prerequisiti

È necessario appartenere al gruppo **Administrators**oppure a un gruppo equivalente.  

### <a name="procedure"></a>Procedura

1. In Server Manager fare clic su **Server locale**.  

2. Nel riquadro **Proprietà** , nella prima colonna, individuare **Gruppo NIC**, quindi fare clic sul collegamento **disabilitato** .  

   Verrà visualizzata la finestra di dialogo **Gruppo NIC** .

   ![Finestra di dialogo Gruppo NIC](../../media/Create-a-New-NIC-Team/nict_02_nicteaming.jpg)

3. In **Adapter e interfacce**selezionare una o più schede di rete che si desidera aggiungere a un gruppo NIC.  

4. Fare clic su **attività**e quindi su **Aggiungi a nuovo team**.  

   Verrà visualizzata la finestra di dialogo **nuovo team** con le schede di rete e i membri del team.

5. In **nome Team**Digitare un nome per il nuovo gruppo NIC e quindi fare clic su **proprietà aggiuntive**.  

6. In **proprietà aggiuntive**selezionare i valori per:

   - **Modalità gruppo**. Le opzioni per la modalità gruppo sono **indipendenti dall'opzione** e **variano a seconda**del parametro. La modalità dipendente dal Commuter include il **gruppo statico** e il **protocollo LACP (Link Aggregation Control Protocol)** . 

     - **Cambia indipendente.** [!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)]

     - **Dipendente dall'opzione.** [!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)] 


       |                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
       |----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
       |              **Gruppo statico**              |                                                                                                                                              Per identificare i collegamenti che formano il team, è necessario configurare manualmente sia l'opzione che l'host. Poiché si tratta di una soluzione configurata in modo statico, non esiste alcun protocollo aggiuntivo per assistere il Commuter e l'host per identificare i cavi collegati in modo errato o altri errori che potrebbero causare un errore del team. Questa modalità è in genere supportata dai commutatori di classe server.                                                                                                                                              |
       | **Protocollo LACP (Link Aggregation Control Protocol)** | A differenza del gruppo statico, la modalità gruppo LACP identifica in modo dinamico i collegamenti connessi tra l'host e il Commuter. Questa connessione dinamica consente la creazione automatica di un team e, in teoria, ma raramente in pratica, l'espansione e la riduzione di un team semplicemente tramite la trasmissione o la ricezione di pacchetti LACP dall'entità peer. Tutte le opzioni di classe server supportano LACP e richiedono che l'operatore di rete abiliti in modo amministrativo LACP sulla porta del commutatore. Quando si configura una modalità gruppo di LACP, gruppo NIC opera sempre in modalità attiva di LACP.  Per impostazione predefinita, gruppo NIC utilizza un breve timer (3 secondi), ma è possibile configurare un timer lungo (90 secondi) con `Set-NetLbfoTeam`. |

       ---

   - **Modalità di bilanciamento del carico**. Le opzioni per la modalità di distribuzione del bilanciamento del carico sono **hash indirizzo**, **porta Hyper-V**e **dinamica**.

     - **Hash indirizzo.** [!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]

     - **Porta Hyper-V.** [!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]

     - **Dinamico.** [!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]

   - **Scheda standby**. Le opzioni per la scheda standby sono **None (tutti gli adapter attivi)** o la selezione di una scheda di rete specifica nel gruppo NIC che funge da scheda standby.

   > [!TIP]  
   > Se si sta configurando un gruppo NIC in una macchina virtuale (VM), è necessario selezionare una **modalità gruppo** di _commutazione indipendente_ e una **modalità di bilanciamento del carico** per _hash Indirizzo_.  

7. Se si desidera configurare il nome dell'interfaccia del team primario o assegnare un numero VLAN al gruppo NIC, fare clic sul collegamento a destra di **interfaccia del team primario**.  

    Verrà visualizzata la finestra di dialogo **nuova interfaccia del team** .

    ![Nuova interfaccia del team](../../media/Create-a-New-NIC-Team/nict_newteaminterface.jpg)

8. A seconda dei requisiti, effettuare una delle operazioni seguenti:  

   -   Specificare un nome di interfaccia tNIC.  

   -   Configurare l'appartenenza VLAN: fare clic su **VLAN specifica** e digitare le informazioni VLAN. Ad esempio, se si desidera aggiungere il gruppo NIC al numero VLAN contabilità 44, digitare contabilità 44-VLAN.   

9. Fai clic su **OK**.  

_**Congratulazioni!**_  È stato creato un nuovo gruppo NIC in un computer host o una macchina virtuale.

## <a name="related-topics"></a>Argomenti correlati

- [Gruppo NIC](NIC-Teaming.md): in questo argomento viene illustrata una panoramica del gruppo NIC (Network Interface Card) in Windows Server 2016. Gruppo NIC consente di raggruppare una o più schede di rete fisiche Ethernet da una a 32 in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.   

- [Gestione e utilizzo degli indirizzi MAC del gruppo NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): quando si configura un gruppo NIC con modalità indipendente dal Commuter e la distribuzione del carico dinamico o degli indirizzi hash, il team USA l'indirizzo Media Access Control (Mac) del membro del gruppo NIC primario in uscita traffico. Il membro del gruppo NIC primario è una scheda di rete selezionata dal sistema operativo dal set iniziale di membri del team.

- [Impostazioni gruppo NIC](nic-teaming-settings.md): in questo argomento viene illustrata una panoramica delle proprietà del gruppo NIC, ad esempio le modalità gruppo e bilanciamento del carico. Vengono inoltre illustrati i dettagli relativi all'impostazione della scheda standby e alla proprietà principale dell'interfaccia del team. Se si dispone di almeno due schede di rete in un gruppo NIC, non è necessario designare una scheda standby per la tolleranza di errore.

- [Risoluzione dei problemi relativi al gruppo NIC](Troubleshooting-NIC-Teaming.md): in questo argomento vengono illustrati i modi per risolvere i problemi relativi al gruppo NIC, ad esempio l'hardware, i titoli del commutatore fisico e la disabilitazione o l'abilitazione di schede di rete con Windows PowerShell 

---
