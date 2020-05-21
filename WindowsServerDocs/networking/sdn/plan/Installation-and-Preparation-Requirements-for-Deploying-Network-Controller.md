---
title: Requisiti per la distribuzione del controller di rete
description: Preparare il Data Center per la distribuzione del controller di rete, che richiede uno o più computer o macchine virtuali e un computer o una macchina virtuale. Prima di poter distribuire il controller di rete, è necessario configurare i gruppi di sicurezza, i percorsi dei file di log (se necessario) e la registrazione DNS dinamica.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/10/2018
ms.openlocfilehash: f5a7ec331c9d70214cbd0a772de6e2b2c7f4f58e
ms.sourcegitcommit: 7116460855701eed4e09d615693efa4fffc40006
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/15/2020
ms.locfileid: "83433175"
---
# <a name="requirements-for-deploying-network-controller"></a>Requisiti per la distribuzione del controller di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Preparare il Data Center per la distribuzione del controller di rete, che richiede uno o più computer o macchine virtuali e un computer o una macchina virtuale. Prima di poter distribuire il controller di rete, è necessario configurare i gruppi di sicurezza, i percorsi dei file di log (se necessario) e la registrazione DNS dinamica.


## <a name="network-controller-requirements"></a>Requisiti del controller di rete

La distribuzione del controller di rete richiede uno o più computer o macchine virtuali che funge da controller di rete e un computer o una macchina virtuale che funge da client di gestione per il controller di rete. 

- Tutte le macchine virtuali e i computer pianificati come nodi del controller di rete devono eseguire Windows Server 2016 Datacenter Edition. 
- Qualsiasi computer o macchina virtuale su cui si installa il controller di rete deve eseguire Datacenter Edition di Windows Server 2016. 
- Il computer client di gestione o la macchina virtuale per il controller di rete deve eseguire Windows 10. 


## <a name="configuration-requirements"></a>Requisiti di configurazione

Prima di distribuire il controller di rete, è necessario configurare i gruppi di sicurezza, i percorsi dei file di log (se necessario) e la registrazione DNS dinamica.

### <a name="step-1-configure-your-security-groups"></a>Passaggio 1. Configurare i gruppi di sicurezza

Per prima cosa è necessario creare due gruppi di sicurezza per l'autenticazione Kerberos. 

È possibile creare gruppi per gli utenti che dispongono dell'autorizzazione per: 

1. Configurare il controller di rete<p>È possibile assegnare a questo gruppo gli amministratori del controller di rete, ad esempio. 
2.  Configurare e gestire la rete tramite il controller di rete<p>È ad esempio possibile assegnare a questo gruppo gli utenti del controller di rete. Utilizzare REST (Representational State Transfer) per configurare e gestire il controller di rete.

>[!NOTE]
>Tutti gli utenti aggiunti devono essere membri del gruppo Domain Users in Active Directory Users and Computers.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Passaggio 2. Configurare i percorsi del file di registro, se necessario

L'operazione successiva consiste nel configurare i percorsi dei file per archiviare i log di debug del controller di rete nel computer del controller di rete o nella macchina virtuale o in una condivisione file remota. 

>[!NOTE]
>Se si archiviano i log in una condivisione file remota, assicurarsi che la condivisione sia accessibile dal controller di rete.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Passaggio 3. Configurare la registrazione DNS dinamica per il controller di rete

Infine, la prossima cosa da fare è distribuire i nodi del cluster del controller di rete nella stessa subnet o in subnet diverse. 


|         Se...         |                                                                                                                                                         Quindi...                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Nella stessa subnet,  |                                                                                                                                È necessario specificare l'indirizzo IP REST del controller di rete.                                                                                                                                 |
| In subnet diverse | È necessario specificare il nome DNS REST del controller di rete creato durante il processo di distribuzione. È inoltre necessario eseguire le operazioni seguenti:<ul><li>Configurare gli aggiornamenti dinamici DNS per il nome DNS del controller di rete nel server DNS.</li><li>Limitare gli aggiornamenti dinamici DNS solo ai nodi del controller di rete.</li></ul> |

