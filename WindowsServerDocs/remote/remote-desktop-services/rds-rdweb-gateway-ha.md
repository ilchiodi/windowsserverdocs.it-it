---
title: Aggiungere disponibilità elevata al front-end Web e Gateway Desktop remoto
description: Descrive i passaggi da eseguire per installare i server Web e Gateway Desktop remoto in una distribuzione Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: e98bbda5460311dd379eab6f5a5bde0ec3845d5c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860284"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Aggiungere disponibilità elevata al front-end Web e Gateway Desktop remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016


Puoi distribuire una farm di Accesso Web Desktop remoto e Gateway Desktop remoto per migliorare la disponibilità e la scalabilità di una distribuzione Servizi Desktop remoto Windows. 

Usa la procedura descritta di seguito per aggiungere un server Web e Gateway Desktop remoto a una distribuzione di base esistente di Servizi Desktop remoto.  

## <a name="pre-requisites"></a>Prerequisiti

Imposta un server in modo che funga da server Web e Gateway Desktop remoto aggiuntivo. Può trattarsi di un server fisico o di una macchina virtuale. Tra le operazioni da eseguire sono incluse l'aggiunta del server al dominio e l'abilitazione della gestione remota.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Passaggio 1: Configurare il nuovo server come parte dell'ambiente Servizi desktop remoto

1. Connettiti al server Servizio di gestione Desktop remoto nel portale di Azure usando il client Connessione Desktop remoto.
2. Aggiungere il nuovo server Web desktop remoto e Gateway per Server Manager:
    1. Avviare Server Manager fare clic su **Gestisci > Aggiungi server**.   
    2. Nella finestra di dialogo Aggiungi server, fare clic su **Trova**.   
    3. Seleziona il server Web e Gateway Desktop remoto appena creato (ad esempio, Contoso-WebGw2) e fai su **OK**.
3. Aggiungi i server Web e Gateway Desktop remoto alla distribuzione:  
    1. Avviare Server Manager.  
    2. Fai clic su **Servizi Desktop remoto > Panoramica > Server distribuzione > Attività > Add RD Web Access Servers** (Aggiungi server Accesso Web Desktop remoto).   
    3. Seleziona il server appena creato (ad esempio, Contoso-WebGw2) e quindi fai clic su **Avanti**.  
    4. Nella pagina di conferma, selezionare **riavviare i computer remoti in base alle esigenze**, quindi fare clic su **Aggiungi**.  
    5. Ripeti questi passaggi per aggiungere il server Gateway Desktop remoto, ma scegli **Server gateway di Desktop remoto** nel passaggio b.
4. Reinstalla i certificati per i server Gateway Desktop remoto:
   1. In Server Manager nel server Servizio di gestione Desktop remoto fai clic su **Servizi Desktop remoto > Panoramica > Attività > Edit Deployment Properties** (Modifica proprietà distribuzione).  
   2. Espandi **Certificati**.  
   3. Scorri verso il basso fino alla tabella. Fai clic su **Servizio ruolo Gateway Desktop remoto > Seleziona certificato esistente.**  
   4. Fai clic su **Scegliere un certificato diverso** e quindi passa al percorso del certificato. Ad esempio, \Contoso-CB1\Certificates. Seleziona il file del certificato per il server Web e Gateway Desktop remoto creato durante le operazioni da eseguire come prerequisiti, ad esempio ContosoRdGwCert, e quindi fai clic su **Apri**.  
   5. Immetti la password per il certificato, seleziona **Consenti aggiunta del certificato all'archivio certificati delle Autorità di certificazione radice attendibili nei computer di destinazione** e quindi fai clic su **OK**.  
   6. Fare clic su **Applica**.
      > [!NOTE] 
      > Potresti dover riavviare manualmente il servizio TSGateway in esecuzione in ogni server Gateway Desktop remoto, tramite Server Manager o Gestione attività.
   7. Ripeti i passaggi a-f per Servizio ruolo Accesso Web Desktop remoto.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Passaggio 2: Configurare le proprietà del server Web e Gateway Desktop remoto nel nuovo server
