---
title: PASSAGGIO 10 installare e configurare EDGE1 2
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare una distribuzione multisito DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1640dbae52a1a7c93355b34822d72faa5351bcda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860312"
---
# <a name="step-10-install-and-configure-2-edge1"></a>PASSAGGIO 10 installare e configurare EDGE1 2

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

2-EDGE1 configurazione è costituita dai seguenti elementi:  
  
- Installare il sistema operativo in EDGE1 2. Installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 in EDGE1 2.  
  
- Configurare le proprietà TCP/IP. Configurare EDGE1 2 con gli indirizzi statici su entrambe le interfacce di rete.  
  
- Configurare il routing tra subnet. Per abilitare la comunicazione tra le subnet Corpnet e 2-Corpnet, è necessario configurare il routing.  
  
- Aggiungere EDGE1 2 al dominio CORP2. Join EDGE1 2 per il dominio corp2.corp.contoso.com.  
  
- Ottenere i certificati su EDGE1 2. I certificati sono necessari per la connessione IPsec tra client DirectAccess e il server di accesso remoto e per autenticare il listener IP-HTTPS, quando i client si connettono tramite HTTPS.  
  
- Fornire l'accesso a CORP\User1. L'utente CORP\User1 è l'amministratore di accesso remoto. Per consentire all'utente di apportare modifiche alla EDGE1 2 da EDGE1, è necessario concedere l'accesso all'utente.  
  
- Installarvi il ruolo Accesso remoto EDGE1 2. Per abilitare una distribuzione multisito, è necessario installarvi il ruolo Accesso remoto EDGE1 2.  
  
2-EDGE1 deve avere due schede di rete installati.  
  
## <a name="installOS"></a>Installare il sistema operativo in EDGE1 2  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettersi EDGE1 2 a una rete che abbia accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e quindi disconnettersi da Internet.  
  
4.  Connettere una scheda di rete alla subnet Corpnet di 2 e l'altra alla rete Internet simulata.  
  
## <a name="tcpip"></a>Configurare le proprietà TCP/IP  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi il **delle proprietà** area, accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nella **connessioni di rete**, fare doppio clic su connessione di rete che è connesso alla subnet Corpnet di 2, fare clic su **rinominare**, digitare **2-Corpnet**, e quindi premere INVIO.  
  
3.  Fare doppio clic su **2-Corpnet**, quindi fare clic su **proprietà**.  
  
4.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
5.  Fare clic su **Utilizza il seguente indirizzo IP**. Nella **indirizzo IP**, digitare **10.2.0.20**, in **Subnet mask**, digitare **255.255.255.0**.  
  
6.  Fare clic su **Utilizza i seguenti indirizzi server DNS**. Nella **server DNS preferito**, digitare **10.2.0.1**e in **server DNS alternativo**, digitare **10.0.0.1**.  
  
7.  Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
8.  Nella **suffisso DNS per la connessione**, digitare **corp2.corp.contoso.com**, quindi fare clic su **OK** due volte.  
  
9. Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)**, quindi fare clic su **Proprietà**.  
  
10. Fare clic su **Usa l'indirizzo IPv6 seguente**. Nelle **indirizzo IPv6**, digitare **2001:db8:2::20**, in **lunghezza prefisso Subnet**, tipo **64**. Fare clic su **utilizza i seguenti indirizzi server DNS**e nella **server DNS preferito**, digitare **2001:db8:2::1**nella **server DNS alternativo**, tipo di **2001:db8:1::1**.  
  
11. Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
12. Nella **suffisso DNS per la connessione**, digitare **corp2.corp.contoso.com**, quindi fare clic su **OK** due volte.  
  
13. Nel **proprietà 2-Corpnet** della finestra di dialogo fare clic su **Chiudi**.  
  
14. Nel **connessioni di rete** finestra, fare clic su connessione di rete che è connesso alla subnet Internet, fare clic su **rinominare**, digitare **Internet**, e quindi premere INVIO .  
  
15. Fare doppio clic su **Internet**, quindi fare clic su **proprietà**.  
  
16. Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
17. Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**, tipo **131.107.0.20**. In **Subnet mask**digitare **255.255.255.0**.  
  
18. Fare clic su **Avanzate**. Nel **le impostazioni IP** nella scheda il **gli indirizzi IP** area, fare clic su **Add**. Nel **indirizzo TCP/IP** nella finestra di dialogo **indirizzo IP** tipo **131.107.0.21**nella **Subnet mask** tipo **255.255.255.0** , quindi fare clic su **Add**.  
  
19. Scegliere il **DNS** scheda.  
  
20. Nelle **suffisso DNS per la connessione**, digitare **isp.example.com**, fare clic su **OK** due volte e quindi fare clic su **Chiudi**.  
  
21. Chiudere la finestra **Connessioni di rete**.  
  
## <a name="routing"></a>Configurare il routing tra subnet  
  
1.  Nel **avviare** digitare**cmd.exe**, quindi premere INVIO.  
  
