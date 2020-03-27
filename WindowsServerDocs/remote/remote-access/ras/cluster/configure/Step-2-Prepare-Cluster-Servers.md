---
title: Passaggio 2 preparare i server Cluster
description: Questo argomento fa parte della Guida deploy Remote Access in a cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 74aac416a5aa69a0cd935d58e3ecb931e4b5fd02
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308331"
---
# <a name="step-2-prepare-cluster-servers"></a>Passaggio 2 preparare i server Cluster

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Prima di poter configurare una distribuzione in cluster, è preparare i server aggiuntivi da aggiungere al cluster.  
  
|Attività|Descrizione|  
|----|--------|  
|[2,1 configurare l'infrastruttura di accesso remoto](#BKMK_config)|In ogni server che si desidera aggiungere al cluster, configurare la topologia di server, gli indirizzi IP, routing e inoltro. Se si configura un cluster con bilanciamento del carico delle macchine virtuali, è necessario configurare le macchine virtuali per l'utilizzo di spoofing degli indirizzi MAC.<br /><br />Inoltre, aggiungere ogni server allo stesso dominio e connettere tutti i server alla stessa subnet.|  
|[2,2 installare il ruolo accesso remoto](#BKMK_Install)|In ogni server aggiuntivo che si desidera aggiungere al cluster, installare il ruolo Accesso remoto|  
|[2,3 installare NLB](#BKMK_NLB)|Nel server di accesso remoto distribuito e in ogni server aggiuntivo che si desidera aggiungere al cluster, installare la funzionalità Bilanciamento carico di RETE. Si noti che questo passaggio non è obbligatorio quando si utilizza un bilanciamento del carico esterno.|  
  
## <a name="21-configure-the-remote-access-infrastructure"></a><a name="BKMK_config"></a>2,1 configurare l'infrastruttura di accesso remoto  
Per configurare un cluster di accesso remoto, è necessario configurare la topologia di server, gli indirizzi IP, routing e inoltro in tutti i server facenti parte del cluster.  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>Per configurare l'infrastruttura di accesso remoto  
  
1.  Configurare ogni server che faranno parte del cluster con la stessa topologia il primo server di accesso remoto.  
  
2.  Configurare ogni server che faranno parte del cluster appropriato IP indirizzi, routing e inoltro in base alla configurazione del primo server di accesso remoto. Si noti che tutti i server del cluster devono essere connesso alla stessa subnet.  
  
3.  Aggiungere ogni server che faranno parte del cluster allo stesso dominio del primo server di accesso remoto.  
  
## <a name="22-install-the-remote-access-role"></a><a name="BKMK_Install"></a>2,2 installare il ruolo accesso remoto  
Per configurare un cluster di accesso remoto, è necessario installare il ruolo Accesso remoto in ogni server che fa parte del cluster.  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Per installare il ruolo accesso remoto nei server VPN Always On  
  
1.  Nel server DirectAccess, nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** per tre volte per visualizzare la schermata di selezione del ruolo server.  
  
3.  Nel **Selezione ruoli Server** finestra di dialogo, selezionare **accesso remoto**, quindi fare clic su **Avanti**.  
  
4.  Fare clic su **Avanti** tre volte.  
  
5.  Nel **Selezione servizi ruolo** finestra di dialogo, selezionare **DirectAccess e VPN (RAS)** e quindi fare clic su **Aggiungi funzionalità**.  
  
6.  Selezionare **Routing**, selezionare **Proxy applicazione Web**, fare clic su **Aggiungi funzionalità**, quindi fare clic su **Avanti**.  
  
7. Fare clic su **Avanti** e quindi su **Installa**.  
  
8.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione abbia avuto esito positivo e quindi fare clic su **Chiudi**.  
  
9.  Ripetere questa procedura su tutti i server che si desidera essere membri del cluster.  
  
## <a name="23-install-nlb"></a><a name="BKMK_NLB"></a>2,3 installare NLB  
Per configurare un cluster di accesso remoto, è necessario installare la funzionalità Bilanciamento carico di rete in ogni server che fa parte del cluster.  
  
> [!NOTE]  
> Questo passaggio non è obbligatorio se viene utilizzato un servizio di bilanciamento del carico esterno.  
  
#### <a name="to-install-the-nlb-role"></a>Per installare il ruolo di bilanciamento carico di RETE  
  
1.  Nel server DirectAccess, nella console di Server Manager, nel **Dashboard**, fare clic su **Aggiungi ruoli e funzionalità**.  
  
2.  Fare clic su **Avanti** quattro volte per visualizzare la schermata di selezione delle funzionalità server.  
  
3.  Nel **Selezionare le funzionalità** finestra di dialogo, selezionare **Bilanciamento carico di rete**, fare clic su **Aggiungi funzionalità**, fare clic su **Avanti**, quindi fare clic su **installare**.  
  
4.  Nella finestra di dialogo **Stato installazione** verificare che l'installazione abbia avuto esito positivo e quindi fare clic su **Chiudi**.  
  
5.  Ripetere questa procedura su tutti i server che si desidera essere membri del cluster.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  
  
-   [Passaggio 3: configurare un cluster con carico bilanciato](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  


