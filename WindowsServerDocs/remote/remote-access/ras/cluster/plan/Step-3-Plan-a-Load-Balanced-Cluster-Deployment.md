---
title: Passaggio 3 pianificare una distribuzione di Cluster con bilanciamento del carico
description: Questo argomento fa parte della Guida deploy Remote Access in a cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: pashort
author: shortpatti
ms.openlocfilehash: beb2f5ce27115bf328917e38910198794f523547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404604"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>Passaggio 3 pianificare una distribuzione di Cluster con bilanciamento del carico

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Il passaggio successivo consiste nel pianificare la configurazione di bilanciamento del carico e la distribuzione di cluster.  
  
|Attività|Descrizione|  
|----|--------|  
|3.1 pianificare il bilanciamento del carico|Decidere se usare Windows carico bilanciamento rete (NLB) o un servizio di bilanciamento del carico esterno (ELB).|  
|3.2 piano IP-HTTPS|Se non viene utilizzato un certificato autofirmato, il server di accesso remoto richiede un certificato SSL in ogni server del cluster, per autenticare le connessioni IP-HTTPS.|  
|3.3 piano per le connessioni client VPN|Tenere presente i requisiti per le connessioni client VPN.|  
|3.4 pianificare il server dei percorsi di rete|Server dei percorsi di rete è ospitato nel server di accesso remoto, se non viene utilizzato un certificato autofirmato, assicurarsi che ogni server del cluster dispone di un certificato server per autenticare la connessione al sito Web.|  
  
## <a name="bkmk_2_1_Plan_LB"></a>3,1 pianificare il bilanciamento del carico  
Accesso remoto può essere distribuito in un singolo server o in un cluster di server di accesso remoto. Il traffico al cluster può essere con carico bilanciato per fornire elevata disponibilità e scalabilità per i client DirectAccess. Sono disponibili due opzioni di bilanciamento del carico:  
  
-   **Bilanciamento carico di RETE di Windows**-Bilanciamento carico di RETE di Windows è una funzionalità di Windows server. Per utilizzarlo, non richiesto hardware aggiuntivo perché tutti i server del cluster sono responsabili della gestione il carico del traffico. Bilanciamento carico di RETE di Windows supporta un massimo di otto server in un cluster di accesso remoto.  
  
-   **Bilanciamento del carico esterno**-utilizza un bilanciamento del carico esterno richiede hardware esterno per gestire il carico del traffico tra i server di cluster di accesso remoto. Inoltre, utilizza un bilanciamento del carico esterno supporta un massimo di 32 server di accesso remoto in un cluster. Alcuni punti da tenere presenti durante la configurazione di bilanciamento del carico esterno sono:  
  
    -   L'amministratore deve verificare che gli indirizzi IP virtuali configurati tramite la procedura guidata di bilanciamento del carico di accesso remoto vengono utilizzati in servizi di bilanciamento del carico esterno (ad esempio sistema F5 Big-Ip locale Traffic Manager). Quando il bilanciamento del carico esterno è abilitata, gli indirizzi IP nelle interfacce interne ed esterne verranno trasformati in indirizzi IP virtuali e devono essere eseguito il plumbing nel bilanciamento del carico. Questa operazione viene eseguita in modo che l'amministratore non è necessario modificare la voce DNS per il nome pubblico della distribuzione del cluster. Inoltre, gli endpoint del tunnel IPsec sono derivati dal server di indirizzi IP. Se l'amministratore a fornire indirizzi IP virtuali separate, il client non sarà in grado di connettersi al server. Vedere l'esempio per la configurazione di DirectAccess con bilanciamento del carico esterno in 3.1.1 esempio di configurazione di bilanciamento del carico esterno.  
  
    -   Molti bilanciamenti del carico esterno (incluso F5) non supportano il bilanciamento del carico di 6to4 e ISATAP. Se il server di accesso remoto è un router ISATAP, la funzione ISATAP deve essere spostata in un altro computer. Una volta funzione ISATAP in un computer diverso, il server DirectAccess deve inoltre disporre di connettività IPv6 nativa con il router ISATAP. Si noti che la connettività deve essere presente prima della configurazione di DirectAccess.  
  
    -   Per il bilanciamento del carico esterno, se Teredo è necessario utilizzare, tutti i server di accesso remoto devono disporre due indirizzi IPv4 pubblici consecutivi come indirizzi IP dedicati. Indirizzi IP virtuali del cluster deve inoltre disporre di due indirizzi IPv4 pubblici consecutivi. Questo non è valido per Bilanciamento carico di RETE di Windows in cui solo indirizzi IP virtuali del cluster deve avere due indirizzi IPv4 pubblici consecutivi. Se non viene usato Teredo, non sono necessari due indirizzi IP consecutivi.  
  
    -   L'amministratore può passare da Bilanciamento carico di RETE di Windows a bilanciamento del carico esterno e viceversa. Si noti che l'amministratore non può passare dal bilanciamento del carico esterno bilanciamento carico di rete di Windows se ha più di 8 server nella distribuzione del bilanciamento del carico esterno.  
  
