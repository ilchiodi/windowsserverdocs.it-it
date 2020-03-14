---
title: 'Passaggio 2: Configurare WSUS'
description: 'Argomento di Windows Server Update Services (WSUS): Configurare WSUS è il secondo passaggio di un processo in quattro passaggi per la distribuzione di WSUS'
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1a78d2006a45bb2af8f87a91d7bb888964ddbcb
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323503"
---
# <a name="step-2-configure-wsus"></a>Passaggio 2: Configurare WSUS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una volta completata l'installazione del ruolo del server WSUS (Windows Server Update Services) nel server, è necessario configurarlo correttamente. L'elenco seguente riassume i passaggi necessari per eseguire la configurazione iniziale per il server WSUS.

|Attività|Description|
|----|--------|
|[2.1. Configurare le connessioni di rete](#21-configure-network-connections)|Configurare la rete di cluster tramite la Configurazione guidata rete.|
|[2.2. Configurare WSUS tramite la Configurazione guidata WSUS](#22-configure-wsus-by-using-the-wsus-configuration-wizard)|Usare la Configurazione guidata WSUS per definire la configurazione di base di WSUS.|
|[2.3. Configurare gruppi di computer WSUS](#23-configure-wsus-computer-groups)|Creare gruppi di computer nella console di amministrazione di WSUS per gestire gli aggiornamenti all'interno dell'organizzazione.|
|[2.4. Configurare gli aggiornamenti client](#24-configure-client-updates)|Specificare come e quando applicare gli aggiornamenti automatici ai computer client.|
|[2.5. Proteggere WSUS con il protocollo Secure Sockets Layer](#25-secure-wsus-with-the-secure-sockets-layer-protocol)|Configurare il protocollo SSL (Secure Sockets Layer) per la protezione di Windows Server Update Services (WSUS).|

## <a name="21-configure-network-connections"></a>2.1. Configurare le connessioni di rete
Prima di avviare il processo di configurazione, accertarsi di essere in grado di rispondere alle domande seguenti.

1.  Il firewall del server è configurato per consentire ai client di accedere al server?

2.  Il computer può essere connesso al server upstream: ad esempio, il server designato per eseguire il download degli aggiornamenti da Microsoft Update?

3.  Se necessario, si conoscono il nome del server proxy e le relative credenziali utente?

Per impostazione predefinita, WSUS è configurato per l'utilizzo di Microsoft Update come origine degli aggiornamenti. Se nella rete è presente un server proxy, è possibile configurare WSUS per l'uso del server proxy. Se tra WSUS e Internet è presente un firewall aziendale, può essere necessario configurare il firewall per consentire a WSUS di scaricare gli aggiornamenti.

> [!TIP]
> Anche se la connessione a Internet è necessaria per scaricare gli aggiornamenti da Microsoft Update, WSUS consente di importare gli aggiornamenti in reti non connesse a Internet.

Quando si è in grado di rispondere a queste domande, è possibile iniziare a configurare le seguenti impostazioni di rete di WSUS:

-   **Aggiornamenti** Specificare come il server acquisisce gli aggiornamenti: da Microsoft Update o da un altro server WSUS.

-   **Proxy** Se è necessario usare un server proxy per l'accesso di WSUS a Internet, configurare le impostazioni proxy nel server WSUS.

-   **Firewall** Se WSUS è posizionato dietro un firewall aziendale, è necessario effettuare alcuni passaggi aggiuntivi nel dispositivo periferico per consentire il traffico corretto di WSUS.

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. Connessione dal server WSUS a Internet
Se tra WSUS e Internet è presente un firewall aziendale, potrebbe essere necessario configurare il firewall per consentire a WSUS di scaricare gli aggiornamenti. Per ottenere gli aggiornamenti da Microsoft Update, il server WSUS usa la porta 443 per il protocollo HTTPS. Sebbene la maggior parte dei firewall aziendali consenta questo tipo di traffico, esistono alcune aziende limitano l'accesso a Internet dal server a causa dei criteri di sicurezza della società. In tal caso, è necessario ottenere l'autorizzazione per consentire l'accesso a Internet da WSUS per gli URL elencati di seguito:

- http\://windowsupdate.microsoft.com

- http\://\*.windowsupdate.microsoft.com

- https\://\*.windowsupdate.microsoft.com

- http\://\*.update.microsoft.com

- https\://\*.update.microsoft.com

- http\://\*.windowsupdate.com

- http\://download.windowsupdate.com

- https\://download.microsoft.com

- http\://\*.download.windowsupdate.com

- http\://wustat.windows.com

- http\://ntservicepack.microsoft.com

- http\://go.microsoft.com

- http\://dl.delivery.mp.microsoft.com

- https\://dl.delivery.mp.microsoft.com

> [!IMPORTANT]
> Per uno scenario in cui Windows Server Update SERVICES non riesce a ottenere gli aggiornamenti a causa di configurazioni del firewall, vedere [articolo 885819](https://support.microsoft.com/kb/885819) della Microsoft Knowledge Base.

La procedura seguente descrive come configurare un firewall aziendale posizionato tra WSUS e Internet. Dal momento che WSUS avvia tutto il traffico di rete, non è necessario configurare Windows Firewall nel server WSUS. Sebbene la connessione tra Microsoft Update e WSUS richieda l'apertura delle porte 80 e 443, è possibile configurare più server WSUS per la sincronizzazione con una porta personalizzata.

### <a name="212-connection-between-wsus-servers"></a>2.1.2. Connessione tra server WSUS
I server WSUS upstream e downstream verranno sincronizzati sulla porta configurata dall'amministratore di WSUS. Per impostazione predefinita, queste porte sono configurate come indicato di seguito:

-   In WSUS 3.2 e versioni precedenti, porta 80 per HTTP e 443 per HTTPS

-   In Windows Server Update SERVICES 6.2 e versioni successive (almeno Windows Server 2012), la porta 8530 per HTTP e 8531 per HTTPS vengono usati

Il firewall nel server WSUS deve essere configurato per consentire il traffico in entrata su queste porte.

### <a name="213-connection-between-clients-windows-update-agent-and-wsus-servers"></a>2.1.3. Connessione tra client (Agente di Windows Update) e server WSUS
Le porte e le interfacce in ascolto vengono configurate nei siti IIS per WSUS e nelle impostazioni di Criteri di gruppo usate per configurare i PC client. Le porte predefinite corrispondono a quelle specificate nella sezione precedente, **Connessione tra server WSUS**, e il firewall nel server WSUS deve essere configurato anche per consentire il traffico in entrata su queste porte.

## <a name="configure-the-proxy-server"></a>Configurare il server proxy
Se la rete aziendale usa server proxy, questi devono supportare i protocolli HTTP e SSL e usare l'autenticazione di base o l'autenticazione di Windows. Questi requisiti possono essere soddisfatti usando una delle configurazioni seguenti:

1.  Un singolo server proxy che supporta due canali di protocollo. In questo caso, impostare un canale per usare HTTP e l'altro per HTTPS.

    > [!NOTE]
    > È possibile configurare un server proxy che gestisce entrambi i protocolli per WSUS durante l'installazione del prodotto server WSUS.

2.  Due server proxy, ognuno dei quali supporta un singolo protocollo. In questo caso, un server proxy è configurato per usare HTTP e l'altro per HTTPS.

Per configurare due server proxy, ognuno dei quali gestirà un protocollo per WSUS, seguire questa procedura:

#### <a name="to-set-up-wsus-to-use-two-proxy-servers"></a>Per configurare WSUS per usare due server proxy

1.  Accedere al computer che dovrà fungere da server WSUS con un account membro del gruppo Administrators locale.

2.  Installare il ruolo del server WSUS. Durante la Configurazione guidata WSUS (illustrata nella sezione successiva) non specificare un server proxy.

3.  Aprire un prompt dei comandi (Cmd.exe) come amministratore. Per aprire un prompt dei comandi come amministratore, fare clic su **Start**. In **Inizia ricerca** digitare **Prompt dei comandi**. Nella parte superiore del menu Start fare clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegliere **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente**, immettere le credenziali appropriate (se richiesto), confermare che l'azione indicata è quella che si vuole eseguire e quindi fare clic su **Continua**.

4.  Nella finestra del prompt dei comandi passare alla cartella C:\Programmi\Update Services\Tools. Digitare il comando seguente:

    **wsusutil ConfigureSSlproxy [< proxy_server proxy_port>] -enable**, dove:

    1.  server_proxy è il nome del server proxy che supporta HTTPS.

    2.  porta_proxy è il numero di porta del server proxy.

5.  Chiudere la finestra del prompt dei comandi.

Per aggiungere il server proxy che usa il protocollo HTTP alla configurazione di WSUS, seguire questa procedura:

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>Per aggiungere un server proxy che usa il protocollo HTTP

1.  Aprire la console di amministrazione di WSUS.

2.  Nel riquadro sinistro espandere il nome del server e quindi fare clic su **Opzioni**.

3.  Nel riquadro **Opzioni** fare clic su **Origine aggiornamenti e server proxy**e quindi fare clic sulla scheda **Server proxy** .

4.  Usare le opzioni seguenti per modificare la configurazione del server proxy esistente:

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>Per modificare o aggiungere un server proxy nella configurazione di WSUS

    1.  Selezionare la casella di controllo accanto a **Usa un server proxy per la sincronizzazione**.

    2.  Nella casella di testo **Nome server proxy** digitare il nome del server proxy.

    3.  Nella casella di testo **Numero della porta del proxy** digitare il numero di porta del server proxy. Il numero di porta predefinito è 80.

    4.  Se il server proxy richiede l'uso di un account utente specifico, selezionare la casella di controllo **Usa credenziali utente per la connessione al server proxy**. Digitare le informazioni richieste relative a nome utente, dominio e password nelle caselle di testo corrispondenti.

    5.  Se il server proxy supporta l'autenticazione di base, selezionare la casella di controllo **Consenti autenticazione di base (la password viene inviata come testo non crittografato)** .

    6.  Fare clic su **OK**.

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>Per rimuovere un server proxy dalla configurazione di WSUS

    1.  Per rimuovere un server proxy dalla configurazione di WSUS, deselezionare la casella di controllo **Usa un server proxy per la sincronizzazione**.

    2.  Fare clic su **OK**.

## <a name="22-configure-wsus-by-using-the-wsus-configuration-wizard"></a>2.2. Configurare WSUS tramite la Configurazione guidata WSUS
Questa procedura presuppone l'utilizzo della Configurazione guidata WSUS visualizzata al primo avvio della console di gestione di Windows Server Update Services. Più avanti in questo argomento, viene illustrato come effettuare le configurazioni utilizzando la pagina **Opzioni** .

#### <a name="to-configure-wsus"></a>Per configurare WSUS

1.  Nel riquadro di spostamento di Server Manager scegliere **Dashboard**, **Strumenti**, quindi fare clic su **Windows Server Update Services**.

    > [!NOTE]
    > Se viene visualizzata la finestra di dialogo **Completamento installazione WSUS**, fare clic su **Esegui**. Nella finestra di dialogo **Completamento installazione WSUS** fare clic su **Chiudi** al termine dell'installazione.

2.  Viene visualizzata la procedura guidata di Windows Server Update Services. Nella pagina **Prima di iniziare** esaminare le informazioni e quindi fare clic su **Avanti**.

3.  Leggere le istruzioni nella pagina **Partecipa al programma Analisi utilizzo Microsoft Update** e valutare se prendervi parte. Se si vuole partecipare al programma, lasciare la selezione predefinita. In caso contrario, deselezionare la casella di controllo e fare clic su **Avanti**.

4.  Nella pagina **Scelta server upstream** sono disponibili due opzioni:

    1.  Sincronizzare gli aggiornamenti con Microsoft Update

    2.  Sincronizzare da un altro server Windows Server Update Services

        -   Se si sceglie di eseguire la sincronizzazione da un altro server WSUS, specificare il nome del server e la porta per la comunicazione con il server upstream.

        -   Per utilizzare SSL, selezionare la casella di controllo **Usa SSL per la sincronizzazione delle informazioni sugli aggiornamenti** . I server utilizzeranno la porta 443 per la sincronizzazione. Verificare che questo server e il server upstream supportino SSL.

        -   Se si tratta di un server di replica, selezionare la casella di controllo **Replica del server upstream**.

5.  Dopo aver selezionato le opzioni appropriate per la distribuzione, fare clic su **Avanti** per continuare.

6.  Nella pagina **Specifica server proxy** selezionare la casella di controllo **Usa un server proxy per la sincronizzazione** , quindi immettere il nome del server proxy e il numero della porta utilizzata nelle caselle corrispondenti. La porta 80 è la porta predefinita.

    > [!IMPORTANT]
    > Se WSUS necessita di un server proxy per accedere a Internet, è necessario completare questo passaggio.

7.  Se si vuole eseguire la connessione al server proxy usando le credenziali di un utente specifico, selezionare la casella di controllo **Usa credenziali utente per la connessione al server proxy** e quindi digitare il nome utente, il domino e la password dell'utente nelle caselle corrispondenti. Se si vuole abilitare l'autenticazione di base dell'utente che effettua la connessione al server proxy, selezionare la casella di controllo **Consenti autenticazione di base (la password viene inviata come testo non crittografato)** .

8.  Fare clic su **Avanti**. Nella pagina **Connessione al server upstream** fare clic su **Avvia connessione**.

9. Effettuata la connessione, scegliere **Avanti** per continuare.

10. Nel **scelta lingue** pagina, è possibile selezionare le lingue da cui WSUS riceverà gli aggiornamenti - tutte le lingue o un sottoinsieme di lingue. La selezione di un sottoinsieme di lingue consente di risparmiare spazio su disco. È tuttavia IMPORTANTE scegliere tutte le lingue necessarie per tutti i client del server WSUS. Se si decide di ottenere gli aggiornamenti solo per determinate lingue, selezionare **Scarica aggiornamenti solo nelle lingue seguenti** e quindi le lingue delle quali si vogliono ricevere gli aggiornamenti. Altrimenti, lasciare la selezione predefinita.

    > [!WARNING]
    > Se si seleziona l'opzione **Scarica aggiornamenti solo nelle lingue seguenti**quando al server è connesso un server WSUS downstream, anche quest'ultimo sarà forzato a utilizzare solo le lingue selezionate.

11. Dopo aver selezionato le opzioni linguistiche appropriate per la distribuzione, fare clic su **Avanti** per continuare.

12. Nella pagina **Scelta prodotti** è possibile specificare i prodotti per cui si desiderano gli aggiornamenti. Selezionare le categorie dei prodotti, ad esempio Windows, o prodotti specifici, ad esempio Windows Server 2008. Se si seleziona una categoria di prodotti, verranno selezionati tutti i prodotti in essa contenuti.

13. Selezionare le opzioni di prodotto appropriate per la distribuzione e quindi fare clic su **Avanti**.

14. Nella pagina **Scelta classificazioni** selezionare le classificazioni degli aggiornamenti che si desidera ottenere. Scegliere tutte le classificazioni o un loro sottoinsieme e quindi fare clic su **Avanti**.

15. La pagina **Imposta pianificazione della sincronizzazione** consente di scegliere se eseguire la sincronizzazione in modo manuale o automatico.

    -   Se si sceglie l'opzione **Sincronizzazione manuale**, è necessario avviare il processo di sincronizzazione dalla console di amministrazione di WSUS.

    -   Se si sceglie l'opzione **Sincronizzazione automatica**, il server WSUS verrà sincronizzato a intervalli specifici.

    Impostare l'ora relativa a **Prima sincronizzazione**e quindi specificare il numero di **Sincronizzazioni al giorno** che si vuole vengano eseguite dal server. Se, ad esempio, si specificano quattro sincronizzazioni giornaliere a partire dalle 3:00, queste verranno eseguite alle 3:00, alle 9:00, alle 15:00 e alle 21:00.

16. Dopo aver selezionato le opzioni di sincronizzazione appropriate per la distribuzione, fare clic su **Avanti** per continuare.

17. Nella pagina **Operazione completata** è ora possibile avviare la sincronizzazione selezionando la casella di controllo **Avvia sincronizzazione iniziale** . Se non si seleziona questa opzione, per eseguire la sincronizzazione iniziale è necessario usare la console di amministrazione di WSUS. Per informazioni sulle impostazioni aggiuntive, fare clic su **Avanti** ; altrimenti, scegliere **Fine** per terminare la procedura guidata e completare la configurazione iniziale di Windows Server Update Services.

18. Dopo aver scelto **Fine**, viene visualizzata la console di amministrazione di WSUS.

Completata la configurazione di base di Windows Server Update Services, leggere le prossime sezioni se si desiderano maggiori informazioni su come modificare le impostazioni utilizzando la console di amministrazione di WSUS.

## <a name="23-configure-wsus-computer-groups"></a>2.3. Configurare gruppi di computer WSUS
I gruppi di computer rappresentano una parte molto IMPORTANTE della distribuzione di Windows Server Update Services (WSUS) in quanto consentono di testare e distribuire gli aggiornamenti a specifici computer. Ci sono due gruppi di computer predefiniti: Tutti i computer e Computer non assegnati. Per impostazione predefinita, un computer client che contatta il server di WSUS per la prima volta viene aggiunto da quest'ultimo a ciascun gruppo.

È possibile creare tutti i gruppi di computer personalizzati necessari alla gestione degli aggiornamenti all'interno della propria organizzazione. È consigliabile creare almeno un gruppo di computer per il test degli aggiornamenti, prima di procedere alla distribuzione sugli altri computer dell'organizzazione.

Utilizzare la procedura seguente per creare un nuovo gruppo e assegnarvi un computer.

#### <a name="to-create-a-computer-group"></a>Per creare un gruppo di computer

1.  Nella console di amministrazione di WSUS in **Update Services** espandere il server WSUS, espandere **Computer**, fare clic con il pulsante destro del mouse su **Tutti i computer** e quindi scegliere **Aggiungi gruppo di computer**.

2.  Nella finestra di dialogo **Aggiungi gruppo di computer**, in **Nome**, specificare il nome del nuovo gruppo e quindi fare clic su **Aggiungi**.

3.  Fare clic su **Computer** e quindi selezionare i computer da assegnare al nuovo gruppo.

4.  Fare clic con il pulsante destro del mouse sui nomi dei computer selezionati nel passaggio precedente e quindi fare clic su **Modifica appartenenza**.

5.  Nella finestra di dialogo **Imposta appartenenza a gruppi di computer** selezionare il gruppo test creato e quindi fare clic su **OK**.

## <a name="24-configure-client-updates"></a>2.4. Configurare gli aggiornamenti client
Durante l'installazione di WSUS, IIS viene configurato automaticamente per distribuire la versione più recente di Aggiornamenti automatici a tutti i computer client che contattano il server WSUS. La configurazione ottimale di Aggiornamenti automatici varia in base all'ambiente di rete utilizzato.

-   In un ambiente che usa il servizio directory Active Directory, è possibile usare un oggetto Criteri di gruppo basato sul dominio esistente oppure crearne uno nuovo.

-   In un ambiente senza Active Directory, usare Editor Criteri di gruppo locali per configurare Aggiornamenti automatici e quindi puntare i computer client al server WSUS.

> [!IMPORTANT]
> Le procedure seguenti presuppongono l'esecuzione di Active Directory nella rete. Presuppongono inoltre che si abbia familiarità con Criteri di gruppo e lo si utilizzi per gestire la rete.

Utilizzare le procedure riportate di seguito per configurare Aggiornamenti automatici per i computer client.

-   [Passaggio 4: Configurare le impostazioni di criteri di gruppo per gli aggiornamenti automatici](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2.3. Configurare gruppi di computer](#23-configure-wsus-computer-groups) in questo argomento

### <a name="configure-automatic-updates-in-group-policy"></a>Configurare Aggiornamenti automatici in Criteri di gruppo

Se nella rete è stato installato Active Directory, è possibile configurare uno o più computer contemporaneamente inserendoli in un oggetto Criteri di gruppo. Configurare quindi quest'ultimo con le impostazioni WSUS. Si consiglia di creare un nuovo oggetto Criteri di gruppo che contenga solo le impostazioni WSUS.

Collegare l'oggetto Criteri di gruppo WSUS a un contenitore Active Directory appropriato per l'ambiente in uso. In un ambiente semplice è possibile collegare un solo oggetto Criteri di gruppo WSUS al dominio. In un ambiente più complesso, è possibile collegare più oggetti Criteri di gruppo WSUS a diverse unità organizzative, in modo da applicare impostazioni di criteri WSUS diverse a tipi di computer differenti.

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>Per abilitare WSUS tramite un oggetto Criteri di gruppo basato su dominio

1.  In Console Gestione Criteri di gruppo selezionare l'oggetto Criteri di gruppo per il quale si vuole configurare WSUS e fare clic su **Modifica**.

2.  In Console Gestione Criteri di gruppo espandere **Configurazione computer**, **Criteri**, **Modelli amministrativi**, **Componenti di Windows** e quindi fare clic su **Windows Update**.

3.  Nel riquadro dei dettagli fare doppio clic su **Configura Aggiornamenti automatici**. Verrà aperto il criterio **Configura Aggiornamenti automatici** .

4.  Fare clic su **Abilitato**e quindi selezionare una delle opzioni seguenti sotto l'impostazione **Configura aggiornamento automatico** :

    -   **Avviso per download e installazione**. Questa opzione avvisa gli utenti con privilegi amministrativi connessi prima che vengano scaricati e installati gli aggiornamenti.

    -   **Download automatico e avviso per l'installazione**. Questa opzione avvia automaticamente il download degli aggiornamenti e avvisa gli utenti con privilegi amministrativi connessi prima che vengano installati gli aggiornamenti. Questa opzione è selezionata per impostazione predefinita.

    -   **Download automatico e pianificazione dell'installazione**. Questa opzione avvia automaticamente il download degli aggiornamenti e li installa nel giorno e all'ora specificati.

    -   **Consenti scelta impostazioni all'amministratore locale**. Questa opzione consente agli amministratori locali di utilizzare Aggiornamenti automatici nel Pannello di controllo per selezionare un'opzione di configurazione, ad esempio, l'ora di installazione pianificata. Gli amministratori locali non possono disabilitare Aggiornamenti automatici.

5.  Selezionare **Abilita appartenenza gruppo destinazione**, scegliere **Abilitato** e quindi digitare il nome del gruppo di computer WSUS a cui si vuole aggiungere il computer nella casella **Nome gruppo di destinazione del computer**.

    > [!NOTE]
    > L'opzione**Abilita appartenenza gruppo destinazione** consente ai computer client di aggiungersi ai gruppi di computer di destinazione nel server WSUS quando viene eseguito il reindirizzamento di Aggiornamenti automatici a un server WSUS. Se lo stato è impostato su Abilitato, il computer identificherà se stesso come membro di un determinato gruppo di computer quando invia informazioni al server WSUS, che usa i dati per determinare quali aggiornamenti vengono distribuiti nel computer. Questa impostazione indica al server WSUS il gruppo che il computer client userà. È necessario creare il gruppo nel server WSUS e aggiungere al gruppo i computer membri del dominio.

6.  Fare clic su **OK** per chiudere il criterio **Abilita appartenenza gruppo destinazione** e tornare al riquadro dei dettagli di Windows Update.

7.  Fare clic su **OK** per chiudere il criterio **Configura Aggiornamenti automatici** e tornare al riquadro dei dettagli di Windows Update.

8.  Nel riquadro dei dettagli di **Windows Update** fare doppio clic su **Specifica il percorso del servizio di aggiornamento Microsoft nella rete Intranet**.

9. Fare clic su **Abilitato**e quindi nelle caselle di testo **Impostare il servizio di aggiornamento nella rete Intranet per il rilevamento degli aggiornamenti** e **Impostare il server per le statistiche nella rete Intranet** digitare lo stesso URL del server WSUS. Digitare, ad esempio, *http://servername* in entrambe le caselle, dove *nomeserver* è il nome del server WSUS.

    > [!WARNING]
    > Durante l'immissione dell'indirizzo Intranet del server WSUS accertarsi di specificare la porta da utilizzare. Per impostazione predefinita, WSUS utilizza le porte 8530 per HTTP e 8531 per HTTPS. Se si usa il protocollo HTTP, ad esempio, è necessario digitare **http://servername:8530** .

10. Fare clic su **OK**.

Al termine dell'impostazione, il computer client verrà visualizzato nella pagina **Computer** della console di amministrazione di WSUS dopo alcuni minuti. Se i computer client sono configurati con un oggetto Criteri di gruppo basato su dominio, per applicare le nuove impostazioni dei criteri al computer client tramite Criteri di gruppo saranno necessari circa 20 minuti. Per impostazione predefinita, gli aggiornamenti di criteri di gruppo in background ogni 90 minuti, con un offset casuale compreso tra 0 a 30 minuti. Se si vuole eseguire l'aggiornamento di Criteri di gruppo prima di questo intervallo di tempo, è possibile aprire una finestra del prompt dei comandi nel computer client e digitare gpupdate /force.

Se i computer client sono configurati mediante Editor Criteri di gruppo locali, l'oggetto Criteri di gruppo viene applicato immediatamente. L'aggiornamento diverrà effettivo dopo circa 20 minuti. Se si esegue il rilevamento manualmente, non sarà necessario attendere 20 minuti per il contatto tra il computer client e WSUS.

Dal momento che l'attesa per l'avvio del rilevamento può richiedere molto tempo, è possibile utilizzare la procedura seguente per avviare immediatamente il rilevamento.

##### <a name="to-initiate-wsus-detection"></a>Per avviare il rilevamento di WSUS

1.  Nel computer client aprire una finestra del prompt dei comandi con privilegi elevati.

2.  Digitare wuauclt.exe /detectnow e quindi premere INVIO.

## <a name="25-secure-wsus-with-the-secure-sockets-layer-protocol"></a>2.5. Proteggere WSUS con il protocollo Secure Sockets Layer

È possibile usare il protocollo Secure Sockets Layer (SSL) per proteggere la distribuzione di WSUS. WSUS utilizza SSL per l'autenticazione tra computer client e server WSUS downstream e il server WSUS. WSUS usa anche SSL per crittografare i metadati di aggiornamento.

> [!IMPORTANT]
> I client e i server downstream che sono configurati per usare Transport Layer Security (TLS) o HTTPS devono anche essere configurati per usare un nome di dominio completo (FQDN) per il server WSUS upstream.

WSUS usa SSL solo per i metadati, non per i file di aggiornamento. Ciò corrisponde al modo in cui Microsoft Update distribuisce gli aggiornamenti. Microsoft riduce il rischio di invio dei file di aggiornamento tramite un canale non crittografato firmando ogni aggiornamento. Viene inoltre calcolato un hash, che viene inviato con i metadati per ogni aggiornamento. Quando un aggiornamento viene scaricato, WSUS verifica la firma digitale e l'hash. Se l'aggiornamento è stato modificato, non viene installato.

### <a name="limitations-of-wsus-ssl-deployments"></a>Limitazioni delle distribuzioni SSL di WSUS
Quando si usa SSL per proteggere una distribuzione di WSUS, è necessario prendere in considerazione le limitazioni seguenti:

1.  L'uso di SSL fa aumentare il carico di lavoro del server. È necessario prevedere un calo del 10% delle prestazioni a causa del costo della crittografia di tutti i metadati inviati in rete.

2.  Se si usa WSUS con un database di SQL Server remoto, la connessione tra il server WSUS e il server di database non è protetta da SSL. Se la connessione al database deve essere protetta, prendere in considerazione i consigli seguenti:

-   Spostare il database WSUS nel server WSUS.

-   Spostare il server di database remoto e il server WSUS in una rete privata.

-   Distribuire Internet Protocol Security (IPsec) per proteggere il traffico di rete. Per altre informazioni su IPsec, vedere [Creazione e uso dei criteri IPsec](https://go.microsoft.com/fwlink/?LinkID=203841).

### <a name="configure-ssl-on-the-wsus-server"></a>Configurare SSL nel server WSUS
WSUS richiede due porte per SSL: una porta che usa HTTPS per inviare i metadati crittografati e una porta che usa HTTP per inviare gli aggiornamenti. Quando si configura WSUS per l'uso di SSL, considerare quanto segue:

-   Non è possibile configurare l'intero sito Web WSUS in modo da richiedere SSL, perché tutto il traffico al sito WSUS dovrebbe essere crittografato. WSUS crittografa solo i metadati di aggiornamento. Se un computer tenta di recuperare file di aggiornamento sulla porta HTTPS, il trasferimento non riesce.

    È necessario richiedere SSL solo per le radici virtuali seguenti:

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    Non richiedere SSL per le radici virtuali seguenti:

    -   **Contenuto**

    -   **Inventory**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   Il certificato dell'autorità di certificazione (CA) deve essere importato nell'archivio delle autorità di certificazione radice attendibili del computer locale o di Windows Server Update Services nei server WSUS downstream. Se il certificato viene importato solo nell'archivio delle autorità di certificazione radice attendibili dell'utente locale, il server WSUS downstream non viene autenticato nel server upstream.

    Per altre informazioni su come usare i certificati SSL in IIS, vedere [Richiedere Secure Sockets Layer (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846).

-   È necessario importare il certificato in tutti i computer che comunicheranno con il server WSUS. Sono inclusi tutti i computer client, i server downstream e i computer che eseguono la console di amministrazione di WSUS. Il certificato deve essere importato nell'archivio delle autorità di certificazione radice attendibili del computer locale o di Windows Server Update Services.

-   È possibile usare qualsiasi porta per SSL. Tuttavia, la porta configurata per SSL determina anche la porta usata da WSUS per inviare il traffico HTTP non crittografato. Vedere gli esempi seguenti:

    -   Se si usa la porta standard del settore 443 per il traffico HTTPS, WSUS usa la porta standard del settore 80 per il traffico HTTP non crittografato.

    -   Se si usa qualsiasi altra porta diversa dalla porta 443 per il traffico HTTPS, WSUS invia il traffico HTTP non crittografato attraverso la porta che precede numericamente la porta per HTTPS. Ad esempio, se si usa la porta 8531 per HTTPS, WSUS userà la porta 8530 per HTTP.

-   È necessario inizializzare nuovamente *ClientServicingProxy* se il nome del server, la configurazione SSL o il numero di porta viene modificato.

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>Per configurare SSL nel server radice WSUS

1.  Accedere al server WSUS con un account membro del gruppo WSUS Administrators o del gruppo Administrators locale.

2.  Fare clic su **Start**, digitare **CMD**, fare clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegliere **Esegui come amministratore**.

3.  Passare alla cartella _%Programmi%_ **\\Update Services\\Tools\\** .

4.  Nella finestra del prompt dei comandi digitare il comando seguente:

    **Wsusutil configuressl**_nomeCertificato_

    dove:

    *nomeCertificato* è il nome DNS del server WSUS.

### <a name="configure-ssl-on-client-computers"></a>Configurare SSL nei computer client
Quando si configura SSL nei computer client, è necessario considerare le problematiche seguenti:

-   È necessario includere un URL per una porta sicura nel server WSUS. Poiché non è possibile richiedere SSL nel server, l'unico modo per assicurarsi che i computer client possano usare un canale di sicurezza consiste nell'usare un URL che specifichi HTTPS. Se si usa qualsiasi altra porta diversa da 443 per SSL, è necessario includere anche tale porta nell'URL.

-   Il certificato in un computer client deve essere importato nell'archivio delle autorità di certificazione radice attendibili del computer locale o del servizio Aggiornamenti automatici. Se il certificato viene importato solo nell'archivio delle autorità di certificazione radice attendibili dell'utente locale, l'autenticazione server per Aggiornamenti automatici non riesce.

-   I computer client devono considerare attendibile il certificato associato al server WSUS. A seconda del tipo di certificato usato, potrebbe essere necessario configurare un servizio per consentire ai computer client di considerare attendibile il certificato associato al server WSUS.

### <a name="configure-ssl-for-downstream-wsus-servers"></a>Configurare SSL per i server WSUS downstream
Le istruzioni seguenti consentono di configurare un server downstream per la sincronizzazione con un server upstream che usa SSL.

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>Per sincronizzare un server downstream con un server upstream che usa SSL

1.  Accedere al computer con un account membro del gruppo Administrators locale o del gruppo WSUS Administrators.

2.  Fare clic su **Start**, **Tutti i programmi**, **Strumenti di amministrazione** e quindi su **Windows Server Update Services**.

3.  Nel riquadro destro espandere il nome del server.

4.  Fare clic su **Opzioni**e quindi su **Origine aggiornamenti e server proxy**.

5.  Nella pagina **Origine aggiornamenti** selezionare **Sincronizza da un altro server di Windows Server Update Services**.

6.  Digitare il nome del server upstream nella casella di testo **Nome server**. Digitare il numero di porta usato dal server per le connessioni SSL nella casella di testo **Numero porta**.

7.  Selezionare la casella di controllo **Usa SSL per la sincronizzazione delle informazioni sugli aggiornamenti** e quindi fare clic su **OK**.

### <a name="additional-ssl-resources"></a>Risorse aggiuntive su SSL
I passaggi necessari per configurare un'autorità di certificazione, associare il certificato al sito Web WSUS e stabilire un trust tra i computer client e il certificato non rientrano nell'ambito di questa guida. Per altre informazioni e per istruzioni su come installare i certificati e configurare questo ambiente, vedere gli argomenti seguenti:

-   [Guida dettagliata all'infrastruttura a chiave pubblica Suite B](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [Implementazione e amministrazione di modelli di certificato](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [Guida alla migrazione e all'aggiornamento di Servizi certificati Active Directory](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [Configurare la registrazione automatica dei certificati](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. Completare la configurazione di IIS
Per impostazione predefinita, l'accesso in lettura anonimo è abilitato in tutti i siti Web IIS nuovi e in quello predefinito. Alcune applicazioni, in particolare Windows SharePoint Services, possono rimuovere l'accesso anonimo. In questo caso, è necessario abilitare nuovamente l'accesso in lettura anonimo prima di poter installare e usare correttamente WSUS.

Per abilitare l'accesso in lettura anonimo, seguire i passaggi per la versione applicabile di IIS:

1.  [Abilitare l'autenticazione anonima (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316), come illustrato nella Guida operativa di IIS 7.

2.  [Abilitazione dell'autenticazione anonima (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391),come illustrato nella Guida operativa di IIS 6.0.

### <a name="27-configure-a-signing-certificate"></a>2.7. Configurare un certificato di firma
WSUS è in grado di pubblicare pacchetti di aggiornamento personalizzati per aggiornare prodotti Microsoft e non Microsoft. WSUS può firmare automaticamente questi pacchetti di aggiornamento personalizzati per l'utente con un certificato Authenticode. Per abilitare la firma degli aggiornamenti personalizzati, è necessario installare un certificato di firma dei pacchetti nel server WSUS. Ci sono diverse considerazioni associate alla firma degli aggiornamenti personalizzati.

1.  **Distribuzione del certificato**. La chiave privata deve essere installata nel server WSUS e la chiave pubblica deve essere installata in modo esplicito nell'archivio di certificati attendibili in tutti i PC client e i server che devono ricevere gli aggiornamenti con firma personalizzata.

2.  **Scadenza**. Quando il certificato autofirmato scade o sta per scadere, WSUS registrerà gli eventi nel registro eventi.

3.  **Revoca/aggiornamenti del certificato**. Se si vuole aggiornare o revocare un certificato (ad esempio dopo averne rilevato la scadenza), WSUS non offre funzionalità per questa operazione. A tale scopo, è necessaria un'attività manuale molto complessa da svolgere manualmente o automatizzare correttamente.


