---
title: PASSAGGIO 7 di installare e configurare 2-APP1
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
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b0f91b4d2b876cb7b22dc8614e7ea5dcce6da2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833562"
---
# <a name="step-7-install-and-configure-2-app1"></a>PASSAGGIO 7 di installare e configurare 2-APP1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

2-APP1 fornisce servizi di condivisione file e web. Configurazione 2-APP1 è costituita dai seguenti elementi:  
  
- Installare il sistema operativo in 2-APP1  
  
- Configurare le proprietà TCP/IP  
  
- Aggiungere 2-APP1 al dominio CORP2  
  
- Installare il ruolo Server Web (IIS) in 2-APP1  
  
- Creare una cartella condivisa in 2-APP1 
  
## <a name="bkmk_InstallOS"></a>Installare il sistema operativo in 2-APP1  
In primo luogo, installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>Per installare il sistema operativo in 2-APP1  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa).  
  
2.  Attenersi alle istruzioni per completare l'installazione, specificando una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  2-APP1 di connettersi a una rete che abbia accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e quindi disconnettersi da Internet.  
  
4.  Connettere 2-APP1 nella subnet Corpnet di 2.  
  
## <a name="bkmk_TCP"></a>Configurare le proprietà TCP/IP  
Configurare le proprietà TCP/IP 2-APP1.  
  
#### <a name="to-configure-tcpip-properties"></a>Per configurare le proprietà TCP/IP  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi il **delle proprietà** area, accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nel **connessioni di rete** finestra, fare doppio clic su **connessione Ethernet cablata**, quindi fare clic su **proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  Fare clic su **Utilizza il seguente indirizzo IP**. Nelle **indirizzo IP**, digitare **10.2.0.3**. In **Subnet mask**digitare **255.255.255.0**. Nelle **gateway predefinito**, digitare **10.2.0.254**.  
  
5.  Fare clic su **Utilizza i seguenti indirizzi server DNS**. Nelle **server DNS preferito**, digitare **10.2.0.1**.  
  
6.  Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda. Nelle **suffisso DNS per la connessione**, digitare **corp2.corp.contoso.com**, fare clic su **OK** due volte.  
  
7.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)**, quindi fare clic su **Proprietà**.  
  
8.  Fare clic su **Usa l'indirizzo IPv6 seguente**. Nelle **indirizzo IPv6**, digitare **2001:db8:2::3**. Nelle **lunghezza prefisso Subnet**, digitare **64**. Nelle **gateway predefinito**, digitare **2001:db8:2::fe**. Fare clic su **utilizza i seguenti indirizzi server DNS**e nella **server DNS preferito**, digitare **2001:db8:2::1**.  
  
9. Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
10. Nella **suffisso DNS per la connessione**, digitare **corp2.corp.contoso.com**, quindi fare clic su **OK** due volte.  
  
11. Nel **proprietà di connessione Ethernet cablata** fare clic su finestra di dialogo **Chiudi**.  
  
12. Chiudere la finestra **Connessioni di rete**.  
  
## <a name="bkmk_JoinDomain"></a>Aggiungere 2-APP1 al dominio CORP2  
Aggiungere 2-APP1 al dominio corp2.corp.contoso.com.  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Per aggiungere 2-APP1 al dominio CORP2  
  
1.  Nella console di Server Manager, in **Server locale**, nella **delle proprietà** area, accanto a **nome Computer**, fare clic sul collegamento.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  Nelle **nome Computer**, digitare **2-APP1**. In **membro del**, fare clic su **dominio**, digitare **corp2.corp.contoso.com**, quindi fare clic su **OK**.  
  
4.  Quando viene chiesto di immettere un nome utente e password, digitare **Administrator** e la relativa password e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo di benvenuto al dominio corp2.corp.contoso.com, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, fare clic su **Cambia utente**, quindi fare clic su **altro utente** e accedere al dominio CORP2 con l'account amministratore.  
  
## <a name="bkmk_IIS"></a>Installare il ruolo Server Web (IIS) in 2-APP1  
Installare il ruolo Server Web (IIS) per rendere un server web 2-APP1.  
  
#### <a name="to-install-the-web-server-iis-role"></a>Per installare il ruolo Server Web (IIS)  
  
1.  Nella console di Server Manager, sul **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** tre volte per visualizzare la schermata di selezione ruoli server  
  
3.  Nel **Selezione ruoli server** pagina, selezionare **Server Web (IIS)**, quindi fare clic su **successivo** quattro volte.  
  
4.  Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
5.  Verificare che l'installazione sia riuscita e quindi fare clic su **Chiudi**.  
  
## <a name="bkmk_Share"></a>Creare una cartella condivisa in 2-APP1  
Creare una cartella condivisa e un file di testo all'interno della cartella in 2-APP1.  
  
#### <a name="to-create-a-shared-folder"></a>Per creare una cartella condivisa  
  
1.  Nel **avviare** digitare**explorer.exe**, quindi premere INVIO.  
  
2.  Fare clic su **Computer**, quindi fare doppio clic su **disco locale (c)**.  
  
3.  Fare clic su **nuova cartella**, digitare **file**, quindi premere INVIO. Lasciare il **disco locale** finestra aperta.  
  
4.  Nel **avviare** digitare**notepad.exe**, fare doppio clic su **Notepad**, fare clic su **avanzate**e quindi fare clic su **runas amministratore**.  
  
5.  Nel **Untitled - Notepad** finestra, digitare **si tratta di un file condiviso in 2-APP1**.  
  
6.  Fare clic su **File**, fare clic su **salvare**, fare clic su **Computer**, fare doppio clic su **disco locale (c)** e quindi fare doppio clic il **file**  cartella.  
  
7.  In **nome File**, digitare **example. txt**, quindi fare clic su **Salva**. Chiudere Blocco note.  
  
8.  Nel **disco locale** finestra, fare doppio clic sui **file** cartella, scegliere **condividere con**e quindi fare clic su **persone specifiche**.  
  
9. Nel **condivisione File** finestra di dialogo, nell'elenco a discesa elenco, fare clic su **tutti gli utenti**e quindi fare clic su **Add**. Nelle **livello di autorizzazione** per **Everyone**, fare clic su **lettura/scrittura**.  
  
10. Fare clic su **Share**, quindi fare clic su **eseguita**.  
  
11. Chiudi il **disco locale** finestra.  
  


