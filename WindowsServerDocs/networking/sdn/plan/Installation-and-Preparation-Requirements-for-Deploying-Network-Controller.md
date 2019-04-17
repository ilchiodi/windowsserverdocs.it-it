---
title: Installazione e i requisiti di preparazione per la distribuzione di Controller di rete
description: Per preparare il Data Center per la distribuzione di Controller di rete, è possibile utilizzare questo argomento.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 7f899e62-6e5b-4fca-9a59-130d4766ee2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c50a2167a894871ea76fb96523c19531100648ce
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="installation-and-preparation-requirements-for-deploying-network-controller"></a>Installazione e i requisiti di preparazione per la distribuzione di Controller di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Per preparare il Data Center per la distribuzione di Controller di rete, è possibile utilizzare questo argomento.  
  
> [!NOTE]  
> Oltre a questo argomento, è disponibile la documentazione seguente Controller di rete.  
> 
> - [Controller di rete](../technologies/network-controller/Network-Controller.md)
> - [Disponibilità elevata del Controller di rete](../technologies/network-controller/network-controller-high-availability.md)
> - [Distribuire Controller di rete tramite Windows PowerShell](../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installare il ruolo server di Controller di rete con Server Manager](../technologies/network-controller/Install-the-Network-Controller-server-role-using-Server-Manager.md)  

Di seguito sono i passaggi di installazione, e altri requisiti software e preparazione che è necessario eseguire prima di distribuire Controller di rete.

## <a name="installation-requirements"></a>Requisiti di installazione

Di seguito sono i requisiti di installazione per il Controller di rete.

- Per le distribuzioni di Windows Server 2016, è possibile distribuire Controller di rete in uno o più computer, uno o più macchine virtuali o una combinazione di computer e le macchine virtuali. Tutti i computer pianificati come nodi di Controller di rete e le macchine virtuali deve essere in esecuzione Windows Server 2016 Datacenter edition.

## <a name="software-requirements"></a>Requisiti software

Distribuzione del Controller di rete richiede uno o più computer o macchine virtuali che verranno utilizzato come il Controller di rete e un computer o macchina virtuale da usare come un client di gestione per il Controller di rete. Questi computer o macchine virtuali è necessario eseguire i seguenti sistemi operativi.  

- Qualsiasi computer o macchina virtuale (VM) su cui si installa il Controller di rete deve essere in esecuzione l'edizione Datacenter di Windows Server 2016.  
  
- Il computer client di gestione o di una macchina virtuale per Controller di rete deve essere in esecuzione Windows 8, Windows 8.1 o Windows 10.  
  
## <a name="additional-requirements"></a>Requisiti aggiuntivi

Di seguito sono passaggi aggiuntivi, è necessario eseguire prima di distribuire Controller di rete.
  
### <a name="configure-security-groups"></a>Configurare gruppi di sicurezza
  
Se il computer o macchine virtuali per i Controller di rete e il client di gestione vengono aggiunti al dominio, configurare i seguenti gruppi di sicurezza per l'autenticazione Kerberos.

