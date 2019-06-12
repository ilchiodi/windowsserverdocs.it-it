---
Title: Panoramica del servizio di migrazione archiviazione
description: Il servizio di migrazione di archiviazione rende più semplice la migrazione di server a una versione più recente di Windows Server. Fornisce uno strumento grafico che archivia i dati nei server, quindi trasferisce i dati e configurazione al server più recenti, tutto senza dover apportare alcuna modifica di utenti o App.
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 05/21/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: ce4cbdc291d98a180ee6f5b597d322620fa1b19f
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453175"
---
# <a name="storage-migration-service-overview"></a>Panoramica del servizio di migrazione archiviazione

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server (canale semestrale)

Il servizio di migrazione di archiviazione rende più semplice la migrazione di server a una versione più recente di Windows Server. Fornisce uno strumento grafico che archivia i dati nei server, quindi trasferisce i dati e configurazione al server più recenti, tutto senza dover apportare alcuna modifica di utenti o App.

Questo argomento descrive il motivo per cui si desidera utilizzare il servizio di migrazione di archiviazione, come funziona il processo di migrazione e quali sono i requisiti per i server di origine e destinazione.

## <a name="why-use-storage-migration-service"></a>Perché usare il servizio di migrazione di archiviazione

Usare il servizio di migrazione di archiviazione perché si ha un server (o un numero elevato di server) che si desidera eseguire la migrazione al più recente di hardware o macchine virtuali. Il servizio di migrazione di archiviazione è progettato per consentire le operazioni seguenti:

- Più server e i relativi dati di inventario
- Trasferire rapidamente i file e condivisioni file di configurazione di sicurezza dai server di origine
- Facoltativamente, assumere l'identità del server di origine (anche noto come il cutting failover) in modo che gli utenti e le app non sia necessario modificare alcuna operazione per accedere ai dati esistenti
- Gestire uno o più migrazioni dall'interfaccia utente di Windows Admin Center

![Diagramma che illustra la migrazione dei file del servizio di migrazione di archiviazione e la configurazione dal server di origine per sincronizzazione File di Azure, macchine virtuali di Azure o i server di destinazione.](media/overview/storage-migration-service-diagram.png)

**Figura 1: Archiviazione servizio di migrazione delle origini e destinazioni**

## <a name="how-the-migration-process-works"></a>Come funziona il processo di migrazione

La migrazione è un processo costituito da tre passaggi:

1. **Inventario server** per raccogliere informazioni sui file e configurazione (illustrato nella figura 2).
2. **Il trasferimento dei dati (copia)** dai server di origine per i server di destinazione.
3. **Trasferimento ai nuovi server** (facoltativo).<br>I server di destinazione presuppongono l'origine delle identità precedente dei server in modo che le App e gli utenti non sono necessario apportare alcuna modifica. <br>I server di origine entrano in uno stato di manutenzione in cui vengono ancora contengono gli stessi file hanno sempre (abbiamo mai rimuovere file dai server di origine), ma non sono disponibili per utenti e App. È quindi possibile rimuovere le autorizzazioni dei server quando lo si desidera.

![Screenshot che illustra un server pronto per essere analizzati](media/migrate/inventory.png)
**figura 2: Servizio di migrazione archiviazione inventario dei server**

## <a name="requirements"></a>Requisiti

Per usare il servizio di migrazione di archiviazione, è necessario quanto segue:

- Oggetto **server di origine** per eseguire la migrazione di file e i dati da
- Oggetto **server di destinazione** che esegue Windows Server 2019 per eseguire la migrazione a: Windows Server 2016 e Windows Server 2012 R2 funzionare, ma sono circa il 50% più lento
- Un' **server orchestrator** che esegue Windows Server 2019 per gestire la migrazione  <br>Se si esegue la migrazione solo alcuni server e uno dei server è in esecuzione Windows Server 2019, è possibile utilizzarlo come agente di orchestrazione. Se si esegue la migrazione di più server, è consigliabile usare un server dell'agente di orchestrazione separata.
- Oggetto **PC o server che esegue [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)**  per eseguire l'interfaccia utente del servizio di migrazione di archiviazione, a meno che non si preferisce usare PowerShell per gestire la migrazione. La versione Windows Admin Center e Windows Server 2019 deve essere entrambi almeno versione 1809.

