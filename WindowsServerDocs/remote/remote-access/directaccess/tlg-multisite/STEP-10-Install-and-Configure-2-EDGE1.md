---
title: 'PASSAGGIO 10: installare e configurare 2-EDGE1'
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7044a0d219decceb7669e92e884c78bcd9e3a61c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388387"
---
# <a name="step-10-install-and-configure-2-edge1"></a>PASSAGGIO 10: installare e configurare 2-EDGE1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

2-la configurazione di EDGE1 è costituita dagli elementi seguenti:  
  
- Installare il sistema operativo in 2 EDGE1. Installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 su 2 EDGE1.  
  
- Configurare le proprietà TCP/IP. Configurare 2-EDGE1 con indirizzi statici su entrambe le interfacce di rete.  
  
- Configurare il routing tra subnet. Per abilitare la comunicazione tra le subnet Corpnet e 2-corpnet, è necessario configurare il routing.  
  
- Unire 2-EDGE1 al dominio CORP2. Unire 2-EDGE1 al dominio corp2.corp.contoso.com.  
  
- Ottenere i certificati su 2 EDGE1. I certificati sono necessari per la connessione IPsec tra i client DirectAccess e il server di accesso remoto e per autenticare il listener IP-HTTPS quando i client si connettono tramite HTTPS.  
  
- Fornire l'accesso a CORP\User1. L'utente Corp\user1. è l'amministratore di accesso remoto. Per consentire all'utente di apportare modifiche a 2 EDGE1 da EDGE1, è necessario concedere l'accesso all'utente.  
  
- Installare il ruolo accesso remoto in 2 EDGE1. Per abilitare una distribuzione multisito, è necessario installare il ruolo accesso remoto in 2 EDGE1.  
  
2: EDGE1 deve disporre di due schede di rete installate.  
  
## <a name="installOS"></a>Installare il sistema operativo in 2 EDGE1  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettere 2-EDGE1 a una rete dotata di accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, quindi disconnettersi da Internet.  
  
4.  Connettere una scheda di rete alla subnet 2-corpnet e l'altra alla rete Internet simulata.  
  
## <a name="tcpip"></a>Configurare le proprietà TCP/IP  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  In **connessioni di rete**fare clic con il pulsante destro del mouse sulla connessione di rete connessa alla subnet Corpnet, fare clic su **Rinomina**, digitare **2-corpnet**e quindi premere INVIO.  
  
3.  Fare clic con il pulsante destro del mouse su **2-corpnet**, quindi scegliere **Proprietà**.  
  
4.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
5.  Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**Digitare **10.2.0.20**, in **subnet mask**, digitare **255.255.255.0**.  
  
6.  Fare clic su **Utilizza i seguenti indirizzi server DNS**. In **server DNS preferito**Digitare **10.2.0.1**e in **server DNS alternativo**digitare **10.0.0.1**.  
  
7.  Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
8.  In **suffisso DNS per la connessione**Digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK** due volte.  
  
9. Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
10. Fare clic su **Usa il seguente indirizzo IPv6**. In **indirizzo IPv6**Digitare **2001: DB8:2:: 20**, in **lunghezza prefisso subnet**, digitare **64**. Fare clic su **utilizza i seguenti indirizzi server DNS**e in **server DNS preferito**, digitare **2001: DB8:2:: 1**, in **server DNS alternativo**, digitare **2001: DB8:1:: 1**.  
  
11. Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
12. In **suffisso DNS per la connessione**Digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK** due volte.  
  
13. Nella finestra di dialogo **Proprietà 2-corpnet** fare clic su **Chiudi**.  
  
14. Nella finestra **connessioni di rete** fare clic con il pulsante destro del mouse sulla connessione di rete connessa alla subnet Internet, scegliere **Rinomina**, digitare **Internet**, quindi premere INVIO.  
  
15. Fare clic con il pulsante destro del mouse su **Internet**, quindi scegliere **Proprietà**.  
  
16. Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
17. Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**, tipo **131.107.0.20**. In **Subnet mask**digitare **255.255.255.0**.  
  
18. Fare clic su **Avanzate**. Nella scheda **impostazioni IP** , nell'area **indirizzi IP** , fare clic su **Aggiungi**. Nella finestra di dialogo **indirizzo TCP/IP** , in **indirizzo IP** digitare **131.107.0.21**, in **subnet mask** digitare **255.255.255.0**, quindi fare clic su **Aggiungi**.  
  
19. Fare clic sulla scheda **DNS** .  
  
20. In **suffisso DNS per la connessione**Digitare **ISP.example.com**, fare clic due volte su **OK** e quindi fare clic su **Chiudi**.  
  