2.  Nella finestra del prompt dei comandi, immettere i comandi seguenti. Dopo aver immesso tutti i comandi, premere INVIO.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Per controllare la comunicazione di rete tra 2-EDGE1 e DC1, digitare **ping dc1.corp.contoso.com**.  
  
4.  Verificare che siano presenti quattro risposte dall'indirizzo IPv4, 10.0.0.1, o dall'indirizzo IPv6, 2001:db8:1::1.  
  
5.  Chiudere la finestra del prompt dei comandi.  
  
## <a name="Join"></a>Aggiungere EDGE1 2 al dominio CORP2  
  
1.  Nella console di Server Manager, in **Server locale**, nella **delle proprietà** area, accanto a **nome Computer**, fare clic sul collegamento.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  Nel **cambiamenti dominio/nome Computer** nella finestra di dialogo **nome Computer**, digitare **EDGE1 2**. In **membro del**, fare clic su **dominio**, digitare **corp2.corp.contoso.com**, quindi fare clic su **OK**.  
  
4.  Se viene chiesto di immettere un nome utente e password, digitare **Administrator** e la relativa password e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo di benvenuto al dominio corp2.corp.contoso.com, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, fare clic su **Cambia utente**, quindi fare clic su **altro utente** e accedere al dominio CORP2 con l'account amministratore.  
  
## <a name="certs"></a>Ottenere i certificati su EDGE1 2  
  
1.  Nel **avviare** digitare**mmc.exe**, quindi premere INVIO.  
  
2.  Nella console di MMC, nel **File** menu, fare clic su **Aggiungi/Rimuovi Snap-in**.  
  
3.  Nel **Aggiungi o Rimuovi Snap-in** la finestra di dialogo, fare clic su **certificati**, fare clic su **Aggiungi**, fare clic su **account Computer**, fare clic su **Avanti**, fare clic su **computer locale**, fare clic su **Fine**, e quindi fare clic su **OK**.  
  
4.  Nell'albero della console dello snap-in certificati, aprire **certificati (Computer locale) \Personal**.  
  
5.  Fare doppio clic su **personali**, scegliere **tutte le attività**, quindi fare clic su **Richiedi nuovo certificato**.  
  
6.  Fare clic due volte su **Avanti** .  
  
7.  Nel **richiesta certificati** pagina, selezionare la **autenticazione Client-Server** e il **Server Web** caselle di controllo e quindi fare clic su **per ulteriori informazioni necessarie per registrare questo certificato**.  
  
8.  Nel **proprietà certificato** finestra di dialogo il **soggetto** nella scheda il **nome soggetto** area, nella **tipo**selezionare  **Nome comune**.  
  
9. In **valore**, digitare **2 edge1.contoso.com**, quindi fare clic su **Aggiungi**.  
  
10. Nel **nome alternativo** area, in **tipo**, selezionare **DNS**.  
  
11. Nelle **valore**, immettere **2 edge1.contoso.com**, quindi fare clic su **Add**.  
  
12. Nel **generali** nella scheda **soprannome**, tipo **certificato IP-HTTPS**.  
  
13. Fare clic su **OK**, fare clic su **registrazione**, quindi fare clic su **Fine**.  
  
14. Nel riquadro dei dettagli dello snap-in certificati, verificare che sia registrato un nuovo certificato con il nome 2-edge1.contoso.com destinati a scopo di autenticazione Server e un nuovo certificato con il nome 2-edge1.corp2.corp.contoso.com sia registrato con Scopi di autenticazione Client e l'autenticazione Server.  
  
15. Chiudere la finestra della console. Se viene chiesto di salvare le impostazioni, fare clic su **No**.  
  
## <a name="Access"></a>Fornire l'accesso a CORP\User1  
  
1.  Nel **avviare** digitare**compmgmt. msc**, quindi premere INVIO.  
  
2.  Nel riquadro a sinistra, fare clic su **utenti e gruppi locali**.  
  
3.  Fare doppio clic su **gruppi**, quindi fare doppio clic su **amministratori**.  
  
4.  Nel **proprietà amministratori** nella finestra di dialogo fare clic su **Aggiungi**, e il **Seleziona utenti, computer, account del servizio o gruppi** nella finestra di dialogo fare clic su **percorsi**.  
  
5.  Nel **posizioni** nella finestra di dialogo il **posizione** struttura ad albero, fare clic su **corp.contoso.com**e quindi fare clic su **OK**.  
  
6.  Nel **immettere i nomi degli oggetti da selezionare** tipo **User1**, quindi fare clic su **OK**.  
  
7.  Nel **proprietà amministratori** la finestra di dialogo, fare clic su **OK**.  
  
8.  Chiudere la finestra Gestione Computer.  
  
## <a name="InstallDA"></a>Installarvi il ruolo Accesso remoto EDGE1 2  
  
1.  Nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
3.  Nella pagina **Selezione ruoli server**, selezionare **Accesso remoto**, fare clic su **Aggiungi funzionalità** e quindi su **Avanti**.  
  
4.  Fare clic su **Avanti** per cinque volte.  
  
5.  Nella finestra di dialogo **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
6.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione sia stata completata correttamente e quindi fare clic su **Chiudi**.  
  


