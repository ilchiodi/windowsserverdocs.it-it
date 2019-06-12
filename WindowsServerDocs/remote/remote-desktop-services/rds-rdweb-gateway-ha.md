---
title: Ottenere una disponibilità elevata in primo piano web di Web desktop remoto e Gateway
description: Viene descritta la procedura per installare il server Web desktop remoto e Gateway in una distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: 4e185e51b09d2e2f8ac4527f9de339de27e02f24
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805141"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Ottenere una disponibilità elevata in primo piano web di Web desktop remoto e Gateway

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016


È possibile distribuire un accesso Web Desktop remoto (accesso Web desktop remoto) e farm di Gateway Desktop remoto (Gateway Desktop remoto) per migliorare la disponibilità e scalabilità di una distribuzione di Windows Server Servizi Desktop remoto (RDS) 

Usare la procedura seguente per aggiungere un server Web desktop remoto e Gateway per una distribuzione di base di Servizi Desktop remoto esistente.  

## <a name="pre-requisites"></a>Prerequisiti

Configurare un server di agire come un aggiuntive Web desktop remoto e Gateway Desktop remoto: può trattarsi di un server fisico o macchina virtuale. Sono inclusi aggiungere server al dominio e abilitare la gestione remota.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Passaggio 1: Configurare il nuovo server come parte dell'ambiente di servizi desktop remoto

1. Connettersi al server RDBMS nel portale di Azure, usando il client connessione Desktop remoto.
2. Aggiungere il nuovo server Web desktop remoto e Gateway per Server Manager:
    1. Avviare Server Manager fare clic su **Gestisci > Aggiungi server**.   
    2. Nella finestra di dialogo Aggiungi server, fare clic su **Trova**.   
    3. Selezionare il server appena creato di Web desktop remoto e Gateway (ad esempio, Contoso-WebGw2) e fare clic su **OK**.
3. Aggiungere i server Web desktop remoto e Gateway per la distribuzione  
    1. Avviare Server Manager.  
    2. Fare clic su **Servizi Desktop remoto > Panoramica > server di distribuzione > attività > Aggiungi server di accesso Web desktop remoto**.   
    3. Selezionare il server appena creato (ad esempio, Contoso-WebGw2) e quindi fare clic su **successivo**.  
    4. Nella pagina di conferma, selezionare **riavviare i computer remoti in base alle esigenze**, quindi fare clic su **Aggiungi**.  
    5. Ripetere questi passaggi per aggiungere il server Gateway Desktop remoto, ma scegliere **server Gateway Desktop remoto** nel passaggio b.
4. Installare nuovamente i certificati per i server Gateway Desktop remoto:
   1. In Server Manager sul server RDBMS, scegliere **Servizi Desktop remoto > Panoramica > attività > modificare le proprietà di distribuzione**.  
   2. Espandere **certificati**.  
   3. Scorrere verso il basso nella tabella. Fare clic su desktop remoto **servizio ruolo Gateway > selezionare un certificato esistente.**  
   4. Fare clic su **scegliere un certificato diverso** e quindi selezionare il percorso di certificato. Ad esempio, \Contoso-CB1\Certificates). Selezionare il file del certificato per il server Web desktop remoto e Gateway durante i prerequisiti (ad esempio ContosoRdGwCert) creato e quindi fare clic su **aperto**.  
   5. Immettere la password per il certificato, seleziona **consentire il certificato da aggiungere all'archivio certificati Autorità di certificazione radice attendibili nel computer di destinazione**, quindi fare clic su **OK**.  
   6. Fare clic su **Applica**.
      > [!NOTE] 
      > Si potrebbe essere necessario riavviare manualmente il servizio TSGateway in esecuzione in ogni server Gateway Desktop remoto, tramite Server Manager o Gestione attività.
   7. Ripetere i passaggi da a f per il servizio ruolo Accesso Web desktop remoto.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Passaggio 2: Configurare le proprietà di Web desktop remoto e Gateway Desktop remoto nel nuovo server
1. Configurare il server come parte di una farm di Gateway Desktop remoto:
    1.  In Server Manager sul server RDBMS, scegliere **tutti i server**. Fare doppio clic su uno dei server Gateway Desktop remoto e quindi fare clic su **connessione Desktop remoto**.
    2.  Accedere al server Gateway Desktop remoto utilizzando un account di amministratore di dominio.  
    3.  In Server Manager nel server Gateway Desktop remoto, fare clic su **strumenti > Servizi Desktop remoto > Gestione Gateway Desktop remoto**.  
    4.  Nel riquadro di spostamento, fare clic sul computer locale (ad esempio, Contoso-WebGw1).  
    5.  Fare clic su **i membri di Farm di Server Gateway Desktop remoto aggiungere**.  
    6.  Nel **Server Farm** scheda, immettere il nome di ogni server Gateway Desktop remoto, quindi fare clic su **Add** e **Apply**.  
    7.  Ripetere i passaggi alla f in ogni server Gateway Desktop remoto in modo che si riconoscano a vicenda come server di Gateway Desktop remoto in una farm. Non bisogna preoccuparsi se sono presenti avvisi, poiché potrebbe richiedere tempo per la propagazione delle impostazioni DNS.
