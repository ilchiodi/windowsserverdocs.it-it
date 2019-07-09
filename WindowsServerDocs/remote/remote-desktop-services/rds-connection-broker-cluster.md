---
title: Aggiungere un server Gestore connessione Desktop remoto per configurare la disponibilità elevata in Servizi Desktop remoto
description: Informazioni su come aggiungere un Gestore connessione Desktop remoto a una distribuzione Servizi Desktop remoto per la disponibilità elevata.
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
ms.openlocfilehash: b1e5726e3976527278b11f105007a32548da0bc4
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805148"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Aggiungere il server Gestore connessione Desktop remoto alla distribuzione e configurare la disponibilità elevata

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

Puoi distribuire un cluster di server Gestore connessione Desktop remoto per aumentare la disponibilità e migliorare la scalabilità dell'infrastruttura di Servizi Desktop remoto. 

## <a name="pre-requisites"></a>Prerequisiti

Imposta un server in modo che funga da secondo Gestore connessione Desktop remoto. Può trattarsi di un server fisico o di una macchina virtuale.

Imposta un database per il Gestore connessione. Puoi usare un'istanza del [database SQL di Azure](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) oppure SQL Server nell'ambiente locale. Di seguito viene illustrato come usare SQL di Azure, ma tali procedure sono valide anche per SQL Server. Dovrai trovare la stringa di connessione per il database e assicurarti di disporre del driver ODBC corretto.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Passaggio 1: Configurare il database per il Gestore connessione

1. Trova la stringa di connessione per il database creato. Tale stringa serve per identificare la versione del driver ODBC necessaria e successivamente durante la configurazione del Gestore connessione vero e proprio (passaggio 3), pertanto conservala dove puoi farvi riferimento facilmente. Di seguito viene spiegato come trovare la stringa di connessione per SQL di Azure:  
    1. Nel portale di Azure fai clic su **Sfoglia > Gruppi di risorse** e quindi sul gruppo di risorse per la distribuzione.   
    2. Seleziona il database SQL appena creato (ad esempio, CB-DB1).   
    3. Fai clic su **Impostazioni** > **Proprietà** > **Mostra stringhe di connessione del database**.   
    4. Copia la stringa di connessione per **ODBC (include Node.js)** , che dovrebbe essere simile alla seguente:   
      
        ```
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
        ```
  
    5. Sostituisci "your_password_here" con la password effettiva. Userai l'intera stringa, con la password inclusa, durante la connessione al database. 
2. Installa il driver ODBC nel nuovo Gestore connessione: 
   1. Se usi una macchina virtuale per il Gestore connessione, crea un indirizzo IP pubblico per il primo Gestore connessione Desktop remoto. Devi eseguire questa operazione solo se la macchina virtuale Servizio di gestione Desktop remoto non ha ancora un indirizzo IP pubblico per consentire le connessioni RDP.
       1. Nel portale di Azure fai clic su **Sfoglia** > **Gruppi di risorse**, fai clic sul gruppo di risorse per la distribuzione e quindi fai clic sulla macchina virtuale corrispondente al primo Gestore connessione Desktop remoto (ad esempio, Contoso-Cb1).
       2. Fare clic su **Impostazioni > interfacce di rete**, quindi scegliere l'interfaccia di rete corrispondente.
       3. Fare clic su **Impostazioni > indirizzo IP**.
       4. Per **indirizzo IP pubblico**, selezionare **Enabled**, quindi fare clic su **indirizzo IP**.
       5. Se si dispone di un indirizzo IP pubblico esistente da utilizzare, selezionarlo dall'elenco. In caso contrario, fare clic su **Create new**, immettere un nome e quindi fare clic su **OK** e quindi **salvare**.
   2. Connettiti al primo Gestore connessione Desktop remoto:
       1. Nel portale di Azure fai clic su **Sfoglia** > **Gruppi di risorse**, fai clic sul gruppo di risorse per la distribuzione e quindi fai clic sulla macchina virtuale corrispondente al primo Gestore connessione Desktop remoto (ad esempio, Contoso-Cb1).
       2. Fare clic su **Connetti > aprire** per aprire il client Desktop remoto.
       3. Nel client, fare clic su **Connect**, quindi fare clic su **utilizza un altro account utente**. Immettere il nome utente e la password per un account di amministratore di dominio.
       4. Fare clic su **Sì** quando l'avviso sul certificato.
   3. Scarica il [driver ODBC per SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) corrispondente alla versione nella stringa di connessione ODBC. Per la stringa di esempio sopra riportata, è necessario installare il driver ODBC versione 13.
   4. Copia il file sqlincli.msi nel primo server Gestore connessione Desktop remoto.   
   5. Apri il file sqlincli.msi e installa il client nativo.  
   6. Ripeti i passaggi da 1 a 5 per ogni Gestore connessione Desktop remoto aggiuntivo (ad esempio, Contoso-Cb2).
   7. Installa il driver ODBC in ogni server che eseguirà il gestore connessione.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Passaggio 2: Configurare il bilanciamento del carico nei Gestori connessione Desktop remoto 

