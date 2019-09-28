---
title: PASSAGGIO 4 creare il cluster di accesso remoto con bilanciamento del carico di rete
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f888ebadfaa91b35f0924b23e9818da1c32f26e5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388476"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>PASSAGGIO 4 creare il cluster di accesso remoto con bilanciamento del carico di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

 Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 consentono di creare cluster di server di accesso remoto. Un cluster funge da singolo server logico e fornisce la configurazione e la gestione centralizzate per i server nel cluster. Quando si usa bilanciamento carico di rete (NLB), è disponibile il supporto per un massimo di 8 membri di accesso remoto in un singolo cluster. I cluster di accesso remoto garantiscono disponibilità elevata e bilanciamento del carico delle connessioni dai client DirectAccess alla rete interna.  
  
Le procedure seguenti consentono di creare e testare un cluster di accesso remoto:  
  
1. Installare la funzionalità Bilanciamento carico di rete in EDGE1 e EDGE2. Prima di abilitare il bilanciamento del carico, è necessario installare la funzionalità Bilanciamento carico di rete sia in EDGE1 che in EDGE2.
  
2. Abilitare il bilanciamento del carico in EDGE1. EDGE1 è stato originariamente installato in modalità server singolo. Per abilitare il bilanciamento del carico, si configurano i nuovi indirizzi IP dedicati esterni e interni (DIP) per EDGE1. Le dip precedenti in EDGE1 vengono configurate automaticamente come indirizzi IP virtuali (VIP) per il cluster. Il nuovo DIP esterno è 131.107.0.10, il nuovo DIP IPv4 interno è 10.0.0.10, il nuovo DIP IPv6 interno è 2001: DB8:1:: 10. I VIP del cluster sono 131.107.0.2 e 131.107.0.3 (External) e 10.0.0.2 e 2001: DB8:1:: 2 (interno).
  
3. Aggiungere EDGE2 al cluster con bilanciamento del carico. Dopo aver abilitato il bilanciamento del carico, è ora possibile aggiungere EDGE2 al cluster per fornire il bilanciamento del carico e la disponibilità elevata per le connessioni client DirectAccess.

## <a name="prerequisites"></a>Prerequisiti

Se si sta creando questo Lab di test nelle macchine virtuali, è necessario abilitare lo spoofing degli indirizzi MAC su EDGE1 e EDGE2.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Abilitare lo spoofing degli indirizzi MAC in EDGE1 e EDGE2  
  
1.  Eseguire un arresto normale su EDGE1 e EDGE2.  
  
2.  Nel computer che ospita le macchine virtuali, nella console di **gestione di Hyper-V**fare clic con il pulsante destro del mouse su Edge1 e quindi scegliere **Impostazioni**.  
  
3.  Nell'elenco **hardware** della finestra di dialogo **Impostazioni** fare clic sulla scheda di rete connessa alla rete corpnet, quindi nel riquadro dei dettagli selezionare la casella **di controllo Abilita spoofing indirizzi Mac** .  
  
4.  Nell'elenco **hardware** fare clic sulla scheda di rete connessa alla rete Internet, quindi nel riquadro dei dettagli selezionare la casella di controllo **Abilita spoofing degli indirizzi Mac** .  
  
5.  Nella finestra di dialogo **Impostazioni** fare clic su **OK**.  
  
6.  Ripetere questa procedura dal passaggio 2 in EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Installare la funzionalità Bilanciamento carico di rete in EDGE1 e EDGE2  
Per configurare EDGE1 e EDGE2 in un cluster, è necessario installare la funzionalità Bilanciamento carico di rete in EDGE1 e EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Per installare Bilanciamento carico di rete  
  
1.  In EDGE1, nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** quattro volte per visualizzare la schermata di selezione delle funzionalità server.  
  
3.  Nel **Selezionare le funzionalità** finestra di dialogo, selezionare **Bilanciamento carico di rete**, fare clic su **Aggiungi funzionalità**, fare clic su **Avanti**, quindi fare clic su **installare**.  
  
4.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  
5.  Ripetere questa procedura in EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Abilita bilanciamento del carico in EDGE1  
Usare questa procedura per abilitare il bilanciamento del carico e configurare i nuovi dip in EDGE1.  
  
### <a name="enable-load-balancing"></a>Abilita bilanciamento del carico  
  
