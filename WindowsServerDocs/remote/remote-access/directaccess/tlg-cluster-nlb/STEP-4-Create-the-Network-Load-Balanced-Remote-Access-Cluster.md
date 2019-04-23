---
title: PASSAGGIO 4 creare il Cluster di accesso remoto con bilanciamento carico di rete
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11f4fe7b68f69b00ec0f8fb9764e0fb4460a2851
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845952"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>PASSAGGIO 4 creare il Cluster di accesso remoto con bilanciamento carico di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

 Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 consentono di creare cluster di server di accesso remoto. Un cluster agisce come un singolo server logico e fornisce la configurazione centralizzata e la gestione per i server del cluster. Quando si usa il bilanciamento del carico di rete (NLB) è supportata per un massimo di 8 membri di accesso remoto in un singolo cluster. I cluster di accesso remoti garantiscono un'elevata disponibilità e bilanciamento del carico delle connessioni dai client DirectAccess alla rete interna.  
  
Le procedure seguenti consentono di creare e testare un cluster di accesso remoto:  
  
1. Installare la funzionalità Bilanciamento carico di rete in EDGE1 ed EDGE2. Prima di abilitare il bilanciamento del carico, è necessario installare la funzionalità Bilanciamento carico di rete su sia EDGE1 ed EDGE2.
  
2. Abilitare il bilanciamento del carico su EDGE1. EDGE1 è stato originariamente installato in modalità server singolo. Per abilitare il bilanciamento del carico, si configura nuovo interni ed esterni indirizzi IP dedicati (DIP) per EDGE1. Il DIP precedente su EDGE1 vengono configurati automaticamente come indirizzi IP virtuali (VIP) per il cluster. Il nuovo DIP esterno è 131.107.0.10, il nuovo DIP interno di IPv4 è 10.0.0.10, il nuovo DIP IPv6 interno è 2001:db8:1::10. Gli indirizzi VIP del cluster sono 131.107.0.2 e 131.107.0.3 (esterno) e 10.0.0.2 e 2001:db8:1::2 (interno).
  
3. Aggiungere EDGE2 al cluster con bilanciamento di carico. Dopo aver abilitato il bilanciamento del carico, è ora possibile aggiungere EDGE2 al cluster per fornire disponibilità elevata e bilanciamento del carico per le connessioni client DirectAccess.

## <a name="prerequisites"></a>Prerequisiti

Se si crea questo lab di test in macchine virtuali, è necessario abilitare lo spoofing sul EDGE1 ed EDGE2 dell'indirizzo MAC.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Abilitare lo spoofing degli indirizzi in EDGE1 ed EDGE2 dell'indirizzo MAC  
  
1.  Eseguire una chiusura normale su EDGE1 ed EDGE2.  
  
2.  Il computer che ospita le macchine virtuali, in **gestione di Hyper-V**, fare doppio clic su EDGE1 e quindi fare clic su **impostazioni**.  
  
3.  Nel **le impostazioni** nella finestra di dialogo il **Hardware** elencare, selezionare la scheda di rete connessa alla rete corpnet e quindi nel riquadro dei dettagli selezionare il **Abilita spoofing indirizzi MAC**  casella di controllo.  
  
4.  Nel **Hardware** elencare, selezionare la scheda di rete connessa alla rete Internet e quindi nel riquadro dei dettagli, selezionare la **Abilita spoofing indirizzi MAC** casella di controllo.  
  
5.  Nel **le impostazioni** finestra di dialogo, fare clic su **OK**.  
  
6.  Ripetere questa procedura dal passaggio 2 in EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Installare la funzionalità Bilanciamento carico di rete in EDGE1 ed EDGE2  
Per configurare EDGE1 ed EDGE2 in un cluster, è necessario installare la funzionalità Bilanciamento carico di rete su sia EDGE1 ed EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Per installare Bilanciamento carico di rete  
  
1.  In EDGE1, nella console di Server Manager nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** quattro volte per visualizzare la schermata di selezione delle funzionalità server.  
  
3.  Nel **Selezionare le funzionalità** finestra di dialogo, selezionare **Bilanciamento carico di rete**, fare clic su **Aggiungi funzionalità**, fare clic su **Avanti**, quindi fare clic su **installare**.  
  
4.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  
5.  Ripetere questa procedura su EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Abilitare il bilanciamento del carico su EDGE1  
Utilizzare questa procedura per abilitare il bilanciamento del carico e configurare il nuovo DIP in EDGE1.  
  
### <a name="enable-load-balancing"></a>Abilitare il bilanciamento del carico  
  
