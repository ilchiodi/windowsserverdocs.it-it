---
title: Aggiungere un server Gestore connessione desktop remoto per configurare la disponibilità elevata in Servizi Desktop remoto
description: Informazioni su come aggiungere un gestore connessione desktop remoto a una distribuzione di servizi desktop remoto per la disponibilità elevata.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 2f4fc63c6ff7c1254fda630a8f34188d8fedc8e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825042"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Aggiungere il server Gestore connessione Desktop remoto alla distribuzione e configurare la disponibilità elevata

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile distribuire un cluster di Gestore connessione Desktop remoto (Gestore connessione desktop remoto) per migliorare la disponibilità e scalabilità dell'infrastruttura di Servizi Desktop remoto. 

## <a name="pre-requisites"></a>Prerequisiti

Configurare un server di agire come una seconda connessione desktop remoto: può trattarsi di un server fisico o una macchina virtuale.

Impostare un database per il gestore di connessione. È possibile usare [Database SQL di Azure](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) istanza o SQL Server nell'ambiente locale. Si parla con Azure SQL riportato di seguito, ma i passaggi sono comunque applicabili a SQL Server. È necessario trovare la stringa di connessione per il database e assicurarsi di avere il driver ODBC corretto.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Passaggio 1: Configurare il database per il gestore di connessione

1. Trovare la stringa di connessione per il database che è stato creato, è necessario sia per identificare la versione del driver ODBC è necessario e in un secondo momento, quando si configura il gestore di connessione se stesso (passaggio 3), quindi è necessario salvare la stringa di una posizione in cui è possibile utilizzarlo come riferimento facilmente. Ecco come trovare la stringa di connessione per SQL di Azure:  
    1. Nel portale di Azure, fare clic su **Sfoglia > gruppi di risorse** e fare clic sul gruppo di risorse per la distribuzione.   
    2. Selezionare il database SQL che appena creato (ad esempio, DB1-CB).   
    3. Fare clic su **Impostazioni > Proprietà > Mostra stringhe di connessione database**.   
    4. Copiare la stringa di connessione **ODBC (include Node. js)**, quali dovrebbe essere simile al seguente:   
      
        Driver = {SQL Server Native Client 13.0}; Server = tcp:cb-sqls1.database.windows.net,1433; Database = CB-DB1; UID =sqladmin@contoso; Pwd = {your_password_here}; Crittografare = yes; TrustServerCertificate = no. Timeout di connessione = 30.   
  
    5. Sostituire "your_password_here" con la password effettiva. Si userà questa stringa intera, con la password, inclusa quando ci si connette al database. 
2. Installare il driver ODBC nel nuovo gestore di connessione: 
   1. Se si usa una macchina virtuale per il gestore di connessione, creare un indirizzo IP pubblico per il primo gestore connessione desktop remoto. (È sufficiente eseguire questa operazione se la macchina virtuale RDBMS non possieda già un indirizzo IP pubblico per consentire le connessioni RDP.)
       1. Nel portale di Azure, fare clic su **Sfoglia > gruppi di risorse**, fare clic sul gruppo di risorse per la distribuzione e quindi fare clic sulla prima macchina virtuale Gestore connessione desktop remoto (ad esempio, Contoso-Cb1).
       2. Fare clic su **Impostazioni > interfacce di rete**, quindi scegliere l'interfaccia di rete corrispondente.
       3. Fare clic su **Impostazioni > indirizzo IP**.
       4. Per **indirizzo IP pubblico**, selezionare **Enabled**, quindi fare clic su **indirizzo IP**.
       5. Se si dispone di un indirizzo IP pubblico esistente da utilizzare, selezionarlo dall'elenco. In caso contrario, fare clic su **Create new**, immettere un nome e quindi fare clic su **OK** e quindi **salvare**.
   2. Connettersi al primo gestore connessione desktop remoto:
       1. Nel portale di Azure, fare clic su **Sfoglia > gruppi di risorse**, fare clic sul gruppo di risorse per la distribuzione e quindi fare clic sulla prima macchina virtuale Gestore connessione desktop remoto (ad esempio, Contoso-Cb1).
       2. Fare clic su **Connetti > aprire** per aprire il client Desktop remoto.
       3. Nel client, fare clic su **Connect**, quindi fare clic su **utilizza un altro account utente**. Immettere il nome utente e la password per un account di amministratore di dominio.
       4. Fare clic su **Sì** quando l'avviso sul certificato.
   3. Scaricare il [ODBC driver for SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) che corrisponde alla versione nella stringa di connessione ODBC. Per la stringa di esempio precedente, è necessario installare il driver ODBC versione 13.
   4. Copiare il file sqlincli.msi al primo server Gestore connessione desktop remoto.   
   5. Aprire il file sqlincli.msi e installare il client nativo.  
   6. Ripetere i passaggi 1-5 per ciascun altri gestori di connessione desktop remoto (ad esempio, Contoso-Cb2).
   7. Installare il driver ODBC in ogni server che eseguirà il gestore di connessione.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Passaggio 2: Configurare il bilanciamento del carico sui gestori connessione desktop remoto 