Se usi l'infrastruttura di Azure, puoi creare un [servizio di bilanciamento del carico di Azure](#create-a-load-balancer). In caso contrario, puoi impostare un [round robin DNS](#configure-dns-round-robin).

### <a name="create-a-load-balancer"></a>Creare un servizio di bilanciamento del carico  
1. Crea un servizio di bilanciamento del carico di Azure:   
      1. Nel portale di Azure fai clic su **Sfoglia > Servizi di bilanciamento del carico > Aggiungi**.   
      2. Immetti un nome per il nuovo servizio di bilanciamento del carico (ad esempio, hacb).   
      3. Seleziona **Interno** per **Schema**, la **rete virtuale** per la distribuzione (ad esempio, Contoso-VNet) e la **subnet** con tutte le risorse (ad esempio, quella predefinita).   
      4. Seleziona **Statico** per **Assegnazione indirizzo IP** e nel campo **Indirizzo IP privato** immetti un valore attualmente non in uso (ad esempio, 10.0.0.32).   
      5. Seleziona la **sottoscrizione** appropriata, il **gruppo di risorse** con tutte le risorse e il **percorso** appropriato.   
      6. Seleziona **Crea**.   
2. Crea un [probe](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) per monitorare quali server sono attivi:   
      1. Nel portale di Azure fai clic su **Sfoglia > Servizi di bilanciamento del carico** e quindi sul servizio di bilanciamento del carico appena creato (ad esempio, CBLB). Fare clic su **Impostazioni**.   
      2. Fai clic su **Probe > Aggiungi**.   
      3. Immetti un nome per il probe (ad esempio, **RDP**), seleziona **TCP** come **Protocollo**, immetti **3389** per il campo **Porta** e infine fai clic su **OK**.   
3. Crea il pool back-end dei Gestori connessione:   
      1. In **Impostazioni** fai clic su **Pool di indirizzi back-end > Aggiungi**.   
      2. Immetti un nome (ad esempio, CBBackendPool) e quindi fai clic su **Aggiungi una macchina virtuale**.  
      3. Scegli un set di disponibilità (ad esempio, CbAvSet) e quindi fai clic su **OK**.   
      3. Fai clic su **Scegli le macchine virtuali**, seleziona ogni macchina virtuale e quindi fai clic su **Seleziona > OK > OK**.   
4. Crea la regola di bilanciamento del carico RDP:   
      1. In **Impostazioni** fai clic su **Regole di bilanciamento del carico** e quindi su **Aggiungi**.   
      2. Immetti un nome (ad esempio, RDP), seleziona **TCP** come **Protocollo**, immetti **3389** per i campi **Porta** e **Porta back-end** e infine fai clic su **OK**.   