1.  Nella EDGE1, fare clic su **avviare**, digitare **RAMgmtUI.exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione accesso remoto, nel riquadro a sinistra, fare clic su **configurazione**, e quindi la **attività** riquadro, fare clic su **abilitare il bilanciamento del carico**.  
  
3.  Di abilitare carico bilanciamento guidata fare clic su **Avanti**.  
  
4.  Nel **metodo di bilanciamento del carico** pagina, fare clic su **usare Windows carico bilanciamento rete (NLB)** e quindi fare clic su **Next**.  
  
5.  Nel **dedicato gli indirizzi IP esterni** nella pagina il **indirizzo IPv4** , digitare **131.107.0.10**, nel **Subnet mask** verificare il prefisso di subnet **255.255.255.0**, quindi fare clic su **successivo**.  
  
6.  Nel **dedicato gli indirizzi IP interni** pagina, effettuare le operazioni seguenti e quindi fare clic su **successivo**:  
  
    1.  Nel **indirizzo IPv4** , digitare **10.0.0.10** e nel **Subnet mask** casella, verificare che sia il prefisso della subnet **255.255.255.0**.  
  
    2.  Nel **indirizzo IPv6** , digitare **2001:db8:1::10** e nella lunghezza del prefisso di Subnet, verificare che sia il valore **64**.  
  
7.  Nel **riepilogo** pagina, fare clic su **Commit**.  
  
8.  Nel **abilitare il bilanciamento del carico** la finestra di dialogo, fare clic su **Chiudi**.  
  
9. Di abilitare carico bilanciamento guidata fare clic su **Chiudi**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Aggiungere EDGE2 al cluster con bilanciamento di carico  
Utilizzare questa procedura per aggiungere EDGE2 il cluster di bilanciamento carico di rete.  
  
> [!NOTE]  
> È necessario attendere due minuti dopo aver completato i passaggi precedenti prima di procedere. Dopo l'abilitazione di bilanciamento carico di rete, il RAConfigTask viene eseguito e configura il computer con le impostazioni di bilanciamento carico di rete. L'operazione potrebbe richiedere alcuni minuti per completare e se l'amministratore esegue un'altra configurazione correlata a bilanciamento carico di rete prima della scadenza di attività, tale configurazione avrà esito negativo.  
  
### <a name="add-edge2-to-the-cluster"></a>Aggiungere EDGE2 al cluster  
  
1.  Nel computer EDGE1 o macchina virtuale, nella Console di gestione accesso remoto, nel **attività** riquadro, di sotto **Cluster con bilanciamento del carico**, fare clic su **aggiungere o rimuovere server**.  
  
2.  Nel **aggiungere o rimuovere server** la finestra di dialogo, fare clic su **Aggiungi Server**.  
  
3.  Nel **aggiungere un Server** procedura guidata per il **selezione Server** , digitare **EDGE2**e quindi fare clic su **successivo**.  
  
4.  Nel **schede di rete** nella pagina **adapter esterno**, verificare che **Internet** è selezionata e nella **scheda interna**, assicurarsi che che **Corpnet** sia selezionata. Fare clic su **esplorare**via le **sicurezza di Windows** finestra di dialogo, assicurarsi che **certificato IP-HTTPS** è selezionata, fare clic su **OK**e quindi fare clic su **Successivo**.  
  
5.  Nel **riepilogo** pagina, fare clic su **Aggiungi**.  
  
6.  Nel **completamento** pagina, fare clic su **Chiudi**.  
  
7.  Nel **aggiungere o rimuovere server** la finestra di dialogo, fare clic su **Commit**.  
  
8.  Nel **aggiunta e rimozione di server** la finestra di dialogo, fare clic su **Chiudi**.  
  
9. Nel **avviare** digitare**nlbmgr.exe**e quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
10. Nel **gestione del bilanciamento del carico di rete**, fare clic su **interna DA cluster**. Nel riquadro dei dettagli, verificare che entrambe **EDGE1(Corpnet)** e **EDGE2(Corpnet)** con lo stato **convergente**.  
  
11. Se non è un server **convergente**, nell'albero della console, fare doppio clic su server, scegliere **controllo Host**, quindi fare clic su **avviare**.  
  
12. Nel **gestione del bilanciamento del carico di rete**, fare clic su **cluster DA Internet**. Assicurarsi che nel riquadro dei dettagli, entrambe **EDGE1(Internet)** e **EDGE2(Internet)** con lo stato **convergente**.  
  
13. Se non è un server **convergente**, nell'albero della console, fare doppio clic su server, scegliere **controllo Host**, quindi fare clic su **avviare**.
