---
title: Gestire i criteri QoS
description: In questo argomento fornisce istruzioni su come creare e gestire i criteri di qualità del servizio (QoS) in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 04fdfa54-6600-43d4-8945-35f75e15275a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5baa83e41c79a558f5bd32f77a234367726592c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-qos-policy"></a>Gestire i criteri QoS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Per informazioni sull'utilizzo della creazione guidata criteri QoS per creare, modificare o eliminare un criterio QoS, è possibile utilizzare questo argomento.

>[!NOTE]
>  Oltre a questo argomento, è disponibile la documentazione seguente relativa gestione dei criteri QoS.
> 
>  - [Errori ed eventi criteri QoS](qos-policy-errors.md)

Nei sistemi operativi Windows, criteri di QoS combina la funzionalità di QoS basata su standard con la gestibilità di criteri di gruppo. Configurazione di questa combinazione rende per una facile applicazione dei criteri QoS a oggetti Criteri di gruppo. Windows include una procedura guidata criteri di QoS che consentono di eseguire le attività seguenti.

-  [Creare un criterio QoS](#bkmk_createpolicy)

-  [Visualizzare, modificare o eliminare un criterio QoS](#bkmk_editpolicy)

##  <a name="bkmk_createpolicy"></a>Creare un criterio QoS

Prima di creare un criterio QoS, è importante comprendere i due controlli chiave QoS che vengono utilizzati per gestire il traffico di rete:

- Valore DSCP

-   Velocità in uscita

### <a name="prioritizing-traffic-with-dscp"></a>Assegnazione di priorità del traffico con DSCP

Come indicato nell'esempio precedente applicazione line-of-business, è possibile definire la priorità del traffico di rete in uscita tramite **Specifica valore DSCP** per configurare un criterio QoS con un valore DSCP specifico. 

Come descritto nella RFC 2474, DSCP accetta valori compresi tra 0 e 63 per essere specificato all'interno del campo TOS di un pacchetto IPv4 e all'interno del campo della classe di traffico IPv6. Router di rete utilizzare il valore DSCP per classificare i pacchetti di rete e li coda in modo appropriato.
  
> [!NOTE]
>  Per impostazione predefinita, il traffico di Windows ha un valore DSCP pari a 0.
  
Il numero di code e il relativo comportamento definizione delle priorità deve essere progettati come parte della strategia QoS dell'organizzazione. Ad esempio, l'organizzazione potrebbe scegliere di cinque code: traffico sensibili alla latenza, traffico di controllo, traffico business-critical, il traffico di tipo massimo sforzo e traffico di trasferimento di dati di massa.  
  
### <a name="throttling-traffic"></a>Limitazione del traffico

Insieme, i valori DSCP limitazione è un altro controllo chiave per la gestione della larghezza di banda di rete. Come accennato in precedenza, è possibile utilizzare il **specifica velocità in uscita** impostazione per configurare un criterio QoS con una velocità specifico per il traffico in uscita. Tramite la limitazione, un criterio QoS limita il traffico di rete in uscita a una velocità specificata. Contrassegno DSCP e limitazione possono essere utilizzati insieme per gestire il traffico in modo efficace.

>[!NOTE]
>Per impostazione predefinita, il **specifica velocità in uscita** casella di controllo non è selezionata.

Per creare un criterio QoS, modificare le impostazioni di un oggetto (criteri di gruppo) dall'interno dello strumento di Console Gestione criteri di gruppo (GPMC). Gestione criteri di gruppo viene aperto l'Editor oggetto Criteri di gruppo.

I nomi dei criteri QoS deve essere univoci. Applicazione dei criteri per i server e gli utenti finali dipende in cui il criterio QoS viene archiviato in Editor criteri di gruppo:

- Un criterio QoS archiviato in Configurazione computer\Impostazioni Settings\QoS criteri si applica ai computer, indipendentemente dall'utente attualmente connesso. In genere utilizzato criteri QoS basati su computer per i computer server.

- Un criterio QoS archiviato in Configurazione computer\Impostazioni di utente Settings\QoS criteri si applica agli utenti dopo l'accesso, indipendentemente dal computer che sono connessi.

#### <a name="to-create-a-new-qos-policy-with-the-qos-policy-wizard"></a>Per creare un nuovo criterio QoS con la procedura guidata criteri QoS

-   In Editor oggetti Criteri di gruppo, fare doppio clic su uno del **criterio QoS** nodi, quindi fare clic su **creare un nuovo criterio **.

### <a name="wizard-page-1---policy-profile"></a>Pagina della procedura guidata 1 - profilo criterio

Nella prima pagina della procedura guidata criteri di QoS, è possibile specificare un nome di criterio e configurare come QoS controlli il traffico di rete in uscita.

#### <a name="to-configure-the-policy-profile-page-of-the-qos-based-policy-wizard"></a>Per configurare la pagina profilo criterio della procedura guidata QoS basata su criteri

1. In **nome criterio**, digitare un nome per il criterio QoS. Il nome deve identificare in modo univoco il criterio.

2. Facoltativamente, utilizzare **Specifica valore DSCP** per abilitare il contrassegno DSCP e quindi configurare un valore DSCP compreso tra 0 e 63.

3. Facoltativamente, utilizzare **specifica velocità in uscita** per abilitare la limitazione del traffico e configurare la velocità. È possibile specificare l'unità di kilobyte per secondo \(KBps\) o megabyte per secondo \(MBps\) il valore della velocità deve essere maggiore di 1.

4. Fare clic su **Avanti**.

### <a name="wizard-page-2---application-name"></a>Pagina della procedura guidata nome applicazione - 2

Nella seconda pagina della procedura guidata criteri di QoS è possibile applicare il criterio a tutte le applicazioni, a un'applicazione specifica come identificato dal relativo nome del file eseguibile, in un percorso e nome dell'applicazione o alle applicazioni server HTTP che gestiscono le richieste per un URL specifico.

- **Tutte le applicazioni** specifica che le impostazioni di gestione del traffico nella prima pagina della procedura guidata criteri di QoS si applicano a tutte le applicazioni.

- **Solo le applicazioni con questo nome di eseguibile** specifica che le impostazioni di gestione del traffico nella prima pagina della procedura guidata criteri QoS per un'applicazione specifica. Il file eseguibile assegna un nome e deve terminare con l'estensione .exe.

- **Solo le applicazioni server HTTP risponde alle richieste per questo URL** specifica che le impostazioni di gestione del traffico nella prima pagina della procedura guidata criteri di QoS riguardano solo alcune applicazioni server HTTP.

Facoltativamente, è possibile immettere il percorso dell'applicazione. Per specificare un percorso dell'applicazione, includere il percorso con il nome dell'applicazione. Il percorso può includere le variabili di ambiente. Ad esempio, %ProgramFiles%\My Path\MyApp.exe o c:\program files\my application path\myapp.exe.

>[!NOTE]
>Il percorso dell'applicazione non può includere un percorso che consente di risolvere un collegamento simbolico.

L'URL deve essere conforme alle [RFC 1738](http://tools.ietf.org/html/rfc1738), sotto forma di `http[s]://<hostname\>:<port\>/<url-path>`. È possibile utilizzare un carattere jolly, `‘*’`, per `<hostname>`e/o `<port>`, ad esempio `http://training.\*/, https://\*.\*`, ma il carattere jolly non può indicare una sottostringa `<hostname>`o `<port>`.

In altre parole, né `http://my\*site/`né `https://\*training\*/`è valido. 

Facoltativamente, è possibile controllare **includono le sottodirectory e i file** per eseguire tutte le sottodirectory e i file seguente un URL di ricerca. Ad esempio, se questa opzione è selezionata e l'URL è `http://training`, QoS criteri prenderà in considerazione le richieste di` http://training/video` perfettamente.

#### <a name="to-configure-the-application-name-page-of-the-qos-policy-wizard"></a>Per configurare la pagina Nome applicazione della procedura guidata criteri QoS

1. In **si applica questo criterio QoS per**, selezionare **tutte le applicazioni** o **solo le applicazioni con questo nome di eseguibile **.

2. Se si seleziona **solo le applicazioni con questo nome di eseguibile**, specificare un nome di eseguibile che termina con l'estensione .exe.

3. Fare clic su **Avanti**.

### <a name="wizard-page-3---ip-addresses"></a>Pagina della procedura guidata 3 - indirizzi IP

Nella terza pagina della procedura guidata criteri di QoS è possibile specificare le condizioni di indirizzi IP per il criterio QoS, tra cui:

- Tutti gli indirizzi IPv4 o IPv6 di origine specifici o indirizzi IPv4 o IPv6 di origine

- Tutti gli indirizzi IPv4 o IPv6 di destinazione o indirizzi IPv4 o IPv6 di destinazione specifici

Se si seleziona **solo per il seguente indirizzo IP di origine** o **solo per il seguente indirizzo IP di destinazione**, è necessario digitare uno dei seguenti:

- Indirizzo IPv4, ad esempio `192.168.1.1`

- Un prefisso di indirizzo IPv4 con notazione della lunghezza prefisso rete, ad esempio `192.168.1.0/24`

- Indirizzo IPv6, ad esempio `3ffe:ffff::1`

- IPv6 indirizzi prefisso, ad esempio `3ffe:ffff::/48`

Se si seleziona entrambi **solo per il seguente indirizzo IP di origine** e **solo per il seguente indirizzo IP di destinazione**, entrambi gli indirizzi o prefissi di indirizzo devono essere entrambi IPv4 o IPv6 basata.

Nella pagina precedente della procedura guidata è specificato l'URL per le applicazioni server HTTP, si noterà che l'indirizzo IP di origine per il criterio di QoS in questa pagina della procedura guidata è inattivo. 

Ciò è vero perché l'indirizzo IP di origine è l'indirizzo del server HTTP e non è configurabile qui. D'altra parte, è comunque possibile personalizzare i criteri di specificando l'indirizzo IP di destinazione. Questo rende possibile per la creazione di criteri diversi per client diversi con le stesse applicazioni server HTTP.

#### <a name="to-configure-the-ip-addresses-page-of-the-qos-policy-wizard"></a>Per configurare la pagina indirizzi IP della procedura guidata criteri QoS

1. In **si applica questo criterio QoS per** (origine) selezionare **qualsiasi indirizzo IP di origine** o **solo per i seguenti IP di origine indirizzo **.

2. Se si è selezionato **solo il seguente indirizzo IP di origine**, specificare un indirizzo IPv4 o IPv6 o un prefisso.

3. In **si applica questo criterio QoS per** (destinazione) selezionare **qualsiasi indirizzo di destinazione** o **solo per l'indirizzo IP di destinazione.**

4. Se si è selezionato **solo per l'indirizzo IP di destinazione**, specificare un indirizzo IPv4 o IPv6 o un prefisso che corrisponde al tipo di indirizzo o prefisso specificato per l'indirizzo di origine.

5.  Fare clic su **Avanti**.  

### <a name="wizard-page-4---protocols-and-ports"></a>Pagina della procedura guidata 4 - protocolli e porte

Nella quarta pagina della procedura guidata criteri di QoS, è possibile specificare i tipi di traffico e le porte controllati dalle impostazioni nella prima pagina della procedura guidata. È possibile specificare:  
-   Il traffico TCP, il traffico UDP o entrambi  

-   Tutte le porte, un intervallo di porte di origine o una porta di origine specifici di origine

-   Tutte le porte di destinazione, un intervallo di porte di destinazione o una porta di destinazione specifici  

#### <a name="to-configure-the-protocols-and-ports-page-of-the-qos-policy-wizard"></a>Per configurare la pagina protocolli e porte della procedura guidata criteri QoS

1. In **selezionare il protocollo di questo criterio QoS viene applicato a**selezionare **TCP**, **UDP**, o **TCP e UDP **.

2. In **specificare il numero di porta di origine**selezionare **da qualsiasi porta di origine** o **da questo numero di porta di origine **.

3. Se si è selezionato **da questo numero di porta di origine**, digitare un numero di porta compreso tra 1 e 65535.

     Facoltativamente, è possibile specificare un intervallo di porte, nel formato "*bassa*:*elevata*," in cui *bassa* e *elevata* rappresentano i limiti inferiore e superiore dell'intervallo di porte, inclusi. *Basso* e *elevata* devono corrispondere a un numero compreso tra 1 e 65535. Spazio non è consentito tra il carattere punto (.) e i numeri.

4. In **specificare il numero di porta di destinazione**selezionare **con qualsiasi porta di destinazione** o **verso questo numero di porta di destinazione**.

5. Se si è selezionato **verso questo numero di porta di destinazione** nel passaggio precedente, digitare un numero di porta compreso tra 1 e 65535.

Per completare la creazione del nuovo criterio QoS, fare clic su **fine** nel **protocolli e porte** pagina della procedura guidata criteri QoS. Al termine, il nuovo criterio QoS è elencato nel riquadro dei dettagli di Editor criteri di gruppo.  
  
Per applicare le impostazioni dei criteri QoS a utenti o computer, collegare l'oggetto Criteri di gruppo in cui si trovano a un contenitore di servizi di dominio Active Directory, ad esempio un dominio, un sito o un'unità organizzativa (OU), i criteri QoS.  
  
##  <a name="bkmk_editpolicy"></a>Visualizzare, modificare o eliminare un criterio QoS

Le pagine dei criteri QoS guidata descritto in precedenza corrispondono alle pagine delle proprietà che vengono visualizzate quando si visualizza o modificano le proprietà di un criterio.  
  
### <a name="to-view-the-properties-of-a-qos-policy"></a>Per visualizzare le proprietà di un criterio QoS  
  
-   Pulsante destro del mouse sul nome del criterio nel riquadro dei dettagli di Editor criteri di gruppo, quindi fare clic su **proprietà**.  
  
     Editor oggetti Criteri di gruppo consente di visualizzare la pagina delle proprietà con le schede seguenti:  
  
    -   Profilo criterio  
  
    -   Nome dell'applicazione  
  
    -   Indirizzi IP  
  
    -   Protocolli e porte  
  
### <a name="to-edit-a-qos-policy"></a>Per modificare un criterio QoS  
  
-   Pulsante destro del mouse sul nome del criterio nel riquadro dei dettagli di Editor criteri di gruppo, quindi fare clic su **Modifica criterio esistente**.  
  
     Editor oggetti Criteri di gruppo Visualizza le **modificare un criterio QoS esistente** la finestra di dialogo.  
  
### <a name="to-delete-a-qos-policy"></a>Per eliminare un criterio QoS  
  
-   Pulsante destro del mouse sul nome del criterio nel riquadro dei dettagli di Editor criteri di gruppo, quindi fare clic su **eliminare criteri**.  
  
### <a name="qos-policy-gpmc-reporting"></a>Report Gestione criteri di gruppo criterio QoS 

Dopo aver applicato un numero di criteri di QoS dell'organizzazione, potrebbe essere utile o necessario rivedere periodicamente la modalità in cui sono applicati i criteri. È possibile visualizzare un riepilogo dei criteri QoS per un utente specifico o un computer utilizzando i report di gestione criteri di gruppo.  
  
#### <a name="to-run-the-group-policy-results-wizard-for-a-report-of-qos-policies"></a>Per eseguire la creazione guidata rapporti Criteri di gruppo per un report dei criteri QoS  
  
-   In Gestione criteri di gruppo, fare doppio clic su di **rapporti Criteri di gruppo** nodo e quindi seleziona il menu di opzione per **Creazione guidata rapporti Criteri di gruppo.**  
  
Dopo aver generati i risultati dei criteri di gruppo, fare clic su di **impostazioni** scheda. Nel **impostazioni** scheda, i criteri QoS sono reperibili nei nodi "Configurazione computer\Impostazioni Settings\QoS dei criteri" e "Configurazione computer\Impostazioni Settings\QoS di criteri".  
  
Nel **impostazioni** scheda, con i relativi nomi di criterio QoS con il valore DSCP, velocità, le condizioni dei criteri sono elencati i criteri QoS e vincere oggetto Criteri di gruppo elencate nella stessa riga.  
  
La visualizzazione dei risultati di criteri di gruppo identifica in modo univoco i criteri di gruppo dominante. Quando più oggetti Criteri di gruppo sono i criteri QoS con lo stesso nome dei criteri QoS, viene applicato l'oggetto Criteri di gruppo con precedenza più alta oggetto Criteri di gruppo. Si tratta di criteri di gruppo dominante. Criteri in conflitto QoS (identificati dal nome criterio) collegati a una priorità più bassa oggetto Criteri di gruppo non vengono applicati. Nota l'oggetto Criteri di gruppo priorità definire criteri QoS distribuiti nel sito, dominio o unità Organizzativa, come appropriato. Dopo la distribuzione, a livello utente o computer, il [regole di precedenza dei criteri QoS](#BKMK_precedencerules) determinare il tipo di traffico è consentito e bloccato.  
  
I criteri di QoS valore DSCP velocità in uscita e le condizioni dei criteri sono visibili in Editor oggetti gruppo di criteri (GPOE)  
  
### <a name="advanced-settings-for-roaming-and-remote-users"></a>Impostazioni avanzate per gli utenti mobili e remoti  
Con i criteri QoS, l'obiettivo è per gestire il traffico in una rete aziendale. Negli scenari di dispositivi mobili, gli utenti potrebbero inviare traffico o disattivare la rete aziendale. Poiché i criteri QoS non sono pertinenti fuori la rete aziendale, solo su interfacce di rete connesse alla rete aziendale per Windows 8, Windows 7 o Windows Vista sono abilitati i criteri QoS.  
  
Ad esempio, un utente potrebbe connettersi proprio computer portatile per la rete aziendale tramite la rete privata virtuale (VPN) da un bar. Per la VPN, l'interfaccia di rete fisica (ad esempio wireless) non sarà possibile applicati i criteri QoS. Tuttavia, l'interfaccia VPN avranno criteri QoS applicati perché si connette alla rete aziendale. Se l'utente immette in un secondo momento un'altra rete aziendale che non dispone di una relazione di trust di Active Directory, i criteri QoS non saranno abilitati.  
  
Tieni presente che questi scenari per dispositivi mobili non sono valide per carichi di lavoro server. Ad esempio, un server con più schede di rete potrebbe essere posizionato sul bordo di una rete aziendale. Il reparto IT può scegliere di usare il traffico di limitazione criteri QoS che ha un'uscita dell'organizzazione; Tuttavia, questa scheda di rete che invia il traffico in uscita non necessariamente connettersi alla rete aziendale. Per questo motivo, i criteri QoS sono sempre abilitati su tutte le interfacce di rete di un computer che esegue Windows Server 2012.  
  
> [!NOTE]
>  Abilitazione selettiva solo per i criteri QoS e non per le impostazioni QoS avanzate descritti più avanti in questo documento.  
  
### <a name="advanced-qos-settings"></a>Impostazioni QoS avanzate

Impostazioni QoS avanzate forniscono ulteriori controlli per gli amministratori IT di gestire il consumo di rete di computer e i contrassegni DSCP. Impostazioni QoS avanzate si applicano solo a livello di computer, mentre i criteri QoS possono essere applicati a livello di computer e utente.

#### <a name="to-configure-advanced-qos-settings"></a>Per configurare le impostazioni QoS avanzate

1.  Fare clic su **configurazione Computer**, quindi fare clic su **impostazioni di Windows in Criteri di gruppo**.
  
2.  Fare doppio clic su **criterio QoS**, quindi fare clic su **impostazioni QoS avanzate**.

     La figura seguente illustra i due avanzate schede delle impostazioni QoS: **traffico TCP in entrata** e **Override contrassegno DSCP**.
  
> [!NOTE]
>  Impostazioni QoS avanzate sono impostazioni di criteri di gruppo a livello di computer.
  
#### <a name="advanced-qos-settings-inbound-tcp-traffic"></a>Impostazioni QoS avanzate: traffico TCP in ingresso

**Il traffico TCP in ingresso** controlli il consumo di larghezza di banda TCP sul lato del destinatario, mentre i criteri QoS influenzano il traffico TCP e UDP in uscita. 

Impostando una velocità effettiva di basso livello sul **traffico TCP in entrata** scheda TCP limita le dimensioni viene annunciata finestra di ricezione TCP. L'effetto di questa impostazione verrà maggiore velocità e collegare l'utilizzo delle connessioni TCP con larghezze o le latenze (prodotto ritardo della larghezza di banda). Per impostazione predefinita, i computer che eseguono Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista sono impostati sul livello di velocità effettiva massima.
  
Il protocollo TCP ricevere finestra è stato modificato in Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista da versioni precedenti di Windows. Le versioni precedenti di Windows limitata la finestra di receive-side TCP a un massimo di 64 kilobyte (KB), mentre Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista in modo dinamico le dimensioni di finestra receive-side fino a 16 megabyte (MB). Nel controllo del traffico TCP in ingresso, è possibile controllare il livello di velocità effettiva in ingresso, impostando il valore massimo per cui può raggiungere il-finestra di ricezione TCP. I livelli corrispondono ai valori massimi seguenti. 
  
|Livello di velocità effettiva in ingresso|Valore massimo|  
|------------------------|-------|  
|0|64 KB|
|1|256 KB|
|2|1 MB|
|3|16 MB|

Le dimensioni della finestra effettivo potrebbero essere un valore uguale o inferiore rispetto al massimo, a seconda delle condizioni della rete.

###### <a name="to-set-the-tcp-receive-side-window"></a>Per impostare la finestra sul lato ricezione TCP

1. In Editor oggetti Criteri di gruppo, fare clic su **criteri del Computer locale**, fare clic su **le impostazioni di Windows**, fare clic **criterio QoS**, quindi fare clic su **impostazioni QoS avanzate**.
  
2. In **velocità effettiva di ricezione TCP**selezionare **configurare la velocità effettiva ricezione TCP**e quindi selezionare il livello di velocità effettiva che vuoi.

3.  Collegare l'oggetto Criteri di gruppo all'unità Organizzativa.

#### <a name="advanced-qos-settings-dscp-marking-override"></a>Impostazioni QoS avanzate: Override contrassegno DSCP

Override contrassegno DSCP limita la capacità di applicazioni per specificare, o "contrassegnare", ovvero i valori DSCP diversi da quelli specificati nei criteri di QoS. Se si specifica che le applicazioni sono autorizzate a impostare i valori DSCP, le applicazioni possono impostare i valori DSCP diverso da zero. 

Specificando **ignora**, le applicazioni che utilizzano QoS APIs avranno i valori DSCP impostati su zero e impostare i valori DSCP solo i criteri QoS. 

Per impostazione predefinita, i computer che eseguono Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista consentono alle applicazioni di specificare i valori DSCP; applicazioni e i dispositivi che non utilizzano le APIs QoS non vengono ignorati.

##### <a name="wireless-multimedia-and-dscp-values"></a>Valori DSCP e wireless Multimedia

Il [Wi-Fi Alliance](https://go.microsoft.com/fwlink/?LinkId=160769) ha stabilito una certificazione per Wireless Multimedia \(WMM\) che definisce quattro categorie di accesso \(WMM_AC\) per definire le priorità del traffico di rete trasmessi in una rete wireless Wi\-Fi. Le categorie di accesso sono \ (in ordine di più alto per minimo priority\): audio, video, impegno, migliore e sfondo; rispettivamente abbreviato in ARMENO, VI, BE e BK. La specifica WMM definisce quali DSCP valori corrispondono con ciascuna delle categorie di accesso di quattro:
  
|Valore DSCP|Categoria di accesso WMM|
|----------|-------------------|
|48-63|Voce (ARMENO)|
|32-47|Video (VI)|
|24-31, 0-7|Massimo sforzo (BE)|
|8-23|Sfondo (BK)|

È possibile creare criteri QoS che utilizzano questi valori DSCP per assicurare che i computer portatili con Wi\-Fi Certified™ per schede di rete wireless WMM riceveranno con priorità di gestione quando è associato certificati Wi\-Fi per i punti di accesso WMM.
  
### <a name="BKMK_precedencerules"></a>Regole di precedenza dei criteri QoS

Simile alle priorità dell'oggetto Criteri di gruppo, criteri di QoS di dispongono di regole di precedenza a risolvere i conflitti quando più criteri QoS si applicano a un set specifico di traffico. Per il traffico TCP o UDP in uscita, un solo i criteri di QoS possono essere applicati alla volta, il che significa che i criteri QoS non dispongono di un effetto cumulativo, ad esempio in cui somma tassi di limitazione.

In generale, il criterio QoS con più condizioni corrispondenti wins. Quando si applicano più criteri QoS, le regole rientrano in tre categorie: livello di utente rispetto a livello di computer; applicazione rispetto quintupla la rete; e tra la rete quintuple.

Da *rete quintupla*, ci significa che l'indirizzo IP di origine, indirizzo IP di destinazione, porta di origine, porta di destinazione e protocollo \(TCP/UDP\).  

 **Criteri QoS a livello di utente ha la precedenza sui criteri QoS a livello di computer**

Questa regola semplifica notevolmente la gestione degli amministratori di rete di QoS di oggetti Criteri di gruppo, in particolare per i criteri di gruppo basato su utente. Ad esempio, se l'amministratore di rete deve definire un criterio QoS per un gruppo di utenti, vengono semplicemente creare e distribuire un oggetto Criteri di gruppo al gruppo. Non è necessario sui computer che gli utenti connessi e se tali computer avrà criteri QoS in conflitto definiti, perché, se esiste un conflitto, i criteri a livello di utente ha sempre la precedenza.

> [!NOTE]
>  Un criterio QoS a livello di utente è applicabile solo a traffico generato da tale utente. Altri utenti di un computer specifico e se il computer stesso, non saranno soggetti a tutti i criteri QoS che vengono definiti per l'utente.

 **Applicazione specificità e tenendo la precedenza quintupla di rete**

Quando più criteri QoS corrispondano al traffico specifico, vengono applicati i criteri più specifici. Tra i criteri di identificano le applicazioni, un criterio che include il percorso di file dell'applicazione di invio è considerato più specifico di un altro criterio che identifica solo il nome dell'applicazione (alcun percorso). Se applicano ancora più criteri con le applicazioni, le regole di precedenza utilizzano la rete quintupla per trovare la corrispondenza migliore.

In alternativa, più criteri QoS potrebbero applicare al traffico stesso specificando le condizioni non sovrapposte. Tra le condizioni di applicazioni e quintupla la rete, i criteri che specifica l'applicazione sono considerato più specifico e viene applicato. 

Ad esempio, policy_A specifica solo un (app.exe) nome applicazione e policy_B specifica la destinazione IP indirizzo 192.168.1.0/24. Quando questi criteri QoS in conflitto \ (app.exe invia traffico a un indirizzo IP compreso nell'intervallo di 192.168.4.0/24\), policy_A viene applicata.

 **Ulteriori specificità ha la precedenza all'interno della rete quintupla**

Per i conflitti tra criteri all'interno di quintupla di rete, i criteri con le condizioni più corrispondenti ha la precedenza. Si supponga ad esempio policy_C specifica l'indirizzo IP di origine "qualsiasi", IP di destinazione indirizzo 10.0.0.1, porta di origine "qualsiasi" porta di destinazione "qualsiasi" e "TCP" del protocollo. 

Si supponga policy_D specifica l'indirizzo IP di origine "qualsiasi", "qualsiasi" porta di destinazione 80 10.0.0.1, porta di origine di indirizzi IP di destinazione e protocollo "TCP". Quindi policy_C e policy_D corrispondere le connessioni a 10.0.0.1:80 di destinazione. Poiché il criterio QoS viene applicato il criterio con le condizioni più specifiche corrispondente, policy_D ha la precedenza in questo esempio.  
  
Tuttavia, i criteri QoS potrebbero essere lo stesso numero di condizioni. Ad esempio, diversi criteri possono specificare ognuno uno solo, ma non lo stesso, parte di quintupla la rete. Tra le quintupla di rete, il seguente è superiore a hanno la precedenza:

- Indirizzo IP di origine

- Indirizzo IP di destinazione

- Porta di origine

- Porta di destinazione

- Protocollo (TCP o UDP)

All'interno di una condizione specifica, come indirizzo IP, un indirizzo IP più specifico viene trattato con precedenza maggiore; ad esempio, un indirizzo IP 192.168.4.1 è più specifico 192.168.4.0/24.

Progettare i criteri QoS in modo specifico semplificare la capacità dell'organizzazione per comprendere quali criteri sono in vigore.

Per l'argomento successivo in questa Guida, vedere [errori ed eventi di criteri di QoS](qos-policy-errors.md).

Per il primo argomento in questa Guida, vedere [criteri Quality of Service (QoS)](qos-policy-top.md).