### <a name="ELBConfigEx"></a>3.1.1 esempio di configurazione di Load Balancer esterno  
In questa sezione vengono descritti i passaggi di configurazione per l'attivazione di un servizio di bilanciamento del carico esterno una nuova distribuzione di accesso remoto. Quando si utilizza un servizio di bilanciamento del carico esterno, il cluster di accesso remoto potrebbe essere simile nella figura seguente, in cui i server di accesso remoto sono connessi alla rete aziendale tramite un servizio di bilanciamento del carico sulla rete interna e a Internet tramite un bilanciamento del carico connesso alla rete esterna:  
  
![Esempio di configurazione di bilanciamento del carico esterno](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)  
  
##### <a name="planning-information"></a>Informazioni sulla pianificazione  
  
1.  Gli indirizzi VIP esterni (indirizzi IP che il client utilizzerà per connettersi a Remote Access) sono stati deciso di 131.107.0.102, 131.107.0.103  
  
2.  Bilanciamento del carico nella rete esterna self-IP - 131.107.0.245 (Internet), 131.107.1.245  
  
    Rete perimetrale (detta anche DMZ e DMZ) è compreso tra il server di accesso remoto e il bilanciamento del carico nella rete esterna.  
  
3.  Indirizzi IP per il server di accesso remoto nella rete perimetrale - 131.107.1.102, 131.107.1.103  
  
4.  Gli indirizzi IP per il server di accesso remoto in rete (ad esempio tra il server di accesso remoto e il bilanciamento del carico sulla rete interna) - ELB 30.11.1.101, 2006:2005:11:1::101  
  
5.  Bilanciamento del carico nella rete interna self-IP - 30.11.1.245 2006:2005:11:1::245 (ELB), 30.1.1.245 2006:2005:1:1::245 (aziendale)  
  
6.  Indirizzi IP virtuali interni (indirizzi IP utilizzati per probe di client web e per server dei percorsi di rete, se installato nel server di accesso remoto) sono stati deciso di 30.1.1.10, 2006:2005:1:1::10  
  
##### <a name="steps"></a>Passaggi  
  
1.  Configurare l'adattatore di rete esterna del server di accesso remoto (che è connesso alla rete perimetrale) con indirizzi 131.107.0.102, 131.107.0.103. Questo passaggio è obbligatorio per la configurazione di DirectAccess rilevare gli endpoint del tunnel IPsec corretti.  
  
2.  Configurare l'adattatore di rete interna del server di accesso remoto (che è connesso alla rete ELB) con gli indirizzi IP di server web-probe/di rete (30.1.1.10, 2006:2005:1:1::10). Questo passaggio è necessario per consentire ai client di accesso IP probe web, l'Assistente connettività di rete indica correttamente lo stato della connessione a DirectAccess. Questo passaggio consente inoltre l'accesso al server dei percorsi di rete, se è configurato nel server DirectAccess.  
  
    > [!NOTE]  
    > Assicurarsi che il controller di dominio sia raggiungibile dal server di accesso remoto con questa configurazione.  
  
3.  Configurare server singolo di DirectAccess nel server di accesso remoto.  
  
4.  Abilitare Bilanciamento esterno nella configurazione di DirectAccess. Utilizzare 131.107.1.102 come esterno indirizzo IP dedicato (DIP) (131.107.1.103 verrà selezionato automaticamente), utilizzare 30.11.1.101, 2006:2005:11:1::101 come il DIP interno.  
  
5.  Configurare l'IP virtuale esterno (VIP, Virtual Private Network) sul bilanciamento del carico esterno con indirizzi 131.107.0.102 e 131.107.0.103. Inoltre, configurare l'interno VIP sul bilanciamento del carico esterno con indirizzi 30.1.1.10 e 2006:2005:1:1::10.  
  