È consigliabile che l'agente di orchestrazione e la destinazione computer dispongono di almeno due core o due Vcpu e almeno 2 GB di memoria. Operazioni di inventario e di trasferimento sono notevolmente più veloci con più processori e memoria.

### <a name="security-requirements"></a>Requisiti di sicurezza

- Un account di migrazione che è un amministratore nel computer di origine e nel computer dell'agente di orchestrazione.
- Un account di migrazione che è un amministratore nel computer di destinazione e nel computer dell'agente di orchestrazione.
- Il computer dell'agente di orchestrazione deve avere la condivisione File e stampanti (SMB-In) regola del firewall abilitata *in ingresso*.
- I computer di origine e destinazione devono avere le seguenti regole del firewall abilitate *in ingresso* (anche se potrebbe essere già li abilitata):
  - Condivisione di file e stampanti (SMB-In)
  - Servizio Accesso rete (NP-In)
  - Strumentazione gestione Windows (DCOM-In)
  - Strumentazione gestione Windows (WMI-In)
  
  > [!TIP]
  > Installa automaticamente il servizio Proxy di servizio di migrazione di archiviazione in un computer Windows Server 2019 apre le porte del firewall necessarie su tale computer.
- Se i computer appartengono a un dominio di Active Directory Domain Services, devono tutte appartengono alla stessa foresta. Il server di destinazione deve essere nello stesso dominio del server di origine anche se si desidera trasferire il nome di dominio dell'origine alla destinazione quando il cutting su. Cutover funzionano tecnicamente tra domini, ma il nome di dominio completo dell'oggetto di destinazione sarà diverso dall'origine...

### <a name="requirements-for-source-servers"></a>Requisiti per i server di origine

Il server di origine deve eseguire uno dei sistemi operativi seguenti:

- Windows Server, come canale semestrale
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2
- Windows Server 2008
- Windows Server 2003 R2
- Windows Server 2003

Se l'agente di orchestrazione è in esecuzione Windows Server, 1903 o versioni successive, è possibile migrare i tipi di origine aggiuntivi seguenti:

- Cluster di failover
- Server Linux che usano Samba. È stato testato il seguente:
    - RedHat Enterprise Linux 7.6, CentOS 7, Debian 8, Ubuntu 16.04 and 12.04.5, SUSE Linux Enterprise Server (SLES) 11 SP4
    - Samba 4.x, 3.6.x

### <a name="requirements-for-destination-servers"></a>Requisiti per i server di destinazione

Il server di destinazione deve eseguire uno dei sistemi operativi seguenti:

- Windows Server, come canale semestrale
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2

> [!TIP]
> Server di destinazione che esegue Windows Server 2019 o Windows Server, come canale semestrale 1809 o versione successiva è necessario raddoppiare le prestazioni di trasferimento delle precedenti versioni di Windows Server. Questo incremento nelle prestazioni è dovuto l'inclusione di un servizio di proxy del servizio di migrazione di archiviazione predefinito, che apre anche il firewall necessarie porte se non sono già aperte.

## <a name="whats-new-in-storage-migration-service"></a>Quali sono le novità nel servizio di migrazione di archiviazione

Windows Server, versione 1903 aggiunge le nuove funzionalità seguenti, quando viene eseguita il server orchestrator:

- Eseguire la migrazione di utenti e gruppi locali nel nuovo server
- La migrazione dell'archiviazione dal cluster di failover
- Eseguire la migrazione di archiviazione da un server Linux che usa Samba
- Migrazione di condivisioni di sincronizzazione più facilmente in Azure con sincronizzazione File di Azure
- Eseguire la migrazione a nuove reti, ad esempio Azure

## <a name="see-also"></a>Vedere anche

- [Eseguire la migrazione di un file server usando il servizio di migrazione di archiviazione](migrate-data.md)
- [Servizi di migrazione archiviazione domande frequenti (FAQ)](faq.md)
- [Servizio di migrazione archiviazione problemi noti](known-issues.md)