- Creare un gruppo di sicurezza e aggiungere tutti gli utenti che dispongono dell'autorizzazione per configurare i Controller di rete. Ad esempio, creare un gruppo denominato **gli amministratori di Controller di rete **. Anche tutti gli utenti aggiunti a questo gruppo devono essere membri del **Domain Users** gruppo in Active Directory Users and Computers.  
  
    > [!NOTE]  
    > Per ulteriori informazioni sulla creazione di un gruppo in Active Directory Users and Computers, vedere [creare un nuovo gruppo ](https://technet.microsoft.com/en-us/library/cc783256(v=ws.10).aspx).  

- Creare un gruppo di sicurezza e aggiungere tutti gli utenti che dispongono dell'autorizzazione per configurare e gestire la rete con i Controller di rete.  Ad esempio, creare un nuovo gruppo denominato **gli utenti di Controller di rete **. Anche tutti gli utenti aggiunti al nuovo gruppo devono essere membri del **Domain Users** gruppo in Active Directory Users and Computers. Tutti gestione e configurazione di Controller di rete viene eseguita utilizzando Representational State Transfer \(REST\).

### <a name="configure-log-file-locations-if-needed"></a>Configurare i percorsi di file registro se necessario

È possibile archiviare i registri di debug di Controller di rete nel computer Controller di rete o macchina virtuale, o in una condivisione file remota. Se si desidera archiviare i log in una condivisione file remota, assicurarsi che la condivisione sia accessibile dal Controller di rete.

### <a name="configure-dynamic-dns-registration-for-network-controller"></a>Configurare la registrazione DNS dinamica per Controller di rete
  
È possibile distribuire i nodi del cluster Controller di rete nella stessa subnet o in subnet diverse. 

>[!NOTE]
>Se i nodi di Controller di rete sono nella stessa subnet, è necessario fornire l'indirizzo IP di altri Controller di rete quando si configura la registrazione DNS dinamica per Controller di rete. Se i nodi in subnet diverse, è necessario fornire il nome DNS di altri Controller di rete quando si configura la registrazione DNS dinamica.

Se i nodi di Controller di rete in subnet diverse, è necessario eseguire la configurazione DNS aggiuntiva seguenti:

- Creare un nome DNS per i Controller di rete durante il processo di distribuzione

- Configurare gli aggiornamenti dinamici DNS per il nome DNS di Controller di rete nel server DNS

- Limitare gli aggiornamenti dinamici DNS solo i nodi di Controller di rete

È possibile utilizzare le procedure seguenti per configurare gli aggiornamenti dinamici DNS e di limitare l'aggiornamento dinamico del record di nome Controller di rete.

> [!NOTE]
> Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire queste procedure.
  
#### <a name="to-allow-dns-dynamic-updates-for-a-zone"></a>Per consentire aggiornamenti dinamici DNS per una zona

1. Aprire Gestore DNS.

2. Nell'albero della console, fare doppio clic sulla zona, quindi fare clic su **proprietà **. La zona **proprietà** apre la finestra di dialogo.

3. Nel **generale** scheda, verificare che il tipo di zona è **primario** o **integrate in Active Directory **.

4. In **gli aggiornamenti dinamici**, verificare che **solo protetti** sia selezionata. Se non è selezionata, modificare il valore di **gli aggiornamenti dinamici** a **solo protetti**, quindi fare clic su **OK **.

#### <a name="to-configure-dns-zone-security-permissions-for-network-controller-nodes"></a>Per configurare le autorizzazioni di protezione di zona DNS per i nodi di Controller di rete

1.  Aprire Gestore DNS.

2.  Nell'albero della console, fare doppio clic sulla zona, quindi fare clic su **proprietà **. La zona **proprietà** apre la finestra di dialogo.

3.  Fare clic su di **sicurezza** scheda e quindi fare clic su **avanzate **. Il **impostazioni di sicurezza avanzate** apre la finestra di dialogo.

4. In **impostazioni di sicurezza avanzate**, fare clic su **Aggiungi **. Il **voce autorizzazione** apre la finestra di dialogo.
  
5. Fare clic su **seleziona un'entità **. Il **Seleziona utente, Computer, Account servizio o gruppo** apre la finestra di dialogo.

6. Nel **Seleziona utente, Computer, Account servizio o gruppo** la finestra di dialogo, fare clic su **tipi di oggetto **. Il **tipi di oggetto** apre la finestra di dialogo. 

7. In **tipi di oggetto**selezionare **computer**, quindi fare clic su **OK **.

8. Nel **Seleziona utente, Computer, Account servizio o gruppo** la finestra di dialogo, digitare il nome NetBIOS di uno dei nodi del Controller di rete nella distribuzione e quindi fare clic su **OK**.

9. In **voce autorizzazione**, assicurarsi che il valore di **tipo** è **Consenti**e il valore di **si applica a** è **questo oggetto e tutti i discendenti**.
  
10. In autorizzazioni selezionare **Scrivi tutte le proprietà** e **eliminare**, quindi fare clic su **OK**.

11. Ripetere i passaggi **5** tramite **10** per tutti i computer e le macchine virtuali del cluster di Controller di rete.

Per ulteriori informazioni, vedere [pianificare un'infrastruttura di rete Software definito](https://technet.microsoft.com/windows-server-docs/networking/sdn/plan/plan-a-software-defined-network-infrastructure).