6.  Ora il server di accesso remoto verrà configurato con gli indirizzi IP pianificati e gli indirizzi IP interni ed esterni per il cluster verranno configurati in base a indirizzi IP pianificati.  
  
## <a name="bkmk_2_2_NLB"></a>3,2 piano IP-HTTPS  
  
1.  **Requisiti dei certificati**-durante la distribuzione del singolo server di accesso remoto si è scelto di utilizzare un certificato IP-HTTPS emesso da un'autorità di certificazione interna o pubblica (CA) o un certificato autofirmato. Per la distribuzione di cluster, è necessario utilizzare stesso tipo di certificato in ogni membro del cluster di accesso remoto. Ovvero, se si utilizza un certificato emesso da un'autorità di certificazione pubblica (scelta consigliata), è necessario installare un certificato emesso da un'autorità di certificazione pubblica ogni membro del cluster. Il nome del soggetto del nuovo certificato deve essere identico al nome del soggetto del certificato IP-HTTPS attualmente utilizzato nella distribuzione. Si noti che se si utilizzano certificati autofirmati questi verranno configurati automaticamente in ogni server durante la distribuzione del cluster.  
  
2.  **Requisiti del prefisso**-accesso remoto consente il bilanciamento del carico di traffico basata su SSL e il traffico DirectAccess. Bilanciare il traffico DirectAccess basato su IPv6 tutti, accesso remoto è necessario esaminare il tunneling IPv4 per tutte le tecnologie di transizione. Poiché il traffico IP-HTTPS è crittografato, esaminare il contenuto del tunnel IPv4 non è possibile. Per consentire il traffico IP-HTTPS il bilanciamento del carico, è necessario allocare un prefisso IPv6 sufficiente in modo che un diverso prefisso IPv6 da/64 può essere assegnato a ogni membro del cluster. È possibile configurare un massimo di 32 server in un cluster con bilanciamento di carico; Pertanto, è necessario specificare un /59 prefisso. Questo prefisso deve essere instradabile nell'indirizzo IPv6 interno del cluster di accesso remoto e viene configurato nella procedura guidata configurazione Server di accesso remoto.  
  
    > [!NOTE]  
    > I requisiti di prefisso sono rilevanti solo in una rete interna IPv6 abilitato (solo IPv6 o IPV4 + IPv6). In una rete aziendale solo IPv4, il prefisso del client viene configurato automaticamente e l'amministratore può essere modificato.  
  
## <a name="BKMK_3.3"></a>piano 3,3 per le connessioni client VPN  
Esistono una serie di considerazioni per le connessioni client VPN:  
  
-   Il traffico client VPN non può essere con carico bilanciato se gli indirizzi client VPN vengono allocati utilizzando DHCP. È necessario un pool di indirizzi statici.  
  
-   RRAS può essere attivata in un cluster con bilanciamento del carico che è stato distribuito, solo per DirectAccess utilizzando **Abilita VPN** nel riquadro attività della console di gestione accesso remoto.  
  
-   Modifiche VPN completate nella console di gestione accesso remoto e Routing (rrasmgmt.msc) dovranno essere replicate manualmente su tutti i server di accesso remoto nel cluster.  
  
-   Per consentire il traffico client VPN IPv6 il bilanciamento del carico, è necessario specificare un prefisso IPv6 bit 59.  
  
## <a name="BKMK_nls"></a>3,4 pianificare il server dei percorsi di rete  
Se si eseguono server dei percorsi di rete nel server di accesso remoto singolo, durante la distribuzione è scelto di utilizzare un certificato rilasciato da un'autorità di certificazione interna (CA) o un certificato autofirmato.  Tenere presente quanto segue:  
  
1.  Ogni membro del cluster di accesso remoto deve disporre di un certificato per il server di percorso di rete che corrisponde alla voce DNS per server dei percorsi di rete.  
  
2.  Il certificato per ogni server del cluster deve essere emesso nello stesso modo del certificato sul singolo accesso remoto server corrente rete percorso certificato del server. Ad esempio, se si utilizza un certificato emesso da una CA interna, è necessario installare un certificato rilasciato dalla CA interna di ogni membro del cluster.  
  
3.  Se si utilizza un certificato autofirmato, un certificato autofirmato verrà configurato automaticamente per ogni server durante la distribuzione del cluster.  
  
4.  Il nome del soggetto del certificato non deve essere identico al nome di un server nella distribuzione di accesso remoto.  