5. Aggiungi un record DNS per il bilanciamento del carico:   
      1. Connettiti alla macchina virtuale corrispondente al server Servizio di gestione Desktop remoto (ad esempio, Contoso-CB1). Leggi l'articolo [Preparare la macchina virtuale del Gestore connessione Desktop remoto](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) per avere informazioni sulla procedura per connetterti alla macchina virtuale.   
      2. In Server Manager fai clic su **Strumenti > DNS**.   
      3. Nel riquadro sinistro espandi **DNS**, fai clic sulla macchina DNS, su **Zone di ricerca diretta** e quindi sul nome del dominio (ad esempio, Contoso.com). Potrebbe essere necessario qualche secondo per elaborare la query inviata al server DNS per le informazioni.  
      4. Fai clic su **Azione > Nuovo host (A o AAAA)** .   
      9. Immetti il nome (ad esempio, hacb) e l'indirizzo IP specificato in precedenza (ad esempio, 10.0.0.32).   

### <a name="configure-dns-round-robin"></a>Configurare un round robin DNS  
  
La procedura descritta di seguito rappresenta un'alternativa alla creazione di un servizio di bilanciamento del carico interno di Azure.   
  
1. Connettiti al server Servizio di gestione Desktop remoto nel portale di Azure usando il client Connessione Desktop remoto.   
2. Crea record DNS:   
      1. In Server Manager fai clic su **Strumenti > DNS**.   
      2. Nel riquadro sinistro espandi **DNS**, fai clic sulla macchina DNS, su **Zone di ricerca diretta** e quindi sul nome del dominio (ad esempio, Contoso.com). Potrebbe essere necessario qualche secondo per elaborare la query inviata al server DNS per le informazioni.  
      3. Fai clic su **Azione** e **Nuovo host (A o AAAA)** .   
      4. Immetti il **nome DNS** per il cluster di server Gestore connessione Desktop remoto (ad esempio, hacb) e quindi immetti l'**indirizzo IP** del primo Gestore connessione Desktop remoto.   
      5. Ripeti i passaggi 3 e 4 per ogni Gestore connessione Desktop remoto aggiuntivo, fornendo un indirizzo IP univoco per ogni record ulteriore.


Ad esempio, se gli indirizzi IP per le due macchine virtuali Gestore connessione Desktop remoto sono 10.0.0.8 e 10.0.0.9, dovrai creare due record host DNS:
 - Nome host: hacb.contoso.com, Indirizzo IP: 10.0.0.8
 - Nome host: hacb.contoso.com, Indirizzo IP: 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Passaggio 3: Configurare i Gestori connessione per la disponibilità elevata

1. Aggiungi il nuovo server Gestore connessione Desktop remoto a Server Manager:
   1. In Server Manager fai clic su **Gestisci > Aggiungi server**.
   2. Fai clic su **Trova**.
   3. Fai clic sul server Gestore connessione Desktop remoto appena creato (ad esempio, Contoso-Cb2) e quindi su **OK**.
2. Configura la disponibilità elevata per il Gestore connessione Desktop remoto:
   1. In Server Manager, fare clic su **Servizi Desktop remoto > Panoramica**.
   2. Fai clic con il pulsante destro del mouse su **Gestore connessione Desktop remoto** e quindi scegli **Configura disponibilità elevata**.
   3. Scorri in avanti le pagine della procedura guidata fino ad arrivare alla sezione Tipo di configurazione. Seleziona **Server di database condiviso** e quindi fai clic su **Avanti**.
   4. Immetti il nome DNS per il cluster di server Gestore connessione Desktop remoto.
   5. Immetti la stringa di connessione per il database SQL e quindi scorri in avanti le pagine della procedura guidata per stabilire la disponibilità elevata.
3. Aggiungi il nuovo Gestore connessione Desktop remoto alla distribuzione:
   1. In Server Manager, fare clic su **Servizi Desktop remoto > Panoramica**.
   2. Fai clic con il pulsante destro del mouse sul Gestore connessione Desktop remoto e quindi scegli **Add RD Connection Broker Server** (Aggiungi server Gestore connessione Desktop remoto).
   3. Scorri in avanti le pagine della procedura guidata fino ad arrivare a Selezione dei server e quindi seleziona il server Gestore connessione Desktop remoto appena creato (ad esempio, Contoso-CB2).
   4. Completa la procedura guidata accettando i valori predefiniti.
4. Configura i certificati attendibili nei client e server Gestore connessione Desktop remoto.