1.  In EDGE1 fare clic sul pulsante **Start**, digitare **RAMgmtUI. exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di gestione accesso remoto, nel riquadro a sinistra, fare clic su **configurazione**, e quindi la **attività** riquadro, fare clic su **abilitare il bilanciamento del carico**.  
  
3.  Di abilitare carico bilanciamento guidata fare clic su **Avanti**.  
  
4.  Nella pagina **metodo di bilanciamento del carico** fare clic su **Usa bilanciamento carico di rete (NLB) di Windows**, quindi fare clic su **Avanti**.  
  
5.  Nella pagina **indirizzi IP dedicati esterni** , nella casella **indirizzo IPv4** digitare **131.107.0.10**, nella casella **subnet mask** verificare che il prefisso della subnet sia **255.255.255.0**, quindi fare clic su **Avanti**.  
  
6.  Nella pagina **indirizzi IP dedicati interni** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    1.  Nella casella **indirizzo IPv4** Digitare **10.0.0.10** e nella casella **subnet mask** verificare che il prefisso di subnet sia **255.255.255.0**.  
  
    2.  Nella casella **indirizzo IPv6** Digitare **2001: DB8:1:: 10** e nella lunghezza del prefisso della subnet, verificare che il valore sia **64**.  
  
7.  Nel **riepilogo** pagina, fare clic su **Commit**.  
  
8.  Nel **abilitare il bilanciamento del carico** la finestra di dialogo, fare clic su **Chiudi**.  
  
9. Di abilitare carico bilanciamento guidata fare clic su **Chiudi**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Aggiungere EDGE2 al cluster con bilanciamento del carico  
Utilizzare questa procedura per aggiungere EDGE2 al cluster NLB.  
  
> [!NOTE]  
> Dopo aver completato i passaggi precedenti, è necessario attendere due minuti prima di procedere. Dopo aver abilitato NLB, RAConfigTask esegue e configura il computer con le impostazioni di bilanciamento carico di sistema. Il completamento dell'operazione potrebbe richiedere alcuni minuti e, se l'amministratore esegue un'altra configurazione correlata a NLB prima della fine dell'attività, la configurazione avrà esito negativo.  
  
### <a name="add-edge2-to-the-cluster"></a>Aggiungere EDGE2 al cluster  
  
1.  Nel computer EDGE1 o nella macchina virtuale, nella console di gestione accesso remoto, nel riquadro **attività** , in **cluster con bilanciamento del carico**, fare clic su **Aggiungi o Rimuovi server**.  
  
2.  Nel **aggiungere o rimuovere server** la finestra di dialogo, fare clic su **Aggiungi Server**.  
  
3.  Nella pagina **Selezione server** della procedura guidata **Aggiungi un server** Digitare **EDGE2**e quindi fare clic su **Avanti**.  
  
4.  Nella pagina **schede di rete** , in **Adapter esterno**, assicurarsi che sia selezionata l'opzione **Internet** e in **Adapter interno**assicurarsi che sia selezionato **corpnet** . Fare clic su **Sfoglia**, nella finestra di dialogo **sicurezza di Windows** verificare che sia selezionato **certificato IP-HTTPS** , fare clic su **OK**e quindi fare clic su **Avanti**.  
  
5.  Nel **riepilogo** pagina, fare clic su **Aggiungi**.  
  
6.  Nel **completamento** pagina, fare clic su **Chiudi**.  
  
7.  Nel **aggiungere o rimuovere server** la finestra di dialogo, fare clic su **Commit**.  
  
8.  Nel **aggiunta e rimozione di server** la finestra di dialogo, fare clic su **Chiudi**.  
  
9. Nella schermata **Start** Digitare**Nlbmgr. exe**e premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
10. In **Gestione bilanciamento carico di rete**fare clic su **cluster interno da**. Nel riquadro dei dettagli verificare che lo stato di **Edge1 (Corpnet)** e **EDGE2 (Corpnet)** sia **convergente**.  
  
11. Se un server non è **convergente**, nell'albero della console fare clic con il pulsante destro del mouse sul server, scegliere **Controlla host**, quindi fare clic su **Avvia**.  
  
12. In **Gestione bilanciamento carico di rete**fare clic su **Internet da cluster**. Assicurarsi che nel riquadro dei dettagli, sia **Edge1 (Internet)** che **EDGE2 (Internet)** abbiano lo stato **convergente**.  
  
13. Se un server non è **convergente**, nell'albero della console fare clic con il pulsante destro del mouse sul server, scegliere **Controlla host**, quindi fare clic su **Avvia**.
