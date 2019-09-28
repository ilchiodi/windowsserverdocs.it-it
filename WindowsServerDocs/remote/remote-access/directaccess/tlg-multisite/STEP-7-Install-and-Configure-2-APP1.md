---
title: 'PASSAGGIO 7: installare e configurare 2-APP1'
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c5a316e1230692fb800c088d752c26ec4a0f3349
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388258"
---
# <a name="step-7-install-and-configure-2-app1"></a>PASSAGGIO 7: installare e configurare 2-APP1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

2: APP1 fornisce servizi di condivisione file e Web. 2-la configurazione di APP1 è costituita dagli elementi seguenti:  
  
- Installare il sistema operativo in 2 APP1  
  
- Configurare le proprietà TCP/IP  
  
- Join 2-APP1 al dominio CORP2  
  
- Installare il ruolo server Web (IIS) in 2 APP1  
  
- Creare una cartella condivisa in 2 APP1 
  
## <a name="bkmk_InstallOS"></a>Installare il sistema operativo in 2 APP1  
Prima di tutto, installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>Per installare il sistema operativo in APP1  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa).  
  
2.  Attenersi alle istruzioni per completare l'installazione, specificando una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettere 2-APP1 a una rete dotata di accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, quindi disconnettersi da Internet.  
  
4.  Connettere 2-APP1 alla subnet 2-Corpnet.  
  
## <a name="bkmk_TCP"></a>Configurare le proprietà TCP/IP  
Configurare le proprietà TCP/IP in 2 APP1.  
  
#### <a name="to-configure-tcpip-properties"></a>Per configurare le proprietà TCP/IP  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nella finestra **connessioni di rete** fare clic con il pulsante destro del mouse su **connessione Ethernet cablata**e quindi scegliere **proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**Digitare **10.2.0.3**. In **Subnet mask**digitare **255.255.255.0**. In **gateway predefinito**Digitare **10.2.0.254**.  
  
5.  Fare clic su **Utilizza i seguenti indirizzi server DNS**. In **server DNS preferito**Digitare **10.2.0.1**.  
  
6.  Fare clic su **Avanzate**e quindi sulla scheda **DNS** . In **suffisso DNS per la connessione**Digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK** due volte.  
  
7.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
8.  Fare clic su **Usa il seguente indirizzo IPv6**. In **indirizzo IPv6**Digitare **2001: DB8:2:: 3**. In **lunghezza prefisso subnet**Digitare **64**. In **gateway predefinito**Digitare **2001: DB8:2:: Fe**. Fare clic su **utilizza i seguenti indirizzi server DNS**e in **server DNS preferito**, digitare **2001: DB8:2:: 1**.  
  
9. Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
10. In **suffisso DNS per la connessione**Digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK** due volte.  
  
11. Nella finestra di dialogo **Proprietà connessione Ethernet cablata** fare clic su **Chiudi**.  
  
12. Chiudere la finestra **Connessioni di rete**.  
  
## <a name="bkmk_JoinDomain"></a>Join 2-APP1 al dominio CORP2  
Unire 2-APP1 al dominio corp2.corp.contoso.com.  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Per aggiungere 2 APP1 al dominio CORP2  
  
1.  Nella console di Server Manager, in **server locale**, nell'area **Proprietà** , accanto a **nome computer**, fare clic sul collegamento.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  In **nome computer**Digitare **2-App1**. In **membro di**fare clic su **dominio**, digitare **Corp2.Corp.contoso.com**e quindi fare clic su **OK**.  
  
4.  Quando viene richiesto di immettere un nome utente e una password, digitare **Administrator** e la relativa password e quindi fare clic su **OK**.  
  
5.  Quando viene visualizzata una finestra di dialogo che ospita il dominio corp2.corp.contoso.com, fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, fare clic su **Cambia utente**, quindi fare clic su **altro utente** e accedere al dominio CORP2 con l'account amministratore.  
  
## <a name="bkmk_IIS"></a>Installare il ruolo server Web (IIS) in 2 APP1  
Installare il ruolo server Web (IIS) per rendere APP1 un server Web.  
  
#### <a name="to-install-the-web-server-iis-role"></a>Per installare il ruolo server Web (IIS)  
  
1.  Nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** tre volte per visualizzare la schermata di selezione ruoli server  
  
3.  Nella pagina **Selezione ruoli server** selezionare **server Web (IIS)** e quindi fare clic quattro volte su **Avanti** .  
  
4.  Nella pagina **Conferma selezioni per l'installazione** fare clic su **Installa**.  
  
5.  Verificare che l'installazione sia stata completata correttamente, quindi fare clic su **Chiudi**.  
  
## <a name="bkmk_Share"></a>Creare una cartella condivisa in 2 APP1  
Creare una cartella condivisa e un file di testo all'interno della cartella in 2 APP1.  
  
#### <a name="to-create-a-shared-folder"></a>Per creare una cartella condivisa  
  
1.  Nel **avviare** digitare**explorer.exe**, quindi premere INVIO.  
  
2.  Fare clic su **computer**, quindi fare doppio clic su **disco locale (C:)** .  
  
3.  Fare clic su **nuova cartella**, digitare **files**, quindi premere INVIO. Lasciare aperta la finestra del **disco locale** .  
  
4.  Nella schermata **Start** Digitare**notepad. exe**, fare clic con il pulsante destro del mouse su **blocco note**, scegliere **Avanzate**e quindi fare clic su **Esegui come amministratore**.  
  
5.  Nella finestra senza **titolo-blocco note** digitare **questo è un file condiviso in 2 App1**.  
  
6.  Fare clic su **file**, su **Salva**, su **computer**, su **disco locale (C:)** , quindi fare doppio clic sulla cartella **file** .  
  
7.  In **nome file**Digitare **example. txt**, quindi fare clic su **Salva**. Chiudere Blocco note.  
  
8.  Nella finestra **disco locale** , fare clic con il pulsante destro del mouse sulla cartella **file** , scegliere **Condividi con**e quindi fare clic su **utenti specifici**.  
  
9. Nella finestra di dialogo **condivisione file** , nell'elenco a discesa, fare clic su **tutti**, quindi fare clic su **Aggiungi**. In **livello di autorizzazione** per **tutti gli utenti**fare clic su **lettura/scrittura**.  
  
10. Fare clic su **Condividi**, quindi fare clic su **fine**.  
  
11. Chiudere la finestra del **disco locale** .  
  