Se si utilizza dell'infrastruttura di Azure, è possibile creare un [servizio Azure load balancer](#create-a-load-balancer); se non è possibile impostare fino [DNS round robin](#configure-dns-round--robin). 

### <a name="create-a-load-balancer"></a>Creare un servizio di bilanciamento del carico  
1. Creare un servizio di bilanciamento del carico di Azure   
      1. Nel portale Azure fare clic **Sfoglia > bilanciamenti del carico > Aggiungi**.   
      2. Immettere un nome per il nuovo bilanciamento del carico (ad esempio, hacb).   
      3. Selezionare **Internal** per il **schema**, **rete virtuale** per la distribuzione (ad esempio, Contoso-VNet) e il **Subnet** con tutti le risorse (ad esempio, per impostazione predefinita).   
      4. Selezionare **statici** per il **assegnazione di indirizzi IP** e immettere un **indirizzo IP privato** vale a dire non attualmente in uso (ad esempio, 10.0.0.32).   
      5. Selezionare un valore appropriato **abbonamento**, il **gruppo di risorse** con tutte le risorse e appropriato **percorso**.   
      6. Selezionare **Create**.   
2. Creare un [probe](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) per monitorare i server che sono attivi:   
      1. Nel portale di Azure, fare clic su **Sfoglia > Load Balancers**e quindi fare clic sul bilanciamento del carico appena creato, (ad esempio, CBLB). Fare clic su **Impostazioni**.   
      2. Fare clic su **probe > aggiungere**.   
      3. Immettere un nome per il probe (ad esempio, **RDP**), selezionare **TCP** come il **protocollo**, immettere **3389** per il **porta**, quindi fare clic su **OK**.   
3. Creare il pool back-end dei broker di connessione:   
      1. Nelle **le impostazioni**, fare clic su **pool di indirizzi back-end > Aggiungi**.   
      2. Immettere un nome (ad esempio, CBBackendPool) e quindi fare clic su **aggiungere una macchina virtuale**.  
      3. Scegliere un set di disponibilità (ad esempio, CbAvSet) e quindi fare clic su **OK**.   
      3. Fare clic su **scegliere le macchine virtuali**, selezionare ogni macchina virtuale e quindi fare clic su **Seleziona > OK > OK**.   
4. Creare la regola di bilanciamento del carico RDP:   
      1. Nella **le impostazioni**, fare clic su **regole di bilanciamento del carico**, quindi fare clic su **Aggiungi**.   
      2. Immettere un nome (ad esempio RDP), selezionare **TCP** per il **Protocol**, immettere **3389** per entrambi **porta** e **porta back-end** , fare clic su **OK**.   
