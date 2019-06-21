---
title: PASSAGGIO 3 di installare e configurare EDGE2
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess in un Cluster con bilanciamento carico di rete di Windows per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 708a2a71f798b842e38510a2c5ea8ed10ae7656e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283364"
---
# <a name="step-3-install-and-configure-edge2"></a>PASSAGGIO 3 di installare e configurare EDGE2

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

EDGE2 è il secondo membro di un cluster di accesso remoto. EDGE2 viene installato e configurato prima di abilitare la configurazione del cluster.

Seguire i passaggi seguenti per configurare EDGE2:

## <a name="installOS"></a>Installare il sistema operativo in EDGE2  
  
1.  In EDGE2, avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettersi EDGE2 a una rete che abbia accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e quindi disconnettersi da Internet.  
  
4.  Connettere una scheda di rete alla subnet Corpnet o il commutatore virtuale che rappresenta la subnet corpnet e l'altra alla subnet Internet o commutatore virtuale che rappresenta la subnet Internet.  
  
## <a name="TCP"></a>Configurare le proprietà TCP/IP  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi il **delle proprietà** area, accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nel **connessioni di rete** finestra, fare doppio clic su connessione di rete che è connessa al commutatore virtuale o subnet Corpnet e quindi fare clic su **rinominare**.  
  
3.  Tipo di **Corpnet**, quindi premere INVIO.  
  
4.  Fare doppio clic su **Corpnet**, quindi fare clic su **proprietà**.  
  
5.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
6.  Fare clic su **Utilizza il seguente indirizzo IP**. Nelle **indirizzo IP**, digitare **10.0.0.8**. In **Subnet mask**digitare **255.255.255.0**.  
  
7.  Fare clic su **Utilizza i seguenti indirizzi server DNS**. In **Server DNS preferito** digitare **10.0.0.1**.  
  
8.  Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
9. Nelle **suffisso DNS per la connessione**, digitare **corp.contoso.com**, fare clic su **OK** due volte.  
  
10. Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
11. Fare clic su **Usa l'indirizzo IPv6 seguente**. Nelle **indirizzo IPv6**, digitare **2001:db8:1::8**. Nelle **lunghezza prefisso Subnet**, digitare **64**.  
  
12. Fare clic su **Utilizza i seguenti indirizzi server DNS**. Nelle **server DNS preferito**, digitare **2001:db8:1::1**.  
  
13. Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
14. Nelle **suffisso DNS per la connessione**, digitare **corp.contoso.com**, fare clic su **OK** due volte e quindi fare clic su **Chiudi**.  
  
15. Nel **connessioni di rete** finestra, fare doppio clic su connessione di rete che è connesso alla subnet Internet e quindi fare clic su **rinominare**.  
  
16. Tipo di **Internet**, quindi premere INVIO.  
  
17. Fare doppio clic su **Internet**, quindi fare clic su **proprietà**.  
  
18. Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
19. Fare clic su **Utilizza il seguente indirizzo IP**. Nelle **indirizzo IP**, immettere **131.107.0.8**. Nelle **Subnet mask**, immettere **255.255.255.0**.  
  
20. Scegliere il **DNS** scheda  
  
21. Nelle **suffisso DNS per la connessione**, digitare **isp.example.com**e quindi fare clic su **OK** due volte e quindi fare clic su **Chiudi**.  
  
22. Chiudere la finestra **Connessioni di rete**.  
  
23. Per controllare la comunicazione di rete tra EDGE2 e DC1, fare clic su **avviare**, digitare **cmd**, quindi premere INVIO.  
  
24. Nella finestra del prompt dei comandi, digitare **ping dc1.corp.contoso.com** e premere INVIO. Verificare che siano presenti quattro risposte da 10.0.0.1 o 2001:db8:1::1 di indirizzi IPv6  
  
25. Chiudere la finestra del prompt dei comandi.  
  
## <a name="rename"></a>Rinominare EDGE2 e aggiungerla al dominio  
  
1.  Nella console di Server Manager, in **Server locale**, nella **delle proprietà** area, accanto a **nome Computer**, fare clic sul collegamento.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  Nel **cambiamenti dominio/nome Computer** nella finestra di dialogo il **nome Computer** , digitare **EDGE2**. Nel **membro del** area, fare clic su **dominio**e nella casella di testo, immettere **corp.contoso.com**, quindi fare clic su **OK**.  
  
4.  Quando viene richiesta l'immissione di nome utente e password, digitare **User1** e la relativa password e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo di benvenuto al dominio corp.contoso.com, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio, eseguire l'accesso come CORP\User1.  
  
## <a name="IPHTTPSCert"></a>Installare il certificato IP-HTTPS  
  
1.  Nel **avviare** digitare**mmc.exe**, quindi premere INVIO. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nel riquadro sinistro della console, passare a **certificati (Computer locale) \Personal\Certificates**. Fare clic il **certificati** nodo, scegliere **tutte le attività**, quindi fare clic su **Richiedi nuovo certificato**.  
  
5.  Nella procedura guidata registrazione certificato, fare clic su **successivo** due volte.  
  
6.  Nel **richiesta certificati** pagina, selezionare la **Server Web** casella di controllo e quindi fare clic su **sono necessarie ulteriori informazioni per registrare questo certificato**.  
  
7.  Nel **proprietà certificato** finestra di dialogo il **soggetto** nella scheda il **nome soggetto** area, nella **tipo** elenco, fare clic su **Nome comune**.  
  
8.  Nelle **valore**, digitare **edge1.contoso.com**, quindi fare clic su **Add**.  
  
9. Nel **nome alternativo** area, nelle **tipo** fare clic su **DNS**.  
  
10. Nelle **valore**, digitare **edge1.contoso.com**, quindi fare clic su **Add**.  
  
11. Nel **generali** nella scheda **soprannome**, tipo **certificato IP-HTTPS**.  
  
12. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
13. Nel riquadro dei dettagli dello snap-in certificati, verificare che un nuovo certificato con il nome edge1.contoso.com iscritto a scopo di autenticazione Server designati.  
  
14. Chiudere la finestra della console. Se viene chiesto di salvare le impostazioni, fare clic su **No**.  
  
## <a name="InstallDA"></a>Installare il ruolo Accesso remoto in EDGE2  
  
1.  Nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
3.  Nella pagina **Selezione ruoli server**, selezionare **Accesso remoto**, fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.  
  
4.  Fare clic su **Avanti** per cinque volte.  
  
5.  Nella finestra di dialogo **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
6.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  


