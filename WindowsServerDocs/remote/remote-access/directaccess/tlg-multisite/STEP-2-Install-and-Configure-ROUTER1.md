---
title: PASSAGGIO 2 installare e configurare ROUTER1
description: 'Questo argomento fa parte della Guida al Lab di test: illustra una distribuzione multisito di DirectAccess per Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5e2b0759e85c05b4ee33971849bd1096d4c8419c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861604"
---
# <a name="step-2-install-and-configure-router1"></a>PASSAGGIO 2 installare e configurare ROUTER1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questa guida al Lab di test multisito, il computer router fornisce un Bridge IPv4 e IPv6 tra le subnet Corpnet e 2-corpnet e funge da router per il traffico IP-HTTPS e Teredo.  
  
- Installare il sistema operativo in ROUTER1 
  
- Configurare le proprietà TCP/IP e rinominare il computer  
  
- Spegnere il firewall
  
- Configurare routing e inoltro
  
## <a name="install-the-operating-system-on-router1"></a>Installare il sistema operativo in ROUTER1  
Prima di tutto, installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-router1"></a>Per installare il sistema operativo in ROUTER1  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa).  
  
2.  Attenersi alle istruzioni per completare l'installazione, specificando una password complessa per l'account amministratore locale. Accedere usando l'account dell'amministratore locale.  
  
3.  Connettere ROUTER1 a una rete dotata di accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, quindi disconnettersi da Internet.  
  
4.  Connettere ROUTER1 alle subnet Corpnet e 2-Corpnet.  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configurare le proprietà TCP/IP e rinominare il computer  
Configurare le impostazioni TCP/IP sul router e rinominare il computer in ROUTER1.  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Per configurare le proprietà TCP/IP e rinominare il computer  
  
1.  Nella console di Server Manager fare clic su **server locale**e quindi nell'area **Proprietà** , accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nella finestra **connessioni di rete** fare clic con il pulsante destro del mouse sulla scheda di rete connessa a corpnet, scegliere **Rinomina**, digitare **corpnet**e premere INVIO.  
  
3.  Fare clic con il pulsante destro del mouse su **corpnet**e quindi scegliere **Proprietà**.  
  
4.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
5.  Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**Digitare **10.0.0.254**. In **subnet mask**Digitare **255.255.255.0**, quindi fare clic su **OK**.  
  
6.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
7.  Fare clic su **Usa il seguente indirizzo IPv6**. In **indirizzo IPv6**Digitare **2001: DB8:1:: Fe**. In **lunghezza prefisso subnet**Digitare **64**, quindi fare clic su **OK**.  
  
8.  Nella finestra di dialogo **Proprietà corpnet** fare clic su **Chiudi**.  
  
9. Nella finestra **connessioni di rete** fare clic con il pulsante destro del mouse sulla scheda di rete connessa a 2-corpnet, scegliere **Rinomina**, digitare **2-corpnet**e premere INVIO.  
  
10. Fare clic con il pulsante destro del mouse su **2-corpnet**, quindi scegliere **Proprietà**.  
  
11. Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
12. Fare clic su **Utilizza il seguente indirizzo IP**. In **indirizzo IP**Digitare **10.2.0.254**. In **subnet mask**Digitare **255.255.255.0**, quindi fare clic su **OK**.  
  
13. Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)** , quindi fare clic su **Proprietà**.  
  
14. Fare clic su **Usa il seguente indirizzo IPv6**. In **indirizzo IPv6**Digitare **2001: DB8:2:: Fe**. In **lunghezza prefisso subnet**Digitare **64**, quindi fare clic su **OK**.  
  
15. Nella finestra di dialogo **Proprietà 2-corpnet** fare clic su **Chiudi**.  
  
16. Chiudere la finestra **Connessioni di rete**.  
  
17. Nella console di Server Manager, in **server locale**, nell'area **Proprietà** , accanto a **nome computer**, fare clic sul collegamento.  
  
18. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
19. Nella finestra di dialogo **Cambiamenti dominio/nome computer** , in **nome computer**digitare **ROUTER1**, quindi fare clic su **OK**.  
  
20. Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
21. Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
22. Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
23. Dopo il riavvio del computer, effettuare l'accesso con l'account amministratore locale.  
  
## <a name="turn-off-the-firewall"></a>Spegnere il firewall  
Questo computer è configurato solo per fornire il routing tra le subnet Corpnet e 2-corpnet; il firewall deve pertanto essere disattivato.  
  
### <a name="to-turn-off-the-firewall"></a>Per disattivare il firewall  
  
1.  Nella schermata **Start** Digitare**WF. msc**, quindi premere INVIO.  
  
2.  In Windows Firewall con sicurezza avanzata, nel riquadro **azioni** fare clic su **Proprietà**.  
  
3.  Nella finestra di dialogo **Windows Firewall con sicurezza avanzata** , nella scheda **profilo di dominio** , in **stato firewall**, fare clic su **disattivato**.  
  
4.  Nella finestra di dialogo **Windows Firewall con sicurezza avanzata** , nella scheda **profilo privato** , in **stato firewall**, fare clic su **disattivato**.  
  
5.  Nella finestra di dialogo **Windows Firewall con sicurezza avanzata** , nella scheda **profilo pubblico** , in **stato firewall**, fare clic su **disattivata**, quindi fare clic su **OK**.  
  
6.  Chiudere Windows Firewall con protezione avanzata.  
  
## <a name="configure-routing-and-forwarding"></a>Configurare routing e inoltro  
Per fornire servizi di routing e inoltro tra le subnet Corpnet e 2-corpnet, è necessario abilitare l'inoltro sulle interfacce di rete e configurare le route statiche tra le subnet.  
  
### <a name="to-configure-static-routes"></a>Per configurare le route statiche  
  
1.  Nella schermata **Start** Digitare**cmd. exe**, quindi premere INVIO.  
  
2.  Abilitare l'inoltri nelle interfacce IPv4 e IPv6 di entrambe le schede di rete usando i comandi seguenti. Dopo l'immissione di ogni comando, premere INVIO.  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  Abilitare il routing IP-HTTPS tra le subnet Corpnet e 2-Corpnet.  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  Abilitare il routing Teredo tra le subnet Corpnet e 2-Corpnet.  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  Chiudere la finestra del prompt dei comandi.