21. Chiudere la finestra **Connessioni di rete**.  
  
## <a name="routing"></a>Configurare il routing tra subnet  
  
1.  Nella schermata **Start** Digitare**cmd. exe**, quindi premere INVIO.  
  
2.  Nella finestra del prompt dei comandi immettere i comandi seguenti. Dopo l'immissione di ogni comando, premere INVIO.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Per controllare la comunicazione di rete tra 2-EDGE1 e DC1, digitare **ping DC1.Corp.contoso.com**.  
  
4.  Verificare che ci siano quattro risposte dall'indirizzo IPv4, 10.0.0.1 o dall'indirizzo IPv6, 2001: DB8:1:: 1.  
  
5.  Chiudere la finestra del prompt dei comandi.  
  
## <a name="Join"></a>Join 2-EDGE1 al dominio CORP2  
  
1.  Nella console di Server Manager, in **server locale**, nell'area **Proprietà** , accanto a **nome computer**, fare clic sul collegamento.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  Nella finestra di dialogo **Cambiamenti dominio/nome computer** Digitare **2-Edge1**in **nome computer**. In **membro di**fare clic su **dominio**, digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK**.  
  
4.  Se viene richiesto di immettere un nome utente e una password, digitare **Administrator** e la relativa password e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo che ospita il dominio corp2.corp.contoso.com, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, fare clic su **Cambia utente**, quindi fare clic su **altro utente** e accedere al dominio CORP2 con l'account amministratore.  
  
## <a name="certs"></a>Ottenere i certificati su 2-EDGE1  
  
1.  Nella schermata **Start** Digitare**MMC. exe**, quindi premere INVIO.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **computer locale**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nell'albero della console dello snap-in certificati aprire **certificati (computer locale) \personal**.  
  
5.  Fare clic con il pulsante destro del mouse su **personale**, scegliere **tutte le attività**, quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti** .  
  
7.  Nella pagina **Richiedi certificati** selezionare le caselle di controllo **autenticazione client-server** e **server Web** , quindi fare clic su **sono necessarie ulteriori informazioni per registrare il certificato**.  
  
8.  Nella finestra di dialogo **Proprietà certificato** , nella scheda **oggetto** , nell'area **nome oggetto** , in **tipo**, selezionare **nome comune**.  
  
9. In **valore**Digitare **2-Edge1.contoso.com**e quindi fare clic su **Aggiungi**.  
  
10. Nel **nome alternativo** area, in **tipo**, selezionare **DNS**.  
  
11. In **valore**immettere **2-Edge1.contoso.com**, quindi fare clic su **Aggiungi**.  
  
12. Nella scheda **generale** , in **nome descrittivo**, digitare **certificato IP-HTTPS**.  
  
13. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
14. Nel riquadro dei dettagli dello snap-in certificati verificare che un nuovo certificato con il nome 2-edge1.contoso.com sia stato registrato con finalità designate di autenticazione server e che sia stato registrato un nuovo certificato con il nome 2-edge1.corp2.corp.contoso.com Finalità previste dell'autenticazione client e dell'autenticazione server.  
  
15. Chiudere la finestra della console. Se viene richiesto di salvare le impostazioni, fare clic su **No**.  
  
## <a name="Access"></a>Fornire l'accesso a Corp\user1.  
  
1.  Nella schermata **Start** Digitare**Compmgmt. msc**, quindi premere INVIO.  
  
2.  Nel riquadro a sinistra, fare clic su **utenti e gruppi locali**.  
  
3.  Fare doppio clic su **gruppi**, quindi fare doppio clic su **amministratori**.  
  
4.  Nel **proprietà amministratori** nella finestra di dialogo fare clic su **Aggiungi**, e il **Seleziona utenti, computer, account del servizio o gruppi** nella finestra di dialogo fare clic su **percorsi**.  
  
5.  Nella finestra di dialogo **percorsi** fare clic su **Corp.contoso.com**nell'albero del **percorso** e quindi fare clic su **OK**.  
  
6.  In **immettere i nomi degli oggetti da selezionare** digitare **User1**e quindi fare clic su **OK**.  
  
7.  Nel **proprietà amministratori** la finestra di dialogo, fare clic su **OK**.  
  
8.  Chiudere la finestra Gestione Computer.  
  
## <a name="InstallDA"></a>Installare il ruolo accesso remoto su 2-EDGE1  
  
1.  Nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
3.  Nella pagina **Selezione ruoli server**, selezionare **Accesso remoto**, fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.  
  
4.  Fare clic su **Avanti** per cinque volte.  
  
5.  Nella finestra di dialogo **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
6.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  