1. Configura il server come parte di una farm di Gateway Desktop remoto:
    1.  In Server Manager nel server Servizio di gestione Desktop remoto fai clic su **Tutti i server**. Fai clic con il pulsante destro del mouse su uno dei server Gateway Desktop remoto e quindi scegli **Connessione Desktop remoto**.
    2.  Accedi al server Gateway Desktop remoto usando un account di amministratore di dominio.  
    3.  In Server Manager nel server Gateway Desktop remoto fai clic su **Strumenti > Servizi Desktop remoto > Gestione Gateway Desktop remoto**.  
    4.  Nel riquadro di spostamento fai clic sul computer locale, ad esempio Contoso-WebGw1.  
    5.  Fai clic su **Aggiungi membri server farm del Gateway Desktop remoto**.  
    6.  Nella scheda **Server farm** immetti il nome di ogni server Gateway Desktop remoto, quindi fai clic su **Aggiungi** e infine su **Applica**.  
    7.  Ripeti i passaggi a-f in ogni server Gateway Desktop remoto in modo che tali server si riconoscano reciprocamente come server Gateway Desktop remoto in una farm. Non ti preoccupare se vengono visualizzati avvisi perché può essere necessario del tempo per propagare le impostazioni DNS.
2. Configura il server come parte di una farm di Accesso Web Desktop remoto. I passaggi seguenti consentono di configurare le chiavi computer di convalida e decrittografia in modo che siano uguali in entrambi i siti RDWeb.
    1.  In Server Manager nel server Servizio di gestione Desktop remoto fai clic su **Tutti i server**. Fai clic con il pulsante destro del mouse sul primo server Accesso Web Desktop remoto (ad esempio, Contoso-WebGw1) e quindi fai clic su **Connessione Desktop remoto**.  
    2.  Accedi al server Accesso Web Desktop remoto usando un account di amministratore di dominio.  
    3.  In Server Manager nel server Accesso Web Desktop remoto fai clic su **Strumenti > Gestione Internet Information Services (IIS)** .  
    4.  Nel riquadro sinistro di Gestione IIS espandi il **server (ad esempio, Contoso-WebGw1) > Siti > Sito Web predefinito** e quindi fai clic su **RDWeb**.  
    5.  Fai clic con il pulsante destro del mouse su **Chiave computer** e quindi scegli **Apri funzionalità**.
    6.  Nella pagina Chiave computer, nel riquadro **Azioni**, seleziona **Genera chiavi** e quindi fai clic su **Applica**.
    7.  Copia la chiave di convalida (puoi fare clic con il pulsante destro del mouse sulla chiave e quindi scegliere **Copia**.)
    8.  In Gestione IIS, in **Sito Web predefinito**, seleziona di volta in volta **Feed**, **FeedLogon** e **Pages**.
    9. Per ognuno:
        1.  Fai clic con il pulsante destro del mouse su **Chiave computer** e quindi scegli **Apri funzionalità**.
        2.  Per la chiave di convalida, deseleziona **Genera automaticamente in fase di esecuzione** e quindi incolla la chiave copiata nel passaggio g.
    10.  Riduci a icona la finestra Connessione Desktop remoto per questo server Web Desktop remoto.  
    11.  Ripeti i passaggi b-e per il secondo server Accesso Web Desktop remoto, terminando con la visualizzazione delle funzionalità di **Chiave computer**.
    12. Per la chiave di convalida, deseleziona **Genera automaticamente in fase di esecuzione** e quindi incolla la chiave copiata nel passaggio g.
    13. Fare clic su **Applica**.
    14. Completa questo processo per le pagine **RDWeb**, **Feed**, **FeedLogon** e **Pages**.
    15. Riduci a icona la finestra Connessione Desktop remoto per il secondo server Accesso Web Desktop remoto e quindi ingrandisci la finestra Connessione Desktop remoto per il primo server Accesso Web Desktop remoto.  
    16. Ripeti i passaggi g-n per copiare la chiave di decrittografia.
    17. Quando le chiavi di convalida e quelle di decrittografia sono identiche in entrambi i server Accesso Web Desktop remoto per le pagine **RDWeb**, **Feed**, **FeedLogon** e **Pages**, disconnettiti da tutte le finestre Connessione Desktop remoto.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Passaggio 3: Configurare il bilanciamento del carico per i server Web e Gateway Desktop remoto

