---
title: Gestisci criteri QoS
description: Questo argomento fornisce istruzioni su come creare e gestire i criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3cff51b3cf76d3224832bf99ff966bf473d6ff6c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871854"
---
# <a name="manage-qos-policy"></a>Gestisci criteri QoS

>Si applica a Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sull'utilizzo della creazione guidata criteri QoS per creare, modificare o eliminare un criterio QoS.

>[!NOTE]
>  Oltre a questo argomento, è disponibile la documentazione seguente relativa alla gestione dei criteri QoS.
> 
>  - [Errori ed eventi dei criteri QoS](qos-policy-errors.md)

Nei sistemi operativi Windows, i criteri QoS combinano la funzionalità di QoS basata su standard con la gestibilità dei Criteri di gruppo. La configurazione di questa combinazione rende più semplice l'applicazione dei criteri QoS a Criteri di gruppo oggetti. Windows include una procedura guidata per i criteri QoS che consente di eseguire le operazioni seguenti.

-  [Creare un criterio QoS](#bkmk_createpolicy)

-  [Visualizzare, modificare o eliminare un criterio QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Creare un criterio QoS

Prima di creare un criterio QoS, è importante comprendere i due controlli QoS principali usati per gestire il traffico di rete:

- Valore DSCP

-   Velocità di limitazione

### <a name="prioritizing-traffic-with-dscp"></a>Assegnazione delle priorità al traffico con DSCP

Come indicato nell'esempio di applicazione line-of-business precedente, è possibile definire la priorità del traffico di rete in uscita usando **specifica il valore DSCP** per configurare un criterio QoS con un valore DSCP specifico. 

Come descritto nella RFC 2474, DSCP consente di specificare i valori da 0 a 63 nel campo TOS di un pacchetto IPv4 e nel campo della classe di traffico in IPv6. I router di rete usano il valore DSCP per classificare i pacchetti di rete e accodarli in modo appropriato.
  
> [!NOTE]
>  Per impostazione predefinita, il traffico di Windows ha un valore DSCP pari a 0.
  
Durante la definizione della strategia QoS dell'organizzazione è necessario impostare il numero di code e la relativa priorità. Ad esempio, l'organizzazione può scegliere di avere cinque code: traffico sensibile alla latenza, traffico di controllo, traffico critico per l'azienda, traffico con il massimo sforzo e traffico per il trasferimento di dati in blocco.  
  
### <a name="throttling-traffic"></a>Limitazione della velocità del traffico

Insieme ai valori DSCP, la limitazione è un altro controllo chiave per la gestione della larghezza di banda di rete. Come indicato in precedenza, è possibile usare l'impostazione **specificare la velocità di limitazione** per configurare un criterio QoS con una velocità specifica di limitazione per il traffico in uscita. Utilizzando la limitazione, un criterio QoS limita il traffico di rete in uscita a una velocità specificata. È possibile utilizzare contemporaneamente il contrassegno DSCP e la limitazione della velocità per un'efficiente gestione del traffico.

>[!NOTE]
>Per impostazione predefinita, la casella di controllo **Specifica velocità in uscita** non è selezionata.

Per creare un criterio QoS, modificare le impostazioni di un oggetto Criteri di gruppo (GPO) dallo strumento Console Gestione Criteri di gruppo (GPMC). GPMC apre quindi il Editor oggetti Criteri di gruppo.

I nomi di criterio QoS devono essere univoci. Il modo in cui i criteri vengono applicati ai server e gli utenti finali dipendono dalla posizione in cui vengono archiviati i criteri QoS nel Editor oggetti Criteri di gruppo:

- Un criterio QoS nel computer Configurazione computer\Impostazioni Settings\QoS criterio si applica ai computer, indipendentemente dall'utente attualmente connesso. I criteri QoS basati su computer vengono in genere utilizzati per computer server.

- Un criterio QoS nei criteri Settings\QoS di configurazione utente viene applicato agli utenti dopo aver eseguito l'accesso, indipendentemente dal computer a cui hanno effettuato l'accesso.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Per creare un nuovo criterio QoS con la procedura guidata criteri QoS

-   In Editor oggetti Criteri di gruppo fare clic con il pulsante destro del mouse su uno dei nodi **criteri QoS** , quindi scegliere **Crea un nuovo criterio**.

### <a name="wizard-page-1---policy-profile"></a>Pagina 1 della procedura guidata-profilo criteri

Nella prima pagina della creazione guidata criteri QoS è possibile specificare un nome di criterio e configurare il modo in cui QoS controlla il traffico di rete in uscita.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Per configurare la pagina Profilo criterio della procedura guidata QoS basata su criteri

1. In **Nome criterio** digitare il nome del criterio QoS. Il nome deve identificare in modo univoco i criteri.

2. Facoltativamente, usare **specifica il valore DSCP** per abilitare il contrassegno DSCP, quindi configurare un valore DSCP compreso tra 0 e 63.

3. È possibile utilizzare **Specifica velocità in uscita** per abilitare la limitazione della velocità del traffico e configurare la velocità. Il valore della velocità di limitazione deve essere maggiore di 1 ed è possibile specificare unità di kilobyte al secondo \(Kbps\) o megabyte al secondo \(Mbps\).

4. Fare clic su **Avanti**.

### <a name="wizard-page-2---application-name"></a>Pagina 2 della procedura guidata-nome applicazione

Nella seconda pagina della procedura guidata per i criteri QoS è possibile applicare il criterio a tutte le applicazioni, a un'applicazione specifica come identificato dal nome dell'eseguibile, a un percorso e a un nome di applicazione o alle applicazioni server HTTP che gestiscono le richieste per un URL specifico.

- **Tutte le applicazioni** specificano che le impostazioni di gestione del traffico nella prima pagina della procedura guidata dei criteri QoS sono valide per tutte le applicazioni.

- **Solo le applicazioni con questo nome eseguibile** specificano che le impostazioni di gestione del traffico nella prima pagina della procedura guidata dei criteri QoS sono destinate a un'applicazione specifica. Il nome del file eseguibile deve terminare con l'estensione exe.

- **Solo le applicazioni server http che rispondono alle richieste per questo URL** specificano che le impostazioni di gestione del traffico nella prima pagina della procedura guidata criteri QoS sono valide solo per alcune applicazioni server http.

È inoltre possibile immettere il percorso dell'applicazione. A tal fine, aggiungere il percorso al nome dell'applicazione. Il percorso può includere variabili di ambiente. ad esempio %ProgramFiles%\Percorso applicazione\App.exe oppure c:\programmi\percorso applicazione\app.exe.

>[!NOTE]
>Il percorso dell'applicazione non può includere un percorso che viene risolto in un collegamento simbolico.

L'URL deve essere conforme allo [standard RFC 1738](https://tools.ietf.org/html/rfc1738), nel `http[s]://<hostname\>:<port\>/<url-path>`formato. È possibile usare un carattere jolly `‘*'`,, `<hostname>` per `https://training.\*/, https://\*.\*`e/ `<port>`o, ad esempio, ma il carattere jolly non può indicare una sottostringa di `<hostname>` o `<port>`.

In altre parole, `https://my\*site/` né né `https://\*training\*/` sono validi. 

Facoltativamente, è possibile selezionare **Includi sottodirectory e file** per eseguire la corrispondenza in tutte le sottodirectory e i file dopo un URL. Se, ad esempio, questa opzione è selezionata e l'URL `https://training`è, i criteri QoS prenderanno` https://training/video` in considerazione le richieste per una corrispondenza corretta.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Per configurare la pagina Nome applicazione della creazione guidata criterio QoS

1. In **questo criterio QoS si applica a**, selezionare **tutte le applicazioni** o **solo le applicazioni con questo nome eseguibile**.

2. Se si seleziona **Solo le applicazioni con questo nome di eseguibile**, specificare un nome di eseguibile che termina con l'estensione exe.

3. Fare clic su **Avanti**.

### <a name="wizard-page-3---ip-addresses"></a>Pagina 3 della procedura guidata-indirizzi IP

Nella terza pagina della creazione guidata criteri QoS è possibile specificare le condizioni di indirizzi IP per i criteri QoS, inclusi i seguenti:

- Tutti gli indirizzi IPv4 o IPv6 di origine o indirizzi IPv4 o IPv6 di origine specifici

- Tutti gli indirizzi IPv4 o IPv6 di destinazione o indirizzi IPv4 o IPv6 di destinazione specifici

Se si seleziona **Solo per il prefisso o l'indirizzo IP di origine seguente** o **Solo per il prefisso o l'indirizzo IP di destinazione seguente**, è necessario digitare uno dei valori seguenti:

- Un indirizzo IPv4, ad esempio`192.168.1.1`

- Prefisso di indirizzo IPv4 che utilizza la notazione della lunghezza del prefisso di rete, ad esempio`192.168.1.0/24`

- Un indirizzo IPv6, ad esempio`3ffe:ffff::1`

- Un prefisso di indirizzo IPv6, ad esempio`3ffe:ffff::/48`

Se si seleziona **solo per l'indirizzo IP di origine seguente** e **solo per l'indirizzo IP di destinazione seguente**, entrambi gli indirizzi o i prefissi di indirizzo devono essere basati su IPv4 o IPv6.

Se è stato specificato l'URL per le applicazioni server HTTP nella pagina precedente della procedura guidata, si noterà che l'indirizzo IP di origine per i criteri QoS in questa pagina della procedura guidata è disattivato. 

Questo è vero perché l'indirizzo IP di origine è l'indirizzo del server HTTP e non è configurabile qui. D'altra parte, è comunque possibile personalizzare il criterio specificando l'indirizzo IP di destinazione. In questo modo è possibile creare criteri diversi per client diversi usando le stesse applicazioni server HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Per configurare la pagina indirizzi IP della creazione guidata criterio QoS

1. In **questo criterio QoS si applica a** (origine) selezionare **qualsiasi indirizzo IP di origine** o **solo per l'indirizzo IP di origine seguente**.

2. Se è stato selezionato **solo il seguente indirizzo di origine IP**, specificare un indirizzo o un prefisso IPv4 o IPv6.

3. In **questo criterio QoS si applica a** (destinazione) selezionare **qualsiasi indirizzo di destinazione** o **solo per l'indirizzo IP di destinazione seguente.**

4. Se è stata selezionata **l'opzione solo per il seguente indirizzo IP di destinazione**, specificare un indirizzo o un prefisso IPv4 o IPv6 corrispondente al tipo di indirizzo o prefisso specificato per l'indirizzo di origine.

5.  Fare clic su **Avanti**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Pagina 4 della procedura guidata-protocolli e porte

Nella quarta pagina della creazione guidata criteri QoS è possibile specificare i tipi di traffico e le porte controllate dalle impostazioni nella prima pagina della procedura guidata. e nello specifico:  
-   Il traffico TCP, il traffico UDP o entrambi  

-   Tutte le porte di origine, un intervallo di porte di origine o una porta di origine specifica

-   Tutte le porte di destinazione, un intervallo di porte di destinazione o una porta di destinazione specifica  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Per configurare la pagina protocolli e porte della procedura guidata per i criteri QoS

1. In **Seleziona il protocollo a cui si applica questo criterio QoS** selezionare **TCP**, **UDP** o **TCP e UDP**.

2. In **Specifica il numero della porta di origine** selezionare **Da qualsiasi porta di origine** o **Da questo numero di porta o intervallo di porte di origine**.

3. Se è stato selezionato **da questo numero di porta di origine**, digitare un numero di porta compreso tra 1 e 65535.

     Facoltativamente, è possibile specificare un intervallo di porte, nel formato "*low*:*High*", dove *low* e *High* rappresentano i limiti inferiori e superiori dell'intervallo di porte, inclusi. Il valore *minimo* e *massimo* devono essere un numero compreso tra 1 e 65535. Tra i due punti (:) e i numeri non deve essere presente alcuno spazio.

4. In **Specifica il numero della porta di destinazione** selezionare **Verso qualsiasi porta di destinazione** o **Verso questo numero di porta o intervallo di porte di destinazione**.

5. Se nel passaggio precedente si seleziona **Verso questo numero di porta o intervallo di porte di destinazione**, digitare un numero di porta compreso tra 1 e 65535.

Per completare la creazione dei nuovi criteri QoS, fare clic su **fine** nella pagina **protocolli e porte** della procedura guidata criteri QoS. Al termine, il nuovo criterio QoS è elencato nel riquadro dei dettagli della Editor oggetti Criteri di gruppo.  
  
Per applicare le impostazioni dei criteri QoS a utenti o computer, collegare l'oggetto Criteri di gruppo in cui si trovano i criteri QoS in un contenitore Active Directory Domain Services, ad esempio un dominio, un sito o un'unità organizzativa (OU).  
  
##  <a name="bkmk_editpolicy"></a>Visualizzare, modificare o eliminare un criterio QoS

Le pagine della procedura guidata criteri QoS descritte in precedenza corrispondono alle pagine delle proprietà visualizzate quando si visualizzano o si modificano le proprietà di un criterio.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Per visualizzare le proprietà di un criterio QoS  
  
-   Fare clic con il pulsante destro del mouse sul nome del criterio nel riquadro dei dettagli della Editor oggetti Criteri di gruppo, quindi scegliere **Proprietà**.  
  
     Il Editor oggetti Criteri di gruppo Visualizza la pagina delle proprietà con le schede seguenti:  
  
    -   Profilo criterio  
  
    -   Nome applicazione  
  
    -   Indirizzi IP  
  
    -   Protocolli e porte  
  
### <a name="to-edit-a-qos-policy"></a>Per modificare un criterio QoS  
  
-   Fare clic con il pulsante destro del mouse sul nome del criterio nel riquadro dei dettagli della Editor oggetti Criteri di gruppo, quindi scegliere **modifica criteri esistenti**.  
  
     Nel Editor oggetti Criteri di gruppo viene visualizzata la finestra di dialogo **modifica criterio QoS esistente** .  
  
### <a name="to-delete-a-qos-policy"></a>Per eliminare un criterio QoS  
  
-   Fare clic con il pulsante destro del mouse sul nome del criterio nel riquadro dei dettagli della Editor oggetti Criteri di gruppo, quindi scegliere **Elimina criterio**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Reporting criteri QoS 

Dopo aver applicato una serie di criteri QoS nell'intera organizzazione, può essere utile o necessario esaminare periodicamente il modo in cui vengono applicati i criteri. Per visualizzare un riepilogo dei criteri QoS per un utente o un computer specifico, è possibile utilizzare la creazione di report di GPMC.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Per eseguire la creazione guidata risultati di Criteri di gruppo per un report dei criteri QoS  
  
-   Nella console Gestione criteri di ricerca fare clic con il pulsante destro del mouse sul nodo **risultati criteri di gruppo** , quindi selezionare l'opzione di menu per **criteri di gruppo creazione guidata risultati.**  
  
Una volta generati Criteri di gruppo risultati, fare clic sulla scheda **Impostazioni** . Nella scheda **Settings (impostazioni** ) i criteri QoS sono disponibili nei nodi "computer computer\Impostazioni Settings\QoS Policy" e "User computer\Impostazioni Settings\QoS Policy".  
  
Nella scheda **Impostazioni** i criteri QoS sono elencati in base ai relativi nomi di criteri QoS con il valore DSCP, la velocità di limitazione, le condizioni dei criteri e gli oggetti Criteri di gruppo vincenti elencati nella stessa riga.  
  
Nella visualizzazione dei risultati Criteri di gruppo viene identificato in modo univoco l'oggetto Criteri di gruppo vincente. Quando più oggetti Criteri di gruppo dispongono di criteri QoS con lo stesso nome di criteri QoS, viene applicato l'oggetto Criteri di gruppo con la precedenza più alta. Questo è l'oggetto Criteri di gruppo vincente. I criteri QoS in conflitto (identificati dal nome dei criteri) associati a un oggetto Criteri di gruppo con priorità inferiore non vengono applicati. Nota le priorità degli oggetti Criteri di gruppo definiscono quali criteri QoS vengono distribuiti nel sito, dominio o unità organizzativa, a seconda dei casi. Dopo la distribuzione, a livello di utente o computer, le [regole di precedenza dei criteri QoS](#BKMK_precedencerules) determinano il traffico consentito e bloccato.  
  
Il valore DSCP del criterio QoS, la velocità di limitazione e le condizioni dei criteri sono visibili anche in Editor oggetti Criteri di gruppo (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Impostazioni avanzate per roaming e utenti remoti  
Con i criteri QoS, l'obiettivo è quello di gestire il traffico sulla rete aziendale. Negli scenari per dispositivi mobili, gli utenti potrebbero inviare il traffico sulla rete aziendale. Poiché i criteri QoS non sono rilevanti quando si è lontani dalla rete aziendale, i criteri QoS sono abilitati solo nelle interfacce di rete connesse all'azienda per Windows 8, Windows 7 o Windows Vista.  
  
Ad esempio, un utente potrebbe connettere il computer portatile alla rete aziendale tramite la rete privata virtuale (VPN) da un bar. Per la VPN, l'interfaccia di rete fisica, ad esempio wireless, non avrà criteri QoS applicati. Tuttavia, l'interfaccia VPN avrà criteri QoS applicati perché si connette all'azienda. Se l'utente immette successivamente un'altra rete aziendale che non dispone di una relazione di trust di servizi di dominio Active Directory, i criteri QoS non verranno abilitati.  
  
Si noti che questi scenari per dispositivi mobili non si applicano ai carichi di lavoro del server. Ad esempio, un server con più schede di rete potrebbe trovarsi sul perimetro della rete aziendale. Il reparto IT può scegliere di limitare il traffico che egresses l'azienda. Tuttavia, la scheda di rete che invia il traffico in uscita non si connette necessariamente alla rete aziendale. Per questo motivo, i criteri QoS sono sempre abilitati in tutte le interfacce di rete di un computer che esegue Windows Server 2012.  
  
> [!NOTE]
>  L'abilitazione selettiva si applica solo ai criteri QoS e non alle impostazioni avanzate di QoS descritte più avanti in questo documento.  
  
### <a name="advanced-qos-settings"></a>Impostazioni QoS avanzate

Le impostazioni avanzate di QoS forniscono controlli aggiuntivi che consentono agli amministratori IT di gestire il consumo di rete del computer e i contrassegni DSCP. Le impostazioni QoS avanzate si applicano solo a livello di computer, mentre i criteri QoS possono essere applicati a livello di computer e utente.

#### <a name="to-configure-advanced-qos-settings"></a>Per configurare le impostazioni QoS avanzate

1.  Fare clic su **Configurazione computer**, quindi fare clic su **impostazioni di Windows in criteri di gruppo**.
  
2.  Fare clic con il pulsante destro del mouse su **criteri QoS**, quindi scegliere **impostazioni QoS avanzate**.

     Nella figura seguente sono illustrate le due schede Advanced QoS Settings: **Traffico TCP in ingresso** e **override del contrassegno DSCP**.
  
> [!NOTE]
>  Le impostazioni QoS avanzate sono impostazioni Criteri di gruppo a livello di computer.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Impostazioni QoS avanzate: traffico TCP in ingresso

Il **traffico TCP in ingresso** controlla l'utilizzo della larghezza di banda TCP sul lato del ricevitore, mentre i criteri QoS influiscono sul traffico TCP e UDP in uscita. 

Impostando un livello di velocità effettiva inferiore sulla scheda **traffico TCP in ingresso** , TCP limiterà le dimensioni della finestra di ricezione TCP annunciata. L'effetto di questa impostazione aumenterà le velocità di velocità effettiva e l'utilizzo dei collegamenti per le connessioni TCP con larghezze di banda o latenze più elevate (prodotto ritardo della larghezza di banda). Per impostazione predefinita, i computer che eseguono Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista sono impostati sul livello di velocità effettiva massima.
  
La finestra di ricezione TCP è cambiata in Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista da versioni precedenti di Windows. Le versioni precedenti di Windows limitavano la finestra del lato di ricezione TCP a un massimo di 64 kilobyte (KB), mentre Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista ridimensionavano dinamicamente la finestra sul lato di ricezione fino a 16 megabyte (MB ). Nel controllo del traffico TCP in ingresso è possibile controllare il livello di velocità effettiva in ingresso impostando il valore massimo che la finestra di ricezione TCP può aumentare. I livelli corrispondono ai valori massimi seguenti. 
  
|Livello di velocità effettiva in ingresso|Massima|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

La dimensione effettiva della finestra può essere un valore uguale o minore del valore massimo, a seconda delle condizioni della rete.

###### <a name="to-set-the-tcp-receive-side-window"></a>Per impostare la finestra del lato di ricezione TCP

1. In Editor oggetti Criteri di gruppo fare clic su **criteri del computer locale**, fare clic su **impostazioni di Windows**, fare clic con il pulsante destro del mouse su **criteri QoS**, quindi scegliere **impostazioni QoS avanzate**.
  
2. In **TCP ricezione velocità effettiva**selezionare **Configura velocità effettiva ricezione TCP**, quindi selezionare il livello di velocità effettiva desiderato.

3.  Collegare l'oggetto Criteri di gruppo all'unità organizzativa.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Impostazioni QoS avanzate: Override contrassegno DSCP

L'override del contrassegno DSCP limita la capacità delle applicazioni di specificare, o "Mark", valori DSCP diversi da quelli specificati nei criteri QoS. Specificando che le applicazioni sono autorizzate a impostare i valori DSCP, le applicazioni possono impostare valori DSCP diversi da zero. 

Se si specifica **Ignore**, le applicazioni che utilizzano le API QoS avranno valori DSCP impostati su zero e solo i criteri QoS possono impostare valori DSCP. 

Per impostazione predefinita, i computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista consentono alle applicazioni di specificare i valori DSCP; le applicazioni e i dispositivi che non usano le API QoS non vengono sottoposti a override.

##### <a name="wireless-multimedia-and-dscp-values"></a>Multimedia wireless e valori DSCP

Il [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) ha stabilito una certificazione per la tecnologia \(wireless\) WMM che definisce quattro categorie \(di\) accesso WMM_AC per la priorità del traffico di rete trasmesso su un Wi\-Rete wireless Fi. Le categorie di accesso \(includono l'ordine di priorità\)più alta a quella più bassa: voce, video, sforzo ottimale e sfondo, rispettivamente abbreviato come vo, vi, be e BK. La specifica WMM definisce quali valori DSCP corrispondono a ognuna delle quattro categorie di accesso:
  
|Valore DSCP|Categoria di accesso WMM|
|----------|-------------------|
|48-63|Voce (VO)|
|32-47|Video (VI)|
|24-31, 0-7|Massimo sforzo (BE)|
|8-23|Sfondo (BK)|

È possibile creare criteri QoS che usano questi valori DSCP per garantire che i computer portatili con\-™ Wi-Fi Certified per le schede wireless WMM ricevano la gestione\-con priorità quando sono associati ai punti di accesso con certificazione Wi-Fi per WMM.
  
### <a name="BKMK_precedencerules"></a>Regole di precedenza dei criteri QoS

Analogamente alle priorità degli oggetti Criteri di gruppo, i criteri QoS hanno regole di precedenza per la risoluzione dei conflitti quando più criteri QoS si applicano a un set specifico di traffico. Per il traffico TCP o UDP in uscita, è possibile applicare un solo criterio QoS alla volta, il che significa che i criteri QoS non hanno un effetto cumulativo, ad esempio dove verranno sommate le frequenze di limitazione.

In generale, prevale il criterio QoS con le condizioni più corrispondenti. Quando si applicano più criteri QoS, le regole rientrano in tre categorie: a livello di utente e a livello di computer; applicazione rispetto alla rete quintupla; e tra i quintupla di rete.

Per *quintupla di rete*, si intende l'indirizzo IP di origine, l'indirizzo IP di destinazione, la porta di origine \(, la porta\)di destinazione e il protocollo TCP/UDP.  

 **I criteri QoS a livello di utente hanno la precedenza sui criteri QoS a livello di computer**

Questa regola facilita enormemente la gestione degli oggetti Criteri di gruppo QoS degli amministratori di rete, in particolare per i criteri basati sui gruppi di utenti. Se, ad esempio, l'amministratore di rete desidera definire un criterio QoS per un gruppo di utenti, può solo creare e distribuire un oggetto Criteri di gruppo a tale gruppo. Non devono preoccuparsi dei computer a cui gli utenti accedono e se tali computer avranno criteri QoS in conflitto definiti, perché, se esiste un conflitto, i criteri a livello di utente hanno sempre la precedenza.

> [!NOTE]
>  Un criterio QoS a livello di utente è applicabile solo al traffico generato da tale utente. Gli altri utenti di un computer specifico e il computer stesso non saranno soggetti a criteri QoS definiti per tale utente.

 **Specificità delle applicazioni e priorità rispetto alla rete quintupla**

Quando più criteri QoS corrispondono al traffico specifico, vengono applicati i criteri più specifici. Tra i criteri che identificano le applicazioni, i criteri che includono il percorso del file dell'applicazione mittente sono considerati più specifici di un altro criterio che identifica solo il nome dell'applicazione (nessun percorso). Se si applicano più criteri con le applicazioni, le regole di precedenza usano la rete quintupla per trovare la migliore corrispondenza.

In alternativa, è possibile che più criteri QoS si applichino allo stesso traffico specificando condizioni non sovrapposte. Tra le condizioni delle applicazioni e la rete quintupla, i criteri che specificano l'applicazione sono considerati più specifici e vengono applicati. 

Ad esempio, policy_A specifica solo un nome di applicazione (app. exe) e policy_B specifica l'indirizzo IP di destinazione 192.168.1.0/24. Quando questi criteri QoS sono \(in conflitto, app. exe invia il traffico a un indirizzo IP compreso nell'intervallo\)di 192.168.4.0/24. viene applicato policy_A.

 **Una maggiore specificità ha la precedenza all'interno della rete quintupla**

Per i conflitti dei criteri all'interno della rete quintupla, i criteri con le condizioni più corrispondenti hanno la precedenza. Si supponga, ad esempio, che policy_C specifichi l'indirizzo IP di origine "any", l'indirizzo IP di destinazione 10.0.0.1, la porta di origine "any", la porta di destinazione "any" e il protocollo "TCP". 

Si supponga quindi che policy_D specifichi l'indirizzo IP di origine "any", l'indirizzo IP di destinazione 10.0.0.1, la porta di origine "any", la porta di destinazione 80 e il protocollo "TCP". Quindi policy_C e policy_D corrispondono entrambe alle connessioni alla destinazione 10.0.0.1:80. Poiché i criteri QoS applicano i criteri con le condizioni di corrispondenza più specifiche, in questo esempio policy_D ha la precedenza.  
  
Tuttavia, i criteri QoS potrebbero avere un numero di condizioni uguale. Ad esempio, diversi criteri possono specificare una sola parte (ma non la stessa) del quintupla di rete. Tra la rete quintupla, l'ordine seguente è dalla precedenza superiore a quella più bassa:

- Indirizzo IP di origine

- Indirizzo IP di destinazione

- Porta di origine

- Porta di destinazione

- Protocollo (TCP o UDP)

In una condizione specifica, ad esempio l'indirizzo IP, un indirizzo IP più specifico viene considerato con precedenza più elevata; un indirizzo IP 192.168.4.1, ad esempio, è più specifico di 192.168.4.0/24.

Progettare i criteri QoS nel modo più specifico possibile per semplificare la capacità dell'organizzazione di comprendere quali criteri sono attivi.

Per l'argomento successivo di questa guida, vedere [gli eventi e gli errori dei criteri QoS](qos-policy-errors.md).

Per il primo argomento di questa guida, vedere [criteri di qualità del servizio (QoS)](qos-policy-top.md).
