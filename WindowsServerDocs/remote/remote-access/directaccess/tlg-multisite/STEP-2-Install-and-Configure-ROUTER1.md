---
title: PASSAGGIO 2 di installare e configurare ROUTER1
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
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a771b5eb8587d23bc67a7e7769264251afdb5bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838512"
---
# <a name="step-2-install-and-configure-router1"></a>PASSAGGIO 2 di installare e configurare ROUTER1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questa Guida al lab di test multisito, il computer router costituisce un ponte IPv4 e IPv6 tra le subnet Corpnet e 2-Corpnet e agisce come un router per il traffico Teredo e IP-HTTPS.  
  
- Installare il sistema operativo in ROUTER1 
  
- Configurare le proprietà TCP/IP e rinominare il computer  
  
- Disattivare il firewall
  
- Configurare il routing e inoltro
  
## <a name="install-the-operating-system-on-router1"></a>Installare il sistema operativo in ROUTER1  
In primo luogo, installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-router1"></a>Per installare il sistema operativo in ROUTER1  
  
1.  Avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa).  
  
2.  Attenersi alle istruzioni per completare l'installazione, specificando una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettersi ROUTER1 a una rete che abbia accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e quindi disconnettersi da Internet.  
  
4.  Connettere ROUTER1 alle subnet Corpnet e 2-Corpnet.  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configurare le proprietà TCP/IP e rinominare il computer  
Configurare le impostazioni TCP/IP sul router e rinominare il computer per ROUTER1.  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Configurare le proprietà TCP/IP e rinominare il computer  
  
1.  Nella console di Server Manager, fare clic su **Server locale**e quindi il **delle proprietà** area, accanto a **connessione Ethernet cablata**, fare clic sul collegamento.  
  
2.  Nel **connessioni di rete** finestra, fare clic su scheda di rete è connessa alla rete aziendale, fare clic su **rinominare**, digitare **Corpnet**, e premere INVIO.  
  
3.  Fare doppio clic su **Corpnet**, quindi fare clic su **proprietà**.  
  
4.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
5.  Fare clic su **Utilizza il seguente indirizzo IP**. Nelle **indirizzo IP**, digitare **10.0.0.254**. Nelle **Subnet mask**, digitare **255.255.255.0**, quindi fare clic su **OK**.  
  
6.  Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)**, quindi fare clic su **Proprietà**.  
  
7.  Fare clic su **Usa l'indirizzo IPv6 seguente**. Nelle **indirizzo IPv6**, digitare **2001:db8:1::fe**. Nelle **lunghezza prefisso Subnet**, digitare **64**, quindi fare clic su **OK**.  
  
8.  Nel **proprietà Corpnet** fare clic su finestra di dialogo **Chiudi**.  
  
9. Nel **connessioni di rete** finestra, fare clic su scheda di rete è connessa alla rete aziendale di 2, fare clic su **rinominare**, digitare **2-Corpnet**, e premere INVIO.  
  
10. Fare doppio clic su **2-Corpnet**, quindi fare clic su **proprietà**.  
  
11. Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
12. Fare clic su **Utilizza il seguente indirizzo IP**. Nelle **indirizzo IP**, digitare **10.2.0.254**. Nelle **Subnet mask**, digitare **255.255.255.0**, quindi fare clic su **OK**.  
  
13. Fare clic su **Protocollo Internet versione 6 (TCP/IPv6)**, quindi fare clic su **Proprietà**.  
  
14. Fare clic su **Usa l'indirizzo IPv6 seguente**. Nelle **indirizzo IPv6**, digitare **2001:db8:2::fe**. Nelle **lunghezza prefisso Subnet**, digitare **64**, quindi fare clic su **OK**.  
  
15. Nel **proprietà 2-Corpnet** fare clic su finestra di dialogo **Chiudi**.  
  
16. Chiudere la finestra **Connessioni di rete**.  
  
17. Nella console di Server Manager, in **Server locale**, nella **delle proprietà** area, accanto a **nome Computer**, fare clic sul collegamento.  
  
18. Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
19. Nel **cambiamenti dominio/nome Computer** nella finestra di dialogo **nome Computer**, tipo **ROUTER1**, quindi fare clic su **OK**.  
  
20. Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
21. Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
22. Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
23. Dopo il riavvio del computer, accedere con l'account amministratore locale.  
  
## <a name="turn-off-the-firewall"></a>Disattivare il firewall  
In questo computer è configurato solo per fornire routing tra le subnet Corpnet e 2-Corpnet; Pertanto, il firewall deve essere spenta.  
  
### <a name="to-turn-off-the-firewall"></a>Per disattivare il firewall  
  
1.  Nel **avviare** digitare**WF. msc**, quindi premere INVIO.  
  
2.  In Windows Firewall con sicurezza avanzata, nella **azioni** riquadro, fare clic su **proprietà**.  
  
3.  Nel **Windows Firewall con sicurezza avanzata** finestra di dialogo il **profilo di dominio** nella scheda **dello stato del Firewall**, fare clic su **Off**.  
  
4.  Nel **Windows Firewall con sicurezza avanzata** finestra di dialogo il **profilo privato** nella scheda **dello stato del Firewall**, fare clic su **Off**.  
  
5.  Nel **Windows Firewall con sicurezza avanzata** finestra di dialogo il **profilo pubblico** nella scheda **dello stato del Firewall**, fare clic su **Off**e quindi Fare clic su **OK**.  
  
6.  Chiudere Windows Firewall con sicurezza avanzata.  
  
## <a name="configure-routing-and-forwarding"></a>Configurare il routing e inoltro  
Per fornire routing e inoltro di servizi tra le subnet Corpnet e 2-Corpnet, è necessario abilitare l'inoltro sulle interfacce di rete e configurare le route statiche tra le subnet.  
  
### <a name="to-configure-static-routes"></a>Per configurare le route statiche  
  
1.  Nel **avviare** digitare**cmd.exe**, quindi premere INVIO.  
  
2.  Abilitare l'inoltro sulle interfacce di IPv4 e IPv6 di entrambe le schede di rete usando i comandi seguenti. Dopo aver immesso tutti i comandi, premere INVIO.  
  
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
  
4.  Abilitare Teredo routing tra le subnet Corpnet e 2-Corpnet.  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  Chiudere la finestra del prompt dei comandi.