Se usi l'infrastruttura di Azure, puoi creare un servizio di bilanciamento del carico di Azure esterno. In caso contrario, puoi impostare un servizio di bilanciamento del carico hardware o software separato. Il bilanciamento del carico è fondamentale per distribuire equamente il traffico sulle connessioni di lunga durata dai client Desktop remoto, attraverso il Gateway Desktop remoto, ai server in cui gli utenti eseguiranno i rispettivi carichi di lavoro.

> [!NOTE] 
> Se il server precedente con il ruolo Web Desktop remoto e Gateway Desktop remoto era già impostato dietro un servizio di bilanciamento del carico esterno, passa direttamente al passaggio 4, seleziona il pool back-end esistente e aggiungi il nuovo server al pool.

1.  Crea un servizio di bilanciamento del carico di Azure:  
    1.  Nel portale di Azure fai clic su **Sfoglia > Servizi di bilanciamento del carico > Aggiungi**.  
    2.  Immetti un nome, ad esempio **WebGwLB**.  
    3.  Seleziona **Pubblico** per **Schema**.
    4.  In **Indirizzo IP pubblico** seleziona **Scegliere un indirizzo IP pubblico** e quindi seleziona un indirizzo IP pubblico esistente o creane uno nuovo.
    5.  Seleziona valori appropriati per **Sottoscrizione**, **Gruppo risorse** e **Percorso**.
    6.  Fare clic su **Crea**.  
2. Crea un [probe](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) per monitorare quali server sono attivi:  
    1.  Nel portale di Azure seleziona **Sfoglia** > **Bilanciamenti del carico** e quindi scegli il servizio di bilanciamento del carico creato nel passaggio precedente.
    2.  Seleziona **Tutte le impostazioni** > **Probe** > **Aggiungi**.  
    3.  Immetti un nome, ad esempio **HTTPS**, per il probe. Seleziona **TCP** come **Protocollo** e immetti **443** per il campo **Porta** e quindi fai clic su **OK**.   
3.  Crea le regole di bilanciamento del carico HTTPS e UDP:  
    1.  In **Impostazioni** fai clic su **Regole di bilanciamento del carico**.  
    2.  Seleziona **Aggiungi** per **HTTPS rule** (Regola HTTPS).  
    3.  Immetti un nome per la regola, ad esempio HTTPS, e seleziona **TCP** come **Protocollo**. Immetti **443** sia nel campo **Porta** che nel campo **Porta back-end** e fai clic su **OK**.  
    4.  In **Regole di bilanciamento del carico** fai clic su **Aggiungi** per **UDP rule** (Regola UDP).  
    5.  Immetti un nome per la regola, ad esempio **UDP**, e seleziona **UDP** come **Protocollo**. Immetti **3391** sia nel campo **Porta** che nel campo **Porta back-end** e fai clic su **OK**.  
4. Crea il pool back-end per i server Web Desktop remoto e Gateway Desktop remoto:
      1. In **Impostazioni** fai clic su **Pool di indirizzi back-end > Aggiungi**.   
      2. Immetti un nome (ad esempio, **WebGwBackendPool**) e quindi fai clic su **Aggiungi una macchina virtuale**.  
      3. Scegli un set di disponibilità (ad esempio, WebGwAvSet) e quindi fai clic su **OK**.   
      4. Fai clic su **Scegli le macchine virtuali**, seleziona ogni macchina virtuale e quindi fai clic su **Seleziona > OK > OK**.