2. Configurare il server come parte di una farm di accesso Web desktop remoto. I passaggi seguenti configurano la convalida e decrittografia le chiavi del computer per essere lo stesso per entrambi i siti RDWeb.
    1.  In Server Manager sul server RDBMS, scegliere **tutti i server**. Fare doppio clic il primo server di accesso Web desktop remoto (ad esempio, Contoso-WebGw1) e quindi fare clic su **connessione Desktop remoto**.  
    2.  Accedere al server Accesso Web desktop remoto usando un account di amministratore di dominio.  
    3.  In Server Manager nel server di accesso Web desktop remoto, fare clic su **strumenti > Gestione Internet Information Services (IIS)** .  
    4.  Nel riquadro sinistro di gestione IIS, espandere la **Server (ad esempio, Contoso-WebGw1) > siti > sito Web predefinito**, quindi fare clic su **RDWeb**.  
    5.  Fare doppio clic su **chiave del computer**, quindi fare clic su **Apri funzionalità**.
    6.  Nella pagina chiave del computer nel **azioni** riquadro, selezionare **genera chiavi**, quindi fare clic su **applica**.
    7.  Copiare la chiave di convalida (è possibile fare doppio clic la chiave e quindi fare clic su **copia**.)
    8.  In Gestione IIS sotto **sito Web predefinito**, selezionare **Feed**, **FeedLogon** e **pagine** a sua volta.
    9. Per ognuno:
        1.  Fare doppio clic su **chiave del computer**, quindi fare clic su **Apri funzionalità**.
        2.  Per la chiave di convalida, cancellare **genera automaticamente in fase di esecuzione**e quindi incollare la chiave copiata nel passaggio c.
    10.  Ridurre a icona la finestra di connessione desktop remoto al server Web desktop remoto.  
    11.  Ripetere i passaggi b-e per il secondo server Accesso Web desktop remoto, che termina con la visualizzazione di funzionalità del **chiave del computer**.
    12. Per la chiave di convalida, cancellare **genera automaticamente in fase di esecuzione**e quindi incollare la chiave copiata nel passaggio c.
    13. Fare clic su **Applica**.
    14. Completare il processo per il **RDWeb**, **Feed**, **FeedLogon** e **pagine** pagine.
    15. Ridurre a icona la finestra di connessione desktop remoto per il secondo server Accesso Web desktop remoto e quindi ingrandire la finestra di connessione desktop remoto per il primo server di accesso Web desktop remoto.  
    16. Ripetere i passaggi g-n per copiare la chiave di decrittografia.
    17. Quando le chiavi di convalida e le chiavi di decrittografia sono identiche in entrambi i server Accesso Web desktop remoto per il **RDWeb**, **Feed**, **FeedLogon** e **pagine**le pagine, disconnettersi da tutte le finestre di connessione desktop remoto.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Passaggio 3: Configurare il bilanciamento del carico per i server Web desktop remoto e Gateway Desktop remoto

Se si utilizza dell'infrastruttura di Azure, è possibile creare un servizio di bilanciamento del carico esterno di Azure; in caso contrario, è possibile configurare un separato bilanciamento del carico hardware o software. Il bilanciamento del carico è chiave in modo che il traffico sarà uniformemente distribuite le connessioni dai client di Desktop remoto, attraverso il Gateway Desktop remoto, ai server che gli utenti verranno eseguiti i carichi di lavoro di lunga durate.

> [!NOTE] 
> Se il server precedente che esegue Web desktop remoto e Gateway Desktop remoto è stato già configurato dietro un servizio di bilanciamento del carico esterno, proseguire con il passaggio per passaggio 4, selezionare il pool back-end esistente e aggiungere il nuovo server al pool.

1.  Creare un servizio di bilanciamento del carico di Azure:  
    1.  Nel portale Azure fare clic **Sfoglia > bilanciamenti del carico > Aggiungi**.  
    2.  Immettere un nome, ad esempio **WebGwLB**.  
    3.  Selezionare **pubbliche** per il **schema**, **indirizzo IP pubblico**e un **indirizzo IP pubblico**. È possibile selezionare un indirizzo IP pubblico esistente o crearne uno nuovo. 
    4.  Selezionare un valore appropriato **abbonamento**, **gruppo di risorse**, e **percorso**.
    5.  Fare clic su **Crea**.  
2. Creare un [probe](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) per monitorare quali server sono attivi:  
    1.  Nel portale Azure fare clic **Sfoglia > Load Balancers**., il servizio di bilanciamento del carico appena creato, ad esempio WebGwLB e impostazioni  
    2.  Fare clic su **probe > aggiungere**.  
    3.  Immettere un nome, ad esempio, **HTTPS**, per il probe. Selezionare **TCP** come la **protocollo**, quindi immettere **443** per il **porta**, quindi fare clic su **OK**.   
3.  Creare regole di bilanciamento del carico UDP e HTTPS:  
    1.  Nelle **le impostazioni**, fare clic su **regole di bilanciamento del carico**.  
    2.  Selezionare **Add** per il **regola HTTPS**.  
    3.  Immettere un nome per la regola, ad esempio, HTTPS e selezionare **TCP** per il **protocollo**. Immettere **443** per entrambe **porta** e **porta back-end**, fare clic su **OK**.  
    4.  Nelle **regole di bilanciamento del carico**, fare clic su **Add** per il **regola UDP**.  
    5.  Immettere un nome per la regola, ad esempio, **UDP**e selezionare **UDP** per il **protocollo**. Immettere **3391** per entrambe **porta** e **porta back-end**, fare clic su **OK**.  
4. Creare il pool back-end per i server Web desktop remoto e Gateway Desktop remoto:
      1. Nelle **le impostazioni**, fare clic su **pool di indirizzi back-end > Aggiungi**.   
      2. Immettere un nome (ad esempio, **WebGwBackendPool**), quindi fare clic su **aggiungere una macchina virtuale**.  
      3. Scegliere un set di disponibilità (ad esempio, WebGwAvSet) e quindi fare clic su **OK**.   
      4. Fare clic su **scegliere le macchine virtuali**, selezionare ogni macchina virtuale e quindi fare clic su **Seleziona > OK > OK**.
