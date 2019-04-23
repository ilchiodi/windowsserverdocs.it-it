---
title: Gestire i criteri QoS
description: Questo argomento fornisce istruzioni su come creare e gestire i criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 94e5a1832a6c1e160b9cc338d50636026a5eb751
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851672"
---
# <a name="manage-qos-policy"></a>Gestire i criteri QoS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come usare la procedura guidata criteri QoS per creare, modificare o eliminare un criterio QoS.

>[!NOTE]
>  Oltre a questo argomento, la documentazione di gestione dei criteri di QoS seguente è disponibile.
> 
>  - [Errori ed eventi di criteri QoS](qos-policy-errors.md)

Nei sistemi operativi Windows, i criteri QoS combina la funzionalità di QoS basata su standard con la gestibilità di criteri di gruppo. Configurazione di questa combinazione rende facile applicazione dei criteri QoS per oggetti Criteri di gruppo. Windows include una procedura guidata criteri QoS che consentono di eseguire le attività seguenti.

-  [Creare un criterio QoS](#bkmk_createpolicy)

-  [Visualizzare, modificare o eliminare un criterio QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Creare un criterio QoS

Prima di creare un criterio QoS, è importante comprendere i due controlli QoS chiave che consentono di gestire il traffico di rete:

- Valore di DSCP

-   Limitazione della velocità

### <a name="prioritizing-traffic-with-dscp"></a>Definizione delle priorità del traffico con DSCP

Come indicato nell'esempio precedente dell'applicazione line-of-business, è possibile definire la priorità del traffico di rete in uscita usando **Specifica valore DSCP** per configurare un criterio QoS con uno specifico valore DSCP. 

Come descritto nella specifica RFC 2474, DSCP consente valori compresi tra 0 e 63 per essere specificata all'interno del campo TOS di un pacchetto IPv4 e all'interno del campo di classe di traffico IPv6. Router di rete usano il valore di DSCP per classificare i pacchetti di rete e accodare in modo appropriato.
  
> [!NOTE]
>  Per impostazione predefinita, il traffico di Windows ha un valore DSCP pari a 0.
  
Durante la definizione della strategia QoS dell'organizzazione è necessario impostare il numero di code e la relativa priorità. Ad esempio, l'organizzazione potrebbe scegliere di avere cinque code: sensibili alla latenza del traffico, controllare il traffico, il traffico di business-critical, sforzo il traffico e il traffico di trasferimento di dati bulk.  
  
### <a name="throttling-traffic"></a>Limitazione della velocità del traffico

Oltre a valori di DSCP, la limitazione delle richieste è un altro controllo chiave per la gestione della larghezza di banda di rete. Come accennato in precedenza, è possibile usare la **specifica velocità in uscita** impostazione per configurare un criterio QoS con una velocità specifica per il traffico in uscita. Tramite la limitazione delle richieste, un criterio QoS limita il traffico di rete in uscita per una velocità specificata. È possibile utilizzare contemporaneamente il contrassegno DSCP e la limitazione della velocità per un'efficiente gestione del traffico.

>[!NOTE]
>Per impostazione predefinita, la casella di controllo **Specifica velocità in uscita** non è selezionata.

Per creare un criterio QoS, modificare le impostazioni di un oggetto (criteri di gruppo) nello strumento di Console Gestione criteri di gruppo (GPMC). GPMC quindi apre Editor oggetti Criteri di gruppo.

I nomi di criterio QoS devono essere univoci. Come i criteri vengono applicati ai server e gli utenti finali dipende i criteri QoS di archiviazione in Editor oggetti Criteri di gruppo:

- Un criterio QoS archiviato in Configurazione computer\Impostazioni Settings\QoS criteri si applica ai computer, indipendentemente dall'utente attualmente connesso. I criteri QoS basati su computer vengono in genere utilizzati per computer server.

- Un criterio QoS archiviato in Configurazione computer\Impostazioni di utente Settings\QoS criteri si applica agli utenti dopo che hanno effettuato l'accesso, indipendentemente dal computer che hanno effettuato l'accesso a.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Per creare un nuovo criterio QoS con la procedura guidata criteri QoS

-   In Editor oggetti Criteri di gruppo, fare doppio clic su uno dei **i criteri QoS** nodi e quindi fare clic su **creare un nuovo criterio**.

### <a name="wizard-page-1---policy-profile"></a>Pagina della procedura guidata 1 - profilo criterio

Nella prima pagina della procedura guidata criteri QoS, è possibile specificare un nome di criterio e configurare la modalità QoS controlla il traffico di rete in uscita.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Per configurare la pagina Profilo criterio della procedura guidata QoS basata su criteri

1. In **Nome criterio** digitare il nome del criterio QoS. Il nome deve identificare in modo univoco il criterio.

2. Facoltativamente, usare **Specifica valore DSCP** per abilitare il contrassegno DSCP e quindi configurare un valore DSCP compreso tra 0 e 63.

3. È possibile utilizzare **Specifica velocità in uscita** per abilitare la limitazione della velocità del traffico e configurare la velocità. Il valore della velocità deve essere maggiore di 1 ed è possibile specificare le unità di kilobyte al secondo \(KBps\) o in megabyte al secondo \(MBps\).

4. Fare clic su **Avanti**.

### <a name="wizard-page-2---application-name"></a>Pagina della procedura guidata 2 - nome dell'applicazione

Nella seconda pagina della procedura guidata criteri QoS è possibile applicare i criteri a tutte le applicazioni, a una specifica applicazione come identificati dal relativo nome dell'eseguibile, in un percorso e nome dell'applicazione o per le applicazioni server HTTP che gestiscono le richieste per un URL specifico.

- **Tutte le applicazioni** specifica che le impostazioni di gestione traffico nella prima pagina della procedura guidata criteri QoS si applicano a tutte le applicazioni.

- **Solo le applicazioni con questo nome di eseguibile** specifica che le impostazioni di gestione traffico nella prima pagina della procedura guidata criteri QoS per un'applicazione specifica. Il nome del file eseguibile deve terminare con l'estensione exe.

- **Solo le applicazioni server HTTP risponde alle richieste per questo URL** specifica che le impostazioni di gestione traffico nella prima pagina della procedura guidata criteri QoS si applicano per solo alcune applicazioni server HTTP.

È inoltre possibile immettere il percorso dell'applicazione. A tal fine, aggiungere il percorso al nome dell'applicazione. Il percorso può includere variabili di ambiente. ad esempio %ProgramFiles%\Percorso applicazione\App.exe oppure c:\programmi\percorso applicazione\app.exe.

>[!NOTE]
>Il percorso dell'applicazione non può includere un percorso che si risolve in un collegamento simbolico.

L'URL deve rispettare [RFC 1738](https://tools.ietf.org/html/rfc1738), sotto forma di `http[s]://<hostname\>:<port\>/<url-path>`. È possibile usare un carattere jolly `‘*’`, per `<hostname>` e/o `<port>`, ad esempio `https://training.\*/, https://\*.\*`, ma il carattere jolly non può indicare una sottostringa del `<hostname>` o `<port>`.

In altre parole, nessuna delle due `https://my\*site/` né `https://\*training\*/` è valido. 

Facoltativamente, è possibile controllare **includere le sottodirectory e file** per eseguire la corrispondenza su tutte le sottodirectory e file dopo un URL. Ad esempio, se questa opzione è selezionata e l'URL è `https://training`, i criteri QoS prenderà in considerazione le richieste per` https://training/video` una corrispondenza soddisfacente.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Per configurare la pagina di nome dell'applicazione della procedura guidata criteri QoS

1. Nel **questo criterio QoS si applica a**, selezionare **tutte le applicazioni** oppure **solo le applicazioni con questo nome di eseguibile**.

2. Se si seleziona **Solo le applicazioni con questo nome di eseguibile**, specificare un nome di eseguibile che termina con l'estensione exe.

3. Fare clic su **Avanti**.

### <a name="wizard-page-3---ip-addresses"></a>Pagina della procedura guidata 3 - indirizzi IP

Nella terza pagina della procedura guidata criteri QoS è possibile specificare le condizioni di indirizzo IP per i criteri QoS, incluse le seguenti:

- Tutti gli indirizzi IPv4 o IPv6 di origine o indirizzi IPv4 o IPv6 di origine specifici

- Tutti gli indirizzi IPv4 o IPv6 di destinazione o indirizzi IPv4 o IPv6 di destinazione specifico

Se si seleziona **Solo per il prefisso o l'indirizzo IP di origine seguente** o **Solo per il prefisso o l'indirizzo IP di destinazione seguente**, è necessario digitare uno dei valori seguenti:

- Indirizzo IPv4, ad esempio `192.168.1.1`

- Prefisso di indirizzo IPv4 con notazione della lunghezza del prefisso rete, ad esempio `192.168.1.0/24`

- Indirizzo IPv6, ad esempio `3ffe:ffff::1`

- IPv6 indirizzo prefisso, ad esempio `3ffe:ffff::/48`

Se si selezionano entrambe **solo per il seguente indirizzo IP di origine** e **solo per il seguente indirizzo IP di destinazione**, entrambi gli indirizzi o prefissi di indirizzo devono essere entrambi IPv4 o IPv6 dal basato.

Nella pagina della procedura guidata precedente è stato specificato l'URL per le applicazioni server HTTP, si noterà che l'indirizzo IP di origine per i criteri di QoS in questa pagina della procedura guidata è inattiva. 

Questo vale in quanto l'indirizzo IP di origine è l'indirizzo del server HTTP e non può essere configurato qui. D'altra parte, è comunque possibile personalizzare i criteri, specificando l'indirizzo IP di destinazione. Questo rende possibile creare criteri diversi per diversi client usando le stesse applicazioni server HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Per configurare la pagina indirizzi IP della procedura guidata criteri QoS

1. In **si applica questo criterio QoS a** (origine), selezionare **qualsiasi indirizzo IP di origine** oppure **solo per l'indirizzo IP di origine indirizzo**.

2. Se è stato selezionato **solo l'indirizzo IP di origine**, specificare un indirizzo IPv4 o IPv6 o del prefisso.

3. Nelle **si applica questo criterio QoS a** (destinazione), selezionare **qualsiasi indirizzo di destinazione** o **solo per l'indirizzo IP di destinazione.**

4. Se è stato selezionato **solo per l'indirizzo IP di destinazione**, specificare un indirizzo IPv4 o IPv6 o un prefisso che corrisponde al tipo di indirizzo o prefisso specificato per l'indirizzo di origine.

5.  Fare clic su **Avanti**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Pagina della procedura guidata 4 - porte e protocolli

Nella quarta pagina della procedura guidata criteri QoS, è possibile specificare i tipi di traffico e le porte controllati dalle impostazioni nella prima pagina della procedura guidata. e nello specifico:  
-   Il traffico TCP, il traffico UDP o entrambi  

-   Tutte le porte di origine, un intervallo di porte di origine o una porta di origine specifica

-   Tutte le porte di destinazione, un intervallo di porte di destinazione o una porta di destinazione specifico  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Per configurare la pagina protocolli e porte della procedura guidata criteri QoS

1. In **Seleziona il protocollo a cui si applica questo criterio QoS** selezionare **TCP**, **UDP** o **TCP e UDP**.

2. In **Specifica il numero della porta di origine** selezionare **Da qualsiasi porta di origine** o **Da questo numero di porta o intervallo di porte di origine**.

3. Se è stato selezionato **da questo numero di porta di origine**, digitare un numero di porta compreso tra 1 e 65535.

     Facoltativamente, è possibile specificare un intervallo di porte, nel formato "*bassa*:*elevata*," in cui *bassa* e *elevata* rappresentano i limiti inferiori e limiti superiori della porta di intervallo (inclusi). *Low* e *elevata* ognuno deve essere un numero compreso tra 1 e 65535. Tra i due punti (:) e i numeri non deve essere presente alcuno spazio.

4. In **Specifica il numero della porta di destinazione** selezionare **Verso qualsiasi porta di destinazione** o **Verso questo numero di porta o intervallo di porte di destinazione**.

5. Se nel passaggio precedente si seleziona **Verso questo numero di porta o intervallo di porte di destinazione**, digitare un numero di porta compreso tra 1 e 65535.

Per completare la creazione del nuovo criterio QoS, fare clic su **Finish** nel **protocolli e porte** pagina della procedura guidata criteri QoS. Al termine, il nuovo criterio di QoS è elencato nel riquadro dei dettagli di Editor oggetti Criteri di gruppo.  
  
Per applicare le impostazioni dei criteri QoS a utenti o computer, collegare l'oggetto Criteri di gruppo in cui i criteri QoS si trovano in un contenitore di Active Directory Domain Services, ad esempio un dominio, un sito o un'unità organizzativa (OU).  
  
##  <a name="bkmk_editpolicy"></a>Visualizzare, modificare o eliminare un criterio QoS

Le pagine dei criteri di QoS guidata descritta in precedenza corrispondono alle pagine delle proprietà che vengono visualizzate quando si visualizza o modifica le proprietà di un criterio.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Per visualizzare le proprietà di un criterio QoS  
  
-   Fare clic sul nome del criterio nel riquadro dei dettagli di Editor oggetti Criteri di gruppo e quindi fare clic su **proprietà**.  
  
     L'Editor oggetti Criteri di gruppo consente di visualizzare la pagina delle proprietà con le schede seguenti:  
  
    -   Profilo criterio  
  
    -   Nome applicazione  
  
    -   Indirizzi IP  
  
    -   Protocolli e porte  
  
### <a name="to-edit-a-qos-policy"></a>Per modificare un criterio QoS  
  
-   Fare clic sul nome del criterio nel riquadro dei dettagli di Editor oggetti Criteri di gruppo e quindi fare clic su **Modifica criterio esistente relativo**.  
  
     L'Editor oggetti Criteri di gruppo consente di visualizzare il **modifica un criterio QoS esistente** nella finestra di dialogo.  
  
### <a name="to-delete-a-qos-policy"></a>Per eliminare un criterio QoS  
  
-   Fare clic sul nome del criterio nel riquadro dei dettagli di Editor oggetti Criteri di gruppo e quindi fare clic su **Elimina criterio**.  
  
### <a name="qos-policy-gpmc-reporting"></a>I criteri QoS Reporting di GPMC 

Dopo aver applicato un numero di criteri di QoS in tutta l'organizzazione, potrebbe essere utile o necessario rivedere periodicamente il modo in cui sono applicati i criteri. Un riepilogo dei criteri QoS per un utente specifico o un computer può essere visualizzato con reporting di GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Per eseguire la creazione guidata rapporti Criteri di gruppo per un report dei criteri QoS  
  
-   In Gestione criteri di gruppo, fare doppio clic il **rapporti Criteri di gruppo** nodo e quindi selezionare il menu di opzione per **Creazione guidata rapporti Criteri di gruppo.**  
  
Dopo avere generati i risultati dei criteri di gruppo, fare clic sui **impostazioni** scheda. Nel **impostazioni** scheda, i criteri QoS sono reperibile nei nodi "Configurazione computer\Impostazioni Settings\QoS criteri" e "Configurazione computer\Impostazioni Settings\QoS di criteri".  
  
Nel **impostazioni** scheda con i relativi nomi dei criteri di QoS con il valore di DSCP, limitazione della velocità, le condizioni dei criteri sono elencati i criteri QoS e vincere oggetto Criteri di gruppo elencate nella stessa riga...  
  
Visualizzazione dei risultati dei criteri di gruppo identifica in modo univoco i criteri di gruppo dominante. Quando più oggetti Criteri di gruppo hanno i criteri di QoS con lo stesso nome dei criteri QoS, viene applicato l'oggetto Criteri di gruppo con la precedenza più alta di oggetto Criteri di gruppo. Si tratta di criteri di gruppo dominante. Conflitto tra criteri QoS (identificati dal nome del criterio) collegati a una priorità inferiore non vengono applicati oggetti Criteri di gruppo. Tenere presente che le priorità di oggetto Criteri di gruppo definiscono quali criteri di QoS vengono distribuiti nel sito, dominio o unità Organizzativa, come appropriato. Dopo la distribuzione, a un livello di utente o computer, il [regole di precedenza dei criteri di QoS](#BKMK_precedencerules) determinare quale traffico è consentito e bloccato.  
  
Valore di DSCP, limitazione della velocità e le condizioni dei criteri il criterio di QoS sono inoltre visibili in Criteri di gruppo oggetto Editor (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Impostazioni avanzate per utenti mobili e remoti  
Con i criteri di QoS, l'obiettivo è gestire il traffico in una rete aziendale. Negli scenari per dispositivi mobili, gli utenti potrebbero essere l'invio di traffico o disattivare la rete aziendale. Poiché i criteri QoS non sono rilevanti mentre dalla rete aziendale, i criteri di QoS sono abilitati solo su interfacce di rete connesse alla sezione aziendale per Windows 8, Windows 7 o Windows Vista.  
  
Ad esempio, un utente potrebbe connettersi proprio computer portatile alla propria rete aziendale tramite la rete privata virtuale (VPN) da un bar. Per VPN, l'interfaccia di rete fisica (ad esempio wireless) non avrà i criteri QoS applicati. Tuttavia, l'interfaccia VPN avrà i criteri QoS applicati perché si connette all'azienda. Se l'utente immette in un secondo momento un'altra rete aziendale che non ha una relazione di trust di dominio Active Directory, non verranno abilitati i criteri QoS.  
  
Si noti che questi scenari per dispositivi mobili non si applicano ai carichi di lavoro server. Ad esempio, un server con più schede di rete può rimanere sul bordo di una rete aziendale. Il reparto IT può scegliere di disporre di QoS criteri limita il traffico che ha un'uscita dell'organizzazione; Tuttavia, questa scheda di rete che invia il traffico in uscita non necessariamente si connette alla rete aziendale. Per questo motivo, i criteri di QoS sono sempre abilitati su tutte le interfacce di rete di un computer che esegue Windows Server 2012.  
  
> [!NOTE]
>  Abilitazione selettiva si applica solo ai criteri di QoS e non per le impostazioni QoS avanzate più avanti in questo documento.  
  
### <a name="advanced-qos-settings"></a>Impostazioni QoS avanzate

Le impostazioni QoS avanzate forniscono controlli aggiuntivi per gli amministratori IT a gestire il consumo di rete di computer e i contrassegni DSCP. Le impostazioni QoS avanzate si applicano solo a livello di computer, mentre i criteri QoS possono essere applicati a livello di computer e utente.

#### <a name="to-configure-advanced-qos-settings"></a>Per configurare le impostazioni QoS avanzate

1.  Fare clic su **configurazione Computer**, quindi fare clic su **le impostazioni di Windows in Criteri di gruppo**.
  
2.  Fare doppio clic su **i criteri QoS**, quindi fare clic su **impostazioni QoS avanzate**.

     La figura seguente mostra che due avanzato schede le impostazioni QoS: **Il traffico TCP in ingresso** e **sostituzione contrassegno DSCP**.
  
> [!NOTE]
>  Le impostazioni QoS avanzate sono impostazioni di criteri di gruppo a livello di computer.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Le impostazioni QoS avanzate: il traffico TCP in ingresso

**Il traffico TCP in ingresso** consente di controllare il consumo di larghezza di banda TCP sul lato del ricevitore, mentre i criteri QoS interessano il traffico TCP e UDP in uscita. 

Impostando a livello in una velocità effettiva inferiore il **il traffico TCP in ingresso** scheda TCP limiterà la dimensione del relativo TCP annunciati finestra di ricezione. L'effetto di questa impostazione verrà la velocità effettiva maggiore e collegare l'utilizzo per le connessioni TCP con larghezze di banda superiori o le latenze (prodotto del ritardo di larghezza di banda). Per impostazione predefinita, i computer che eseguono Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista sono impostati a livello di velocità effettiva massima.
  
La ricezione TCP finestra è stato modificato in Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista da versioni precedenti di Windows. Le versioni precedenti di Windows limitato la finestra di ricezione TCP a un massimo di 64 kilobyte (KB), mentre Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista in modo dinamico ridimensionare la finestra di receive-side fino a 16 megabyte (MB ). Nel controllo del traffico TCP in ingresso, è possibile controllare il livello di velocità effettiva in ingresso impostando il valore massimo che può raggiungere la finestra di ricezione TCP. I livelli di corrispondono ai valori massimi seguenti. 
  
|Livello di velocità effettiva in ingresso|Massimo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

Le dimensioni della finestra effettivo possono essere un valore uguale o inferiore rispetto al valore massimo, in base alle condizioni di rete.

###### <a name="to-set-the-tcp-receive-side-window"></a>Per impostare la finestra di ricezione TCP

1. In Editor oggetti Criteri di gruppo, fare clic su **criteri del Computer locale**, fare clic su **le impostazioni di Windows**, fare clic destro **criteri QoS**e quindi fare clic su **QoS avanzate Impostazioni**.
  
2. Nelle **velocità effettiva di ricezione TCP**, selezionare **configurare la velocità effettiva ricezione TCP**e quindi selezionare il livello di velocità effettiva desiderati.

3.  Collegamento oggetto Criteri di gruppo all'unità Organizzativa.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Impostazioni QoS avanzate: Sostituzione di contrassegno DSCP

Override contrassegno DSCP limita la capacità delle applicazioni per specificare, o "mark", ovvero DSCP valori diversi da quelli specificati nei criteri di QoS. Specifica che le applicazioni possono impostare i valori DSCP, le applicazioni possono impostare i valori di DSCP diverso da zero. 

Specificando **Ignore**, le applicazioni che usano QoS APIs avranno i valori DSCP impostati su zero e solo i criteri QoS possono impostare i valori di DSCP. 

Per impostazione predefinita, i computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista consentono alle applicazioni di specificare valori DSCP. le applicazioni e i dispositivi che non usano le APIs QoS non vengono sottoposte a override.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valori DSCP e wireless Multimedia

Il [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) ha stabilito una certificazione per Wireless Multimedia \(WMM\) che definisce quattro categorie di accesso \(WMM_AC\) per definire la priorità del traffico di rete trasmessi su una Wi\-Fi rete wireless. Includono le categorie di accesso \(in ordine di priorità più alta alla minima\): voce, video, migliore sforzo e background; abbreviati rispettivamente come VO, VI, BE e BK. La specifica WMM definisce quale DSCP valori corrispondono con ognuna delle categorie di quattro accesso:
  
|Valore di DSCP|Categoria di accesso WMM|
|----------|-------------------|
|48-63|Voce (VO)|
|32-47|Video (VI)|
|24-31, 0-7|Senza alcuna garanzia (BE)|
|8-23|Background (BK)|

È possibile creare criteri di QoS che usano questi valori DSCP per assicurare che formato portabile computer con Wi\-Fi Certified™, per la ricezione delle schede di rete wireless WMM assegnata la priorità di gestione quando associato Wi\-Fi certificate per WMM punti di accesso.
  
### <a name="BKMK_precedencerules"></a>Regole di precedenza di criteri QoS

Analogamente alle priorità dell'oggetto Criteri di gruppo, i criteri QoS di avere le regole di precedenza per risolvere i conflitti quando più criteri di QoS vengono applicati a un set specifico di traffico. Per il traffico TCP o UDP in uscita, un solo criterio di QoS può essere applicato alla volta, il che significa che i criteri QoS non è un effetto cumulativo, ad esempio in cui potrebbero essere sommati tassi di limitazione.

In generale, i criteri di QoS con più condizioni di corrispondenza wins. Quando vengono applicati più criteri di QoS, le regole possono essere suddivise in tre categorie: a livello di utente e a livello di computer; applicazione rispetto a quintupla la rete; e tra la rete quintuple.

Dal *rete quintupla*, si intende che l'indirizzo IP di origine, indirizzo IP di destinazione, porta di origine, porta di destinazione e protocollo \(TCP/UDP\).  

 **Criteri QoS a livello di utente ha la precedenza sui criteri QoS a livello di computer**

Questa regola semplifica notevolmente la gestione degli amministratori di rete di QoS di oggetti Criteri di gruppo, in particolare per i criteri di gruppo basato su utente. Ad esempio definire un criterio di QoS per un gruppo di utenti, l'amministratore di rete deve sono può solo creare e distribuire un GPO a tale gruppo. Non devono preoccuparsi sui computer in cui gli utenti sono connessi e se tali computer avrà criteri di QoS in conflitto è definiti, perché, se si verifica un conflitto, i criteri a livello di utente ha sempre la precedenza.

> [!NOTE]
>  Un criterio di QoS a livello di utente riguarda solo il traffico generato da tale utente. Altri utenti di un computer specifico e il computer stesso, non sarà soggetto a tutti i criteri QoS che vengono definiti per tale utente.

 **Specificità di applicazione e che hanno la precedenza su quintupla di rete**

Quando più criteri di QoS corrispondono al traffico specifico, viene applicato il criterio più specifico. Tra i criteri che identificano le applicazioni, un criterio che include il percorso file dell'applicazione di provenienza è considerato più specifico rispetto a un altro criterio che identifica solo il nome dell'applicazione (Nessun percorso). Se più criteri con le applicazioni vengono mantenuti, le regole sulla precedenza usano la rete quintupla per trovare la migliore corrispondenza.

In alternativa, più criteri di QoS potrebbero riguardare lo stesso traffico specificando delle condizioni non sovrapposti. Tra le condizioni di applicazioni e quintupla la rete, il criterio che specifica l'applicazione è considerato più specifico e viene applicato. 

Ad esempio, policy_A specifica solo il nome di un'applicazione (app.exe) e policy_B specifica di 192.168.1.0/24 indirizzo IP destinazione. Quando questi criteri di QoS sono in conflitto \(app.exe invia il traffico a un indirizzo IP compreso nell'intervallo di 192.168.4.0/24\), policy_A viene applicato.

 **Conferire maggiore specificità ha la precedenza all'interno della rete quintupla**

Per i conflitti di criteri all'interno di quintupla di rete, i criteri con le condizioni più corrispondente ha la precedenza. Si supponga ad esempio policy_C specifica di indirizzi IP di origine "any", indirizzo IP di destinazione 10.0.0.1, la porta "qualsiasi" origine, destinazione della porta "any" e "TCP" del protocollo. 

Successivamente, si supponga policy_D specifica "qualsiasi" indirizzo IP di origine, IP di destinazione indirizzo 10.0.0.1, porta di origine "qualsiasi", porta di destinazione 80 e protocolli "TCP". Quindi policy_C e policy_D far corrispondere le connessioni a destinazione 10.0.0.1: 80. Poiché criterio QoS viene applicato il criterio con le condizioni di corrispondenza più specifiche, policy_D ha la precedenza in questo esempio.  
  
Tuttavia, i criteri QoS potrebbero avere un numero uguale di condizioni. Ad esempio, diversi criteri potrebbero ognuno dei quali specifica solo uno (ma non uguale) informazione quintupla la rete. Tra quintupla la rete, l'ordine seguente è più alto per ridurre la precedenza:

- Indirizzo IP di origine

- Indirizzo IP di destinazione

- Porta di origine

- Porta di destinazione

- Protocollo (TCP o UDP)

All'interno di una determinata condizione, ad esempio l'indirizzo IP, viene considerato un indirizzo IP più specifico con priorità più alta; ad esempio, un indirizzo IP 192.168.4.1 è più specifico 192.168.4.0/24.

Progettare i criteri di QoS in modo specifico semplificare la capacità dell'organizzazione per capire quali criteri sono in vigore.

Per l'argomento successivo in questa Guida, vedere [errori ed eventi di criteri di QoS](qos-policy-errors.md).

Per il primo argomento in questa Guida, vedere [dei criteri di qualità del servizio (QoS)](qos-policy-top.md).
