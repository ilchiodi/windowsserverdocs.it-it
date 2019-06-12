---
title: Requisiti per la distribuzione di Controller di rete
description: Preparare il tuo Data Center per la distribuzione del Controller di rete, che richiede uno o più computer o macchine virtuali e un solo computer o macchina virtuale. Prima di distribuire Controller di rete, è necessario configurare i gruppi di sicurezza, percorsi dei file di log (se necessario) e la registrazione dinamica DNS.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: 51ba991397a7c35ee0198f8e75c67b2f99b7c7bc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446312"
---
# <a name="requirements-for-deploying-network-controller"></a>Requisiti per la distribuzione di Controller di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Preparare il tuo Data Center per la distribuzione del Controller di rete, che richiede uno o più computer o macchine virtuali e un solo computer o macchina virtuale. Prima di distribuire Controller di rete, è necessario configurare i gruppi di sicurezza, percorsi dei file di log (se necessario) e la registrazione dinamica DNS.


## <a name="network-controller-requirements"></a>Requisiti del Controller di rete

Distribuzione del Controller di rete richiede uno o più computer o macchine virtuali che fungono dal Controller di rete e un computer o macchina virtuale da usare come un client di gestione per il Controller di rete. 

- Tutte le macchine virtuali e computer pianificato come nodi di Controller di rete deve essere in esecuzione Windows Server 2016 Datacenter edition. 
- Qualsiasi computer o macchina virtuale (VM) su cui si installa il Controller di rete deve essere in esecuzione l'edizione Datacenter di Windows Server 2016. 
- Gestione client computer o macchina virtuale per il Controller di rete deve essere in esecuzione Windows 10. 


## <a name="configuration-requirements"></a>Requisiti di configurazione

Prima di distribuire Controller di rete, è necessario configurare i gruppi di sicurezza, percorsi dei file di log (se necessario) e la registrazione dinamica DNS.

### <a name="step-1-configure-your-security-groups"></a>Passaggio 1. Configurare i gruppi di sicurezza

La prima operazione da eseguire è creare due gruppi di sicurezza per l'autenticazione Kerberos. 

Creare gruppi per gli utenti che dispongono dell'autorizzazione per: 

1. Configurare Controller di rete<p>È possibile denominare questo gruppo di amministratori del Controller di rete, ad esempio. 
2.  Configurare e gestire la rete dal controller di rete<p>Ad esempio, è possibile denominare questo gruppo Users, Controller di rete. Usare Representational State Transfer (REST) per configurare e gestire i Controller di rete.

>[!NOTE]
>Tutti gli utenti che si aggiungono devono essere membri del gruppo Domain Users in Active Directory Users and Computers.

### <a name="step-2-configure-log-file-locations-if-needed"></a>Passaggio 2. Configurare i percorsi dei file di log se necessario

La prossima cosa che si vuole fare è configurare i percorsi dei file per archiviare i log di debug di Controller di rete nel computer Controller di rete o macchina virtuale o in una condivisione file remota. 

>[!NOTE]
>Se si archiviano i log in una condivisione file remota, assicurarsi che la condivisione sia accessibile dal Controller di rete.


### <a name="step-3-configure-dynamic-dns-registration-for-network-controller"></a>Passaggio 3. Configurare la registrazione dinamica DNS per il Controller di rete

Infine, l'operazione successiva da eseguire è distribuire nodi di cluster di Controller di rete nella stessa subnet o subnet diverse. 


|         Se...         |                                                                                                                                                         Quindi...                                                                                                                                                         |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Nella stessa subnet,  |                                                                                                                                È necessario fornire l'indirizzo IP REST Controller di rete.                                                                                                                                 |
| In subnet diverse, | È necessario fornire il nome DNS REST Controller di rete, che si crea durante il processo di distribuzione. È anche necessario eseguire quanto segue:<ul><li>Configurare gli aggiornamenti dinamici DNS per il nome DNS di Controller di rete nel server DNS.</li><li>Limitare gli aggiornamenti dinamici DNS verso solo i nodi di Controller di rete.</li></ul> |

---

> [!NOTE]
> L'appartenenza al **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire queste procedure.