---

> [!NOTE]
> Per eseguire queste procedure, è necessaria almeno l'appartenenza al gruppo **Domain Admins**o a un gruppo equivalente.

1. Consente gli aggiornamenti dinamici DNS per una zona.

   a. Aprire Gestore DNS, quindi nell'albero della console fare clic con il pulsante destro del mouse sulla zona applicabile, quindi scegliere **Proprietà**. 

   b. Nella scheda **generale** verificare che il tipo di zona sia **primario** o integrato in **Active Directory**.

   c. In **aggiornamenti dinamici**verificare che sia selezionata l'opzione **solo protetto** , quindi fare clic su **OK**.

2. Configurare le autorizzazioni di sicurezza per la zona DNS per i nodi del controller di rete

   a.  Fare clic sulla scheda **Sicurezza** e quindi su **Avanzate**. 

   b. In **impostazioni di sicurezza avanzate**fare clic su **Aggiungi**. 

   c. Fare clic su **Seleziona un'entità**. 

   d. Nella finestra di dialogo **Seleziona utente, computer, account servizio o gruppo** fare clic su **tipi di oggetto**. 

   e. In **tipi di oggetto**selezionare **computer**e quindi fare clic su **OK**.

   f. Nella finestra di dialogo **Seleziona utente, computer, account servizio o gruppo** Digitare il nome NetBIOS di uno dei nodi del controller di rete nella distribuzione, quindi fare clic su **OK**.

   g. In **voce autorizzazione**verificare i valori seguenti:

      - **Tipo** = Consenti
      - **Si applica a** = questo oggetto e tutti gli oggetti discendenti

   h. In **autorizzazioni**selezionare **Scrivi tutte le proprietà** ed **Elimina**, quindi fare clic su **OK**.

3. Ripetere la ripetizione per tutti i computer e le macchine virtuali nel cluster di controller di rete.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Passaggio 4. Configurare il nome dell'entità servizio se si usa l'autenticazione basata su Kerberos

Se il controller di rete utilizza l'autenticazione basata su Kerberos per la comunicazione con i client di gestione, è necessario configurare un nome dell'entità servizio (SPN) per il controller di rete in Active Directory. Il controller di rete configura automaticamente il nome SPN. È sufficiente fornire le autorizzazioni per la registrazione e la modifica del nome SPN da parte dei computer del controller di rete. Per altri dettagli, vedere [configurare i nomi dell'entità servizio (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Opzioni di distribuzione

### <a name="network-controller-deployment"></a>Distribuzione del controller di rete

L'installazione è a disponibilità elevata con tre nodi del controller di rete configurati nelle macchine virtuali. Viene anche illustrato come due tenant con rete virtuale tenant 2 suddivisi in due subnet virtuali per simulare un livello Web e un livello database.  

![Pianificazione di SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Distribuzione del controller di rete e del servizio di bilanciamento del carico software

Per disponibilità elevati sono presenti due o più nodi SLB/MUX.

![Pianificazione di SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Distribuzione di controller di rete, Load Balancer software e gateway RAS

Sono disponibili tre macchine virtuali del gateway. due sono attivi e uno è ridondante.

![Pianificazione di SDN NC](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  

>[!IMPORTANT] 
>Se si esegue la distribuzione tramite VMM, assicurarsi che le macchine virtuali dell'infrastruttura (server VMM, AD/DNS, SQL Server e così via) non siano ospitate in nessuno dei quattro host visualizzati nei diagrammi.  


## <a name="next-steps"></a>Passaggi successivi
[Pianificare un'infrastruttura di rete definita dal software](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Argomenti correlati
- [Controller di rete](../technologies/network-controller/Network-Controller.md) 
- [Disponibilità elevata del controller di rete](../technologies/network-controller/network-controller-high-availability.md) 
- [Distribuire controller di rete tramite Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Installare il ruolo del server del controller di rete con Server Manager](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