5. Aggiungere un record DNS per il bilanciamento del carico:   
      1. Connettersi alla macchina virtuale server RDBMS (ad esempio, Contoso-CB1). Consultare il [preparare la VM di Gestore connessione desktop remoto](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) articolo per i passaggi per la modalità di connessione alla macchina virtuale.   
      2. In Server Manager fare clic su **strumenti > DNS**.   
      3. Nel riquadro sinistro, espandere **DNS**, fare clic sulla macchina DNS, fare clic su **zone di ricerca diretta**, quindi scegliere il nome di dominio (ad esempio, Contoso.com). (Potrebbe richiedere alcuni secondi per elaborare la query al server DNS per le informazioni).  
      4. Fare clic su **azione > Nuovo Host (A o AAAA)**.   
      9. Immettere il nome (ad esempio, hacb) e l'indirizzo IP specificato in precedenza (ad esempio, 10.0.0.32).   
  
### <a name="configure-dns-round-robin"></a>Configurare DNS round robin  
  
I passaggi seguenti sono un'alternativa alla creazione di un bilanciamento del carico interno di Azure.   
  
1. Connettersi al server RDBMS nel portale di Azure. tramite il client connessione Desktop remoto   
2. Creare record DNS:   
      1. In Server Manager fare clic su **strumenti > DNS**.   
      2. Nel riquadro sinistro, espandere **DNS**, fare clic sulla macchina DNS, fare clic su **zone di ricerca diretta**, quindi scegliere il nome di dominio (ad esempio, Contoso.com). (Potrebbe richiedere alcuni secondi per elaborare la query al server DNS per le informazioni).  
      3. Fare clic su **azione** e **nuovo Host (A o AAAA)**.   
      4. Immettere il **nome DNS** per il Gestore connessione desktop remoto del cluster (ad esempio, hacb) e quindi immettere il **indirizzo IP** del primo gestore connessione desktop remoto.   
      5. Ripetere i passaggi 3-4 per ogni gestore di connessione di desktop remoto aggiuntive, fornendo ogni indirizzo IP univoco per ogni record aggiuntivi.


Ad esempio, se gli indirizzi IP per le due macchine virtuali di Gestore connessione desktop remoto sono 10.0.0.8 e 10.0.0.9, si creerà due record host DNS:
 - Nome host: hacb.contoso.com, l'indirizzo IP: 10.0.0.8
 - Nome host: hacb.contoso.com, l'indirizzo IP: 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Passaggio 3: Configurare i gestori di connessione per la disponibilità elevata

1. Aggiungere il nuovo server Gestore connessione desktop remoto a Server Manager:
   1. In Server Manager fare clic su **Gestisci > Aggiungi server**.
   2. Fare clic su **trova**.
   3. Selezionare il server Gestore connessione desktop remoto appena creato (ad esempio, Contoso-Cb2) e fare clic su **OK**.
2. Configurare la disponibilità elevata di Gestore connessione desktop remoto:
   1. In Server Manager, fare clic su **Servizi Desktop remoto > Panoramica**.
   2. Fare doppio clic su **Gestore connessione desktop remoto**, quindi fare clic su **configurare la disponibilità elevata**.
   3. Pagina della procedura guidata finché non giunge alla sezione del tipo di configurazione. Selezionare **server di database condiviso**, quindi fare clic su **successivo**.
   4. Immettere il nome DNS per il cluster di Gestore connessione desktop remoto.
   5. Immettere la stringa di connessione per il database SQL e quindi spostarsi tra la procedura guidata per stabilire la disponibilità elevata.
3. Aggiungere il nuovo gestore connessione desktop remoto alla distribuzione
   1. In Server Manager, fare clic su **Servizi Desktop remoto > Panoramica**.
   2. Fare doppio clic su Gestore connessione desktop remoto e quindi fare clic su **aggiunta di Server Gestore connessione desktop remoto**.
   3. Pagina procedura guidata finché non giunge alla selezione dei Server, quindi selezionare il server Gestore connessione desktop remoto appena creato (ad esempio, Contoso-CB2).
   4. Completare la procedura guidata, accettando i valori predefiniti.
4. Configurare i certificati attendibili sui client e server Gestore connessione desktop remoto.