1. Consentire gli aggiornamenti dinamici DNS per una zona.

   a. Aprire Gestore DNS e nell'albero della console, fare doppio clic sulla zona e quindi fare clic su **proprietà**. 

   b. Nel **generale** scheda, verificare che il tipo di zona sia impostato **primario** oppure **integrate in Active Directory**.

   c. Nelle **gli aggiornamenti dinamici**, verificare che **proteggere solo** sia selezionata e quindi fare clic su **OK**.

2. Configurare le autorizzazioni di sicurezza di zona DNS per i nodi di Controller di rete

   a.  Fare clic sulla scheda **Sicurezza** e quindi su **Avanzate**. 

   b. Nelle **impostazioni di sicurezza avanzate**, fare clic su **Add**. 

   c. Fai clic su **Seleziona un'entità**. 

   d. Nel **Seleziona utenti, Computer, Account del servizio o gruppo** finestra di dialogo, fare clic su **i tipi di oggetto**. 

   e. Nelle **tipi di oggetti**, selezionare **computer**, quindi fare clic su **OK**.

   f. Nel **Seleziona utenti, Computer, Account del servizio o gruppo** della finestra di dialogo digitare il nome NetBIOS di uno dei nodi del Controller di rete nella distribuzione e quindi fare clic su **OK**.

   g. Nelle **voce di autorizzazione**, verificare i valori seguenti:

      - **Tipo** = consentire
      - **Si applica a** = questo oggetto e tutti i discendenti

   h. Nella **le autorizzazioni**, selezionare **Scrivi tutte le proprietà** e **Elimina**, quindi fare clic su **OK**.

3. Ripetere per tutti i computer e macchine virtuali del cluster di Controller di rete.

### <a name="step-4-configure-service-principal-name-if-using-kerberos-based-authentication"></a>Passaggio 4. Se l'autenticazione tramite Kerberos basata su configurare nome dell'entità servizio

Se il Controller di rete utilizza l'autenticazione basata su Kerberos per la comunicazione con i client di gestione, è necessario configurare un nome dell'entità servizio (SPN) per il Controller di rete in Active Directory. Il Controller di rete configura automaticamente il nome SPN. È sufficiente eseguire consiste nel fornire le autorizzazioni per le macchine di Controller di rete registrare e modificare il nome SPN. Per altre informazioni, vedere [configurare principale nomi servizio (SPN)](https://docs.microsoft.com/windows-server/networking/sdn/security/kerberos-with-spn#configure-service-principal-names-spn).

## <a name="deployment-options"></a>Opzioni di distribuzione

### <a name="network-controller-deployment"></a>Distribuzione del Controller di rete

Il programma di installazione è a disponibilità elevata con tre nodi di Controller di rete configurati in macchine virtuali. Viene inoltre illustrato due tenant con la rete virtuale del Tenant 2 suddivisa in due subnet virtuali per simulare un livello web e un livello di database.  

![Pianificazione di controller di rete SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Software e controller di rete di distribuzione del bilanciamento del carico

Per ottenere una disponibilità elevata, sono disponibili due o più nodi SLB/MUX.

![Pianificazione di controller di rete SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)

### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Distribuzione di Controller di rete, bilanciamento del carico Software e Gateway RAS

Esistono tre macchine virtuali gateway; due sono attivi e uno è ridondante.

![Pianificazione di controller di rete SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  



Automatizzare la distribuzione basata su TP5, Active Directory deve essere disponibile e raggiungibile da queste subnet. Per altre informazioni su Active Directory, vedere [Panoramica di Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).  

>[!IMPORTANT] 
>Se si distribuisce tramite VMM, verificare che le macchine virtuali dell'infrastruttura (Server VMM, AD/DNS, SQL Server, e così via) non sono ospitati in uno qualsiasi dei quattro host illustrato nei diagrammi.  


## <a name="next-steps"></a>Passaggi successivi
[Piano di un Software Defined Network Infrastructure](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).

## <a name="related-topics"></a>Argomenti correlati
- [Controller di rete](../technologies/network-controller/Network-Controller.md) 
- [Disponibilità elevata di Controller di rete](../technologies/network-controller/network-controller-high-availability.md) 
- [Distribuire controller di rete tramite Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)   
- [Installare il ruolo del server del controller di rete con Server Manager](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)   
