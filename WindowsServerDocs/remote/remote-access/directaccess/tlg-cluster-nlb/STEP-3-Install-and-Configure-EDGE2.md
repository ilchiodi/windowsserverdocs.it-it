---
title: PASSAGGIO 3 installare e configurare EDGE2
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess in un cluster con bilanciamento carico di servizio di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3dad1db575bd9b9b4a70a24da44d1d030273f021
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404866"
---
# <a name="step-3-install-and-configure-edge2"></a>PASSAGGIO 3 installare e configurare EDGE2

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

EDGE2 è il secondo membro di un cluster di accesso remoto. EDGE2 viene installato e configurato prima di abilitare la configurazione del cluster.

Per configurare EDGE2, seguire questa procedura:

## <a name="installOS"></a>Installare il sistema operativo in EDGE2  
  
1.  In EDGE2 avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettere EDGE2 a una rete dotata di accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, quindi disconnettersi da Internet.  
  
4.  Connettere una scheda di rete alla subnet Corpnet o al Commuter virtuale che rappresenta la subnet Corpnet e l'altra alla subnet Internet o al Commuter virtuale che rappresenta la subnet Internet.  
  
## <a name="TCP"></a>Configurare le proprietà TCP/IP  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nella finestra **connessioni di rete** fare clic con il pulsante destro del mouse sulla connessione di rete connessa alla subnet Corpnet o al Commuter virtuale, quindi fare clic su **Rinomina**.  
  
3.  Digitare **corpnet**, quindi premere INVIO.  
  
4.  Fare clic con il pulsante destro del mouse su **corpnet**e quindi scegliere **Proprietà**.  
  
5.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
6.  Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**Digitare **10.0.0.8**. In **Subnet mask**digitare **255.255.255.0**.  
  
7.  Fare clic su **Utilizza i seguenti indirizzi server DNS**. In **Server DNS preferito** digitare **10.0.0.1**.  
  
8.  Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
9. In **suffisso DNS per la connessione**Digitare **Corp.contoso.com**, quindi fare clic su **OK** due volte.  
  
10. Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
11. Fare clic su **Usa il seguente indirizzo IPv6**. In **indirizzo IPv6**Digitare **2001: DB8:1:: 8**. In **lunghezza prefisso subnet**Digitare **64**.  
  
12. Fare clic su **Utilizza i seguenti indirizzi server DNS**. In **server DNS preferito**Digitare **2001: DB8:1:: 1**.  
  
13. Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
14. In **suffisso DNS per la connessione**Digitare **Corp.contoso.com**, fare clic due volte su **OK** e quindi fare clic su **Chiudi**.  
  
15. Nella finestra **connessioni di rete** fare clic con il pulsante destro del mouse sulla connessione di rete connessa alla subnet Internet e quindi scegliere **Rinomina**.  
  
16. Digitare **Internet**, quindi premere INVIO.  
  
17. Fare clic con il pulsante destro del mouse su **Internet**, quindi scegliere **Proprietà**.  
  
18. Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
19. Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**immettere **131.107.0.8**. In **subnet mask**immettere **255.255.255.0**.  
  
20. Fare clic sulla scheda **DNS** .  
  
21. In **suffisso DNS per la connessione**Digitare **ISP.example.com**, quindi fare clic due volte su **OK** e quindi su **Chiudi**.  
  
22. Chiudere la finestra **Connessioni di rete**.  
  
23. Per controllare la comunicazione di rete tra EDGE2 e DC1, fare clic sul pulsante **Start**, digitare **cmd**, quindi premere INVIO.  
  
24. Nella finestra del prompt dei comandi digitare **ping DC1.Corp.contoso.com** e premere INVIO. Verificare che ci siano quattro risposte da 10.0.0.1 o l'indirizzo IPv6 2001: DB8:1:: 1  
  
25. Chiudere la finestra del prompt dei comandi.  
  
## <a name="rename"></a>Rinominare EDGE2 e aggiungerlo al dominio  
  
1.  Nella console di Server Manager, in **server locale**, nell'area **Proprietà** , accanto a **nome computer**, fare clic sul collegamento.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** , nella casella **nome computer** digitare **EDGE2**. Nel **membro dell'** area, fare clic **su dominio**, quindi nella casella di testo immettere **Corp.contoso.com**, quindi fare clic su **OK**.  
  
4.  Quando viene richiesta l'immissione di nome utente e password, digitare **User1** e la relativa password e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo di benvenuto al dominio corp.contoso.com, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio, accedere come CORP\User1.  
  
## <a name="IPHTTPSCert"></a>Installare il certificato IP-HTTPS  
  
1.  Nella schermata **Start** Digitare**MMC. exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nel riquadro sinistro della console, passare a **certificati (Computer locale) \Personal\Certificates**. Fare clic con il pulsante destro del mouse sul nodo **certificati** , scegliere **tutte le attività**, quindi fare clic su **Richiedi nuovo certificato**.  
  
5.  Nella procedura guidata di registrazione certificato fare clic due volte su **Avanti** .  
  
6.  Nella pagina **Richiedi certificati** selezionare la casella di controllo **server Web** e quindi fare clic su **sono necessarie ulteriori informazioni per registrare il certificato**.  
  
7.  Nella finestra di dialogo **Proprietà certificato** , nella scheda **oggetto** , nell'area **nome oggetto** , nell'elenco **tipo** , fare clic su **nome comune**.  
  
8.  In **valore**Digitare **Edge1.contoso.com**e quindi fare clic su **Aggiungi**.  
  
9. Nell'area **nome alternativo** , nell'elenco **tipo** , fare clic su **DNS**.  
  
10. In **valore**Digitare **Edge1.contoso.com**e quindi fare clic su **Aggiungi**.  
  
11. Nella scheda **generale** , in **nome descrittivo**, digitare **certificato IP-HTTPS**.  
  
12. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
13. Nel riquadro dei dettagli dello snap-in certificati verificare che un nuovo certificato con il nome edge1.contoso.com sia stato registrato con finalità designate di autenticazione server.  
  
14. Chiudere la finestra della console. Se viene richiesto di salvare le impostazioni, fare clic su **No**.  
  
## <a name="InstallDA"></a>Installare il ruolo accesso remoto in EDGE2  
  
1.  Nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
3.  Nella pagina **Selezione ruoli server**, selezionare **Accesso remoto**, fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.  
  
4.  Fare clic su **Avanti** per cinque volte.  
  
5.  Nella finestra di dialogo **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
6.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  


