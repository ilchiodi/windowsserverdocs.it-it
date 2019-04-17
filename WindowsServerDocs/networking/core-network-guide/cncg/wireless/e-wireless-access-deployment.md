---
title: Distribuzione di accesso wireless
description: Questo argomento fa parte della Guida "Distribuzione basata su Password 802.1 X Authenticated Wireless Access" rete di Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f78da14b91e5c1e9a2dc22f92ea95c89153befa8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="wireless-access-deployment"></a>Distribuzione di accesso wireless

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Segui questi passaggi per distribuire accesso senza fili:

- [Distribuire e configurare punti di accesso Wireless](#bkmk_aps)

- [Creare un gruppo di sicurezza utenti Wireless](#bkmk_groups)

- [Configurare la rete senza fili \ (IEEE 802.11\) criteri](#bkmk_policies)

- [Configurare i server dei criteri di rete](#bkmk_nps)

- [Aggiungere nuovi computer Wireless al dominio](#bkmk_domain)

## <a name="bkmk_aps"></a>Distribuire e configurare punti di accesso Wireless

Segui questi passaggi per distribuire e configurare punti di accesso wireless:

- [Specificare le frequenze di canale AP senza fili](#bkmk_channel)

- [Configurare i punti di accesso Wireless](#bkmk_wirelessaps)

>[!NOTE]
>Le procedure descritte in questa Guida non includono istruzioni per i casi in cui il **controllo dell'Account utente** viene visualizzata la finestra di dialogo per richiedere l'autorizzazione a continuare. Se viene visualizzata questa finestra di dialogo mentre si siano eseguendo le procedure in questa Guida, se è stata aperta la finestra di dialogo in risposta alle azioni dell'utente, fare clic su **continua**.

### <a name="bkmk_channel"></a>Specificare le frequenze di canale AP senza fili

Quando si distribuiscono più punti di accesso wireless in un unico sito geografico, è necessario configurare i punti di accesso wireless che hanno sovrapposti segnali di usare frequenze di canale univoco per ridurre le interferenze tra punti di accesso wireless.

È possibile utilizzare le seguenti linee guida per agevolare la scelta delle frequenze di canale che non sono in conflitto con altre reti wireless nella posizione geografica della rete senza fili.

- Se sono presenti altre organizzazioni con uffici in prossimità o nello stesso edificio dell'organizzazione, identificano se sono presenti reti senza fili alle organizzazioni di proprietà. Scopri sia la posizione e il canale assegnato frequenze di loro AP senza fili, perché è necessario assegnare le frequenze di canale diverso per il punto di accesso e per determinare la posizione migliore per installare il punto di accesso

- Identificare sovrapposti segnali wireless su piani accanto all'interno della propria organizzazione. Dopo che identifica la sovrapposizione di aree di copertura esterno e all'interno dell'organizzazione, assegnare delle frequenze di canale per AP senza fili, assicurandosi che di due punti di accesso wireless con sovrapposizioni nella copertura vengono assegnati frequenze di canale diverso.

### <a name="bkmk_wirelessaps"></a>Configurare i punti di accesso Wireless

Utilizzare le seguenti informazioni e la documentazione fornita dal produttore del punto di accesso wireless per configurare i punti di accesso wireless.

Questa procedura consente di enumerare gli elementi in genere configurati in un punto di accesso wireless. I nomi degli elementi possono variare in base alla marca e modello e potrebbero essere diverse da quelle nell'elenco seguente. Per informazioni dettagliate, vedere la documentazione di AP senza fili.

#### <a name="to-configure-your-wireless-aps"></a>Per configurare i punti di accesso wireless  

- **SSID**. Specificare il nome del network\(s\) wireless \ (ad esempio, ExampleWLAN\). Questo è il nome che verrà annunciato ai client senza fili.

- **Crittografia**. Specificare \(preferred\) WPA2\-Enterprise o WPA\-Enterprise e AES \(preferred\) o crittografia TKIP, a seconda di quali versioni sono supportate per le schede di rete di computer client wireless.

- **\(Static\) indirizzo IP AP wireless**. In ogni punto di accesso, configurare un indirizzo IP statico che rientra nell'intervallo di esclusione dell'ambito DHCP per la subnet. Utilizzando un indirizzo che viene escluso da assegnazione DHCP impedisce il server DHCP di assegnare lo stesso indirizzo IP a un computer o un altro dispositivo.

- **La subnet mask**. Configurare questa impostazione in base alle impostazioni di subnet mask della LAN a cui si è connessi AP senza fili.  

- **Nome DNS**. Alcuni AP senza fili può essere configurato con un nome DNS. Il servizio DNS nella rete possa risolvere i nomi DNS per un indirizzo IP. In ogni punto di accesso wireless che supportano questa funzionalità, immettere un nome univoco per la risoluzione DNS.  

- **Servizio DHCP**. Se il punto di accesso wireless è un servizio DHCP predefinita, disabilitarla.  

- **Segreto condiviso RADIUS**. Utilizzare un segreto condiviso RADIUS univoco per ogni punto di accesso wireless a meno che non si prevede di configurare i punti di accesso come client RADIUS in Criteri di rete dal gruppo. Se si prevede di configurare i punti di accesso dal gruppo in Criteri di rete, il segreto condiviso deve essere lo stesso per ogni membro del gruppo. Inoltre, ogni segreto condiviso che è utilizzare deve essere una sequenza casuale di almeno 22 caratteri che combina lettere maiuscole e minuscole, numeri e segni di punteggiatura. Per assicurare la casualità, è possibile utilizzare un generatore casuale di caratteri, ad esempio il generatore casuale di caratteri disponibile in NPS **configurazione 802.1 X** procedura guidata per creare i segreti condivisi.

>[!TIP]
>Registrare il segreto condiviso per ogni punto di accesso wireless e archiviarlo in un luogo sicuro, ad esempio una sede sicuro. Quando si configurano i client RADIUS in NPS, è necessario conoscere il segreto condiviso per ogni punto di accesso wireless.  

- **Indirizzo IP del server RADIUS**. Digitare l'indirizzo IP del server dei criteri di rete.

- **UDP port\(s\)**. Per impostazione predefinita, dei criteri di rete utilizza le porte UDP 1812 e 1645 per i messaggi di autenticazione e porte UDP 1813 e 1646 per i messaggi di accounting. Si consiglia di utilizzare le stesse porte UDP nei punti di accesso, ma se si dispone di un motivo valido per utilizzare porte diverse, assicurarsi di non solo configurare i punti di accesso con i nuovi numeri di porta ma anche riconfigurare tutti i server dei criteri di rete da utilizzare gli stessi numeri di porta come i punti di accesso. Se i punti di accesso e i server dei criteri di rete non sono configurati con le stesse porte UDP, dei criteri di rete non può ricevere o elaborare le richieste di connessione dai punti di accesso e tutti i tentativi di connessione wireless nella rete avrà esito negativo.

- **VSA**. Alcuni punti di accesso wireless richiedono attributi specifici vendor\ \(VSAs\) per fornire funzionalità AP wireless completo. VSA vengono aggiunti in Criteri di rete dei criteri di rete.

- **Filtri DHCP**. Configurare i punti di accesso wireless per bloccare i client wireless inviare pacchetti IP dalla porta UDP 68 alla rete, come documentato dal produttore del punto di accesso wireless.

- **Il filtro dei DNS**. Configurare i punti di accesso wireless per bloccare i client wireless inviare pacchetti IP dalla porta TCP o UDP 53 alla rete, come documentato dal produttore del punto di accesso wireless.

## <a name="create-security-groups-for-wireless-users"></a>Creare gruppi di sicurezza per gli utenti Wireless

Segui questi passaggi per creare uno o più utenti wireless gruppi di sicurezza e quindi aggiungere gli utenti al gruppo di sicurezza utenti wireless appropriati:

- [Creare un gruppo di sicurezza utenti Wireless](#bkmk_groups)

- [Aggiungere utenti al gruppo di sicurezza Wireless](#bkmk_addusers)

### <a name="bkmk_groups"></a>Creare un gruppo di sicurezza utenti Wireless

È possibile utilizzare questa procedura per creare un gruppo di protezione wireless in Active Directory Users e computer Microsoft Management Console \(MMC\) snap\-in.  

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

#### <a name="to-create-a-wireless-users-security-group"></a>Per creare un gruppo di sicurezza utenti wireless

1. Fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**. Il Active Directory Users and Computers snap-in viene aperto. Se non è già selezionata, fare clic sul nodo per il dominio. Ad esempio, se il dominio è example.com, fare clic su **example.com**.

2. Nel riquadro dei dettagli, fare clic con la cartella in cui si desidera aggiungere un nuovo gruppo \ (ad esempio, fare clic con **utenti**\), scegliere **New**, quindi fare clic su **gruppo**.

3. In **nuovo oggetto-gruppo**, in **nome gruppo**, digitare il nome del nuovo gruppo. Ad esempio, digitare **gruppo rete senza fili**.

4. In **ambito del gruppo**, selezionare una delle opzioni seguenti:

    - **Locale di dominio**

    - **Globale**

    - **Universal**

5. In **tipo di gruppo**selezionare **sicurezza**.

6. Fare clic su **OK**.

Se è necessario più di un gruppo di sicurezza per gli utenti wireless, ripetere questi passaggi per creare i gruppi di utenti senza fili aggiuntivi. In un secondo momento è possibile creare criteri di rete in Criteri di rete per applicare diverse condizioni e Constraints a ogni gruppo, offrendo diverse autorizzazioni di accesso e le regole di connettività.

### <a name="bkmk_addusers"></a>Aggiungere utenti al gruppo di sicurezza utenti Wireless

È possibile utilizzare questa procedura per aggiungere un utente, computer o gruppo al gruppo di sicurezza wireless nella Active Directory Users e computer Microsoft Management Console \(MMC\) snap\-in.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Per aggiungere utenti al gruppo di sicurezza wireless

1. Fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**. Verrà visualizzata la Active Directory MMC Utenti e computer. Se non è già selezionata, fare clic sul nodo per il dominio. Ad esempio, se il dominio è example.com, fare clic su **example.com**.

2. Nel riquadro dei dettagli fare doppio clic sulla cartella contenente il gruppo di protezione.

3. Il riquadro dei dettagli fare clic con il gruppo di sicurezza wireless e quindi fare clic su **proprietà**. Il **proprietà** verrà visualizzata la finestra di dialogo per il gruppo di sicurezza.

4. Nel **membri** scheda, fare clic su **Aggiungi**e quindi completare una delle procedure seguenti per aggiungere un computer o aggiungere un utente o gruppo.

##### <a name="to-add-a-user-or-group"></a>Per aggiungere un utente o gruppo

1. In **immettere i nomi degli oggetti da selezionare**, digitare il nome dell'utente o gruppo che si desidera aggiungere, quindi fare clic su **OK**.

2. Per assegnare l'appartenenza al gruppo ad altri utenti o gruppi, ripetere il passaggio 1 di questa procedura.  

##### <a name="to-add-a-computer"></a>Per aggiungere un computer

1. Fare clic su **tipi di oggetto**. Il **tipi di oggetto** apre la finestra di dialogo.

2. In **tipi di oggetto**selezionare **computer**, quindi fare clic su **OK**.

3. In **immettere i nomi degli oggetti da selezionare**, digitare il nome del computer che si desidera aggiungere e quindi fare clic su **OK**.

4. Per assegnare l'appartenenza al gruppo ad altri computer, ripetere i passaggi da 1 \-3 di questa procedura.

## <a name="bkmk_policies"></a>Configurare la rete senza fili \ (IEEE 802.11\) criteri

Segui questi passaggi per configurare la rete Wireless \ (IEEE 802.11\) estensione criteri di gruppo:

- [Aprire o aggiungere e aprire un oggetto Criteri di gruppo](#bkmk_opengpme)

- [Attivare la rete senza fili predefinita \ (IEEE 802.11\) criteri](#bkmk_activate)

- [Configurare il nuovo criterio di rete Wireless](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Aprire o aggiungere e aprire un oggetto Criteri di gruppo

Per impostazione predefinita, la funzionalità Gestione criteri di gruppo è installata nei computer che eseguono Windows Server 2016, quando è installato il ruolo server di servizi di dominio Active Directory \(AD DS\) e il server è configurato come controller di dominio. La procedura descritta di seguito viene descritto come aprire la Console Gestione criteri di gruppo \(GPMC\) sul controller di dominio. Quindi, la procedura viene descritto come per aprire un criterio di gruppo esistente eseguito a livello di oggetto \(GPO\) per la modifica, o creare un nuovo dominio oggetto Criteri di gruppo e aprirlo e modificarlo.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Per aprire o aggiungere e aprire un oggetto Criteri di gruppo

1. Nel controller di dominio, fare clic su **Start**, fare clic su **strumenti di amministrazione di Windows**, quindi fare clic su **Gestione criteri di gruppo**. Verrà visualizzata la Console Gestione criteri di gruppo.  

2. Nel riquadro a sinistra, fare doppio clic nell'insieme di strutture. Ad esempio, ottiene facendo doppio clic **foresta: example.com**.  

3. Nel riquadro a sinistra, fare doppio clic **domini**, quindi fare doppio clic il dominio per cui si desidera gestire un oggetto Criteri di gruppo. Ad esempio, ottiene facendo doppio clic **example.com**.  

4. Eseguire una delle operazioni seguenti:

    -   **Per aprire un oggetto Criteri di gruppo esistente a livello di domain \ per la modifica**, fare doppio clic sul dominio che contiene l'oggetto Criteri di gruppo che si desidera gestire, fare clic con il criterio di dominio che si desidera gestire, ad esempio il criterio dominio predefinito, quindi fare clic su **modifica**. **Editor Gestione criteri di gruppo** apre.

    -   **Per creare un nuovo oggetto Criteri di gruppo e aprire per modifica**, fare clic con il dominio per cui si desidera creare un nuovo oggetto Criteri di gruppo, quindi fare clic su **crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.

        In **nuovo oggetto Criteri di gruppo**, in **nome**, digitare un nome per il nuovo oggetto Criteri di gruppo e quindi fare clic su **OK**.

        Fare clic con il nuovo oggetto Criteri di gruppo, quindi fare clic su **modifica**. **Editor Gestione criteri di gruppo** apre.

Nella sezione successiva si utilizzerà Editor Gestione criteri di gruppo per creare criteri wireless.

### <a name="bkmk_activate"></a>Attivare la rete senza fili predefinita \ (IEEE 802.11\) criteri

Questa procedura viene descritto come attivare l'impostazione predefinita, la rete Wireless \ (IEEE 802.11\) criteri utilizzando il \(GPME\) Editor Gestione criteri di gruppo.

>[!NOTE]
>Dopo aver attivato il **Windows Vista e versioni successive** versione della rete Wireless \ (IEEE 802.11\) criteri o **Windows XP** versione, l'opzione di versione viene automaticamente rimosso dall'elenco di opzioni quando si clic con **rete senza fili \ (IEEE 802.11\) criteri**. Ciò si verifica perché dopo aver selezionato una versione di criteri, il criterio viene aggiunto nel riquadro dei dettagli di Editor quando si seleziona il **rete senza fili \ (IEEE 802.11\) criteri** nodo. Questo stato resta a meno che non si elimina il criterio wireless, momento in cui la versione del criterio wireless restituisce al menu di fare clic con **rete senza fili \ (IEEE 802.11\) criteri** nell'editor. Inoltre, i criteri wireless sono visualizzati solo nel riquadro dei dettagli di Editor quando il **rete senza fili \ (IEEE 802.11\) criteri** nodo selezionato.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Per attivare l'impostazione predefinita della rete Wireless \ (IEEE 802.11\) criteri  

1. Utilizzare la procedura precedente, **per aprire o aggiungere e aprire un oggetto Criteri di gruppo** per aprire l'editor.

2. Nell'Editor, nel riquadro a sinistra, fare doppio clic **configurazione Computer**, fare doppio clic **criteri**, fare doppio clic **le impostazioni di Windows**, quindi fare doppio clic **le impostazioni di sicurezza**.

![Criteri di gruppo di Wireless 802.1 X](../../../media/Wireless-GP/Wireless-GP.jpg)

3. In **le impostazioni di sicurezza**, fare clic con **rete senza fili \ (IEEE 802.11\) criteri**, quindi fare clic su **creare un nuovo criteri Wireless per Windows Vista e versioni successive**. 

![802.1 X Wireless criteri](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. Il **nuove proprietà di criteri di rete senza fili** apre la finestra di dialogo. In **nome criterio**, digitare un nuovo nome per il criterio o mantenere il nome predefinito. Fare clic su **OK** per salvare il criterio. Il criterio predefinito viene attivato ed elencato nel riquadro dei dettagli di editor con il nuovo nome fornito o con il nome predefinito **nuovi criteri di rete Wireless**.

![Proprietà di nuovo criterio di rete Wireless](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. Nel riquadro dei dettagli fare doppio clic **nuovi criteri di rete Wireless** per aprirlo.

Nella sezione successiva è possibile eseguire configurazione dei criteri di ordine di preferenza l'elaborazione dei criteri e le autorizzazioni di rete.

### <a name="bkmk_policyconfig"></a>Configurare il nuovo criterio di rete Wireless

È possibile utilizzare le procedure in questa sezione per configurare la rete Wireless \ (IEEE 802.11\) dei criteri. Questo criterio consente di configurare le impostazioni di sicurezza e autenticazione, gestire i profili wireless e specificare le autorizzazioni per le reti wireless che non sono configurate come reti preferite.

- [Configurare un profilo di connessione Wireless per PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Impostare l'ordine di preferenza per i profili di connessione Wireless](#bkmk_preferenceorder)  

- [Definire le autorizzazioni di rete](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurare un profilo di connessione Wireless per PEAP\-MS\-CHAP v2

Questa procedura include i passaggi necessari per configurare un PEAP\-MS\-CHAP v2 wireless profilo.  

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Per configurare un profilo di connessione wireless per PEAP\-MS\-CHAP v2

1. Nell'editor, nella finestra di dialogo Proprietà rete wireless per il criterio appena creato, scegliere il **generale** scheda e **descrizione**, digitare una breve descrizione per il criterio.

2. Per specificare che il servizio configurazione automatica WLAN viene utilizzato per configurare le impostazioni della scheda di rete wireless, assicurarsi che **servizio configurazione automatica WLAN di Windows usa per i client** sia selezionata.  

3. In **Connetti alle reti disponibili secondo l'ordine dei profili elencati di seguito**, fare clic su **Aggiungi**e quindi seleziona **infrastruttura**. Il **proprietà del nuovo profilo** apre la finestra di dialogo.

4. Nel**proprietà del nuovo profilo** della finestra di dialogo di **connessione** nella scheda il **nome profilo**, digitare un nuovo nome per il profilo. Ad esempio, digitare **Example.com WLAN profilo per Windows 10**.

5. In **rete Name\(s\) \(SSID\)**, digitare l'identificatore SSID corrispondente al SSID configurato nei punti di accesso wireless e quindi fare clic su **Aggiungi**.

    Se la distribuzione si usa più SSID e ogni punto di accesso wireless Usa le stesse impostazioni di sicurezza wireless, ripetere questo passaggio per aggiungere il SSID per ogni punto di accesso wireless che si desidera applicare questo profilo.

    Se la distribuzione si usa più SSID e le impostazioni di sicurezza per ogni SSID non corrispondono, configurare un profilo separato per ogni gruppo di SSID che usa le stesse impostazioni di sicurezza. Ad esempio, se si dispone di un gruppo di punti di accesso wireless configurati per utilizzare WPA2\-Enterprise e AES e un altro gruppo di punti di accesso wireless e per utilizzare WPA\ Enterprise e TKIP, configurare un profilo per ogni gruppo di punti di accesso wireless.

6. Se il testo predefinito **NEWSSID** è presente, selezionarlo e quindi fare clic su **rimuovere**.

7. Se è stato distribuito punti di accesso wireless configurati per eliminare il beacon di trasmissione, selezionare **Connetti anche se la rete non sta trasmettendo**.

    > [!NOTE]
    > Per abilitare questa opzione, è possibile creare un rischio per la sicurezza perché i client wireless effettueranno la ricerca e tentativi di connessione a qualsiasi rete wireless. Per impostazione predefinita, questa impostazione non è abilitata.  

8. Fare clic su di **sicurezza** scheda, fare clic su **avanzate**e quindi configurare quanto segue:

    1. Per configurare impostazioni avanzate 802.1 X, in **IEEE 802.1 X**selezionare **Imponi impostazioni avanzate 802.1 X**.

        Quando l'avanzate per 802.1 X impostazioni vengono applicate, i valori predefiniti di **Max Eapol\ di avvio EAPOL**, **periodo di sospensione**, **periodo di avvio**, e **periodo di autenticazione** sono sufficienti per le distribuzioni wireless tipiche. Per questo motivo, non è necessario modificare le impostazioni predefinite a meno che non hai un motivo specifico per questa operazione.

    2. Per abilitare Single Sign-On, selezionare **abilitare Single Sign-On per la rete**.

    3. I valori predefiniti rimanenti **Single Sign-On** sono sufficienti per le distribuzioni wireless tipiche.

    4. In **Roaming veloce**, se il punto di accesso wireless è configurato per pre-autenticazione, selezionare **la rete utilizza pre-autenticazione**.

9. Per specificare che le comunicazioni wireless soddisfino FIPS 140\-2, selezionare **Esegui crittografia in modalità certificata 140\ 2 FIPS**.

10. Fare clic su **OK** per tornare al **sicurezza** scheda. In **selezionare i metodi di sicurezza per la rete**, in **autenticazione**selezionare **WPA2\ Enterprise** se è supportato il punto di accesso wireless e schede di rete client wireless. In caso contrario, selezionare **WPA\ Enterprise**.

11. In **crittografia**, se supportato il punto di accesso wireless e schede di rete client wireless, selezionare **AES-CCMP**. Se si utilizza punti di accesso e schede di rete wireless che supportano 802.11 AC, selezionare **AES-GCMP**. In caso contrario, selezionare **TKIP**.

    > [!NOTE]  
    > Le impostazioni delle opzioni **autenticazione** e **crittografia** deve corrispondere alle impostazioni configurate nei punti di accesso wireless. Le impostazioni predefinite per **modalità di autenticazione**, **numero massimo errori di autenticazione**, e **memorizza informazioni utente per successive connessioni a questa rete** sono sufficienti per le distribuzioni wireless tipiche.  

12. In **selezionare un metodo di autenticazione di rete**selezionare **PEAP \(PEAP\)**, quindi fare clic su **proprietà**. Il **proprietà PEAP** apre la finestra di dialogo.

13. In **proprietà PEAP**, verificare che **verificare l'identità del server mediante convalida del certificato** sia selezionata.

14. In **autorità di certificazione radice attendibili**, selezionare l'autorità di certificazione radice attendibile \(CA\) che ha emesso il server dei certificati per il server dei criteri di rete.

    > [!NOTE]  
    > Questa impostazione limita le CA radice che i client considerano attendibili a quelle selezionate. Se non è selezionata nessuna CA radice attendibili, i client considereranno attendibili che tutte le CA radice elencate nell'archivio certificati delle autorità di certificazione radice attendibili.  

15. Nel **selezionare il metodo di autenticazione** selezionare **password protetta \ (v2\ EAP\-MS\-CHAP)**.

16. Fare clic su **configurare**. Nel **proprietà EAP MSCHAPv2** finestra di dialogo verificare **utilizza automaticamente il nome di accesso di Windows e la password \ (e dominio se any\)** sia selezionata e fare clic su **OK**.

17. Per abilitare la riconnessione rapida PEAP, assicurarsi che **Abilita riconnessione rapida** sia selezionata.

18. Per richiedere server TLV di Cryptobinding in tentativi di connessione, selezionare **Disconnetti se il server non presenta TLV di Cryptobinding **.

19. Per specificare che l'identità dell'utente viene mascherato in fase di autenticazione, selezionare **Consenti Privacy identità**e nella casella di testo, digitare un nome di identità anonima, o lasciare vuota la casella di testo.

    >[! NOTE SULLA]
    >- Deve essere creato un criterio di NPS per 802.1 X Wireless utilizzando criteri di rete **criterio di richiesta di connessione **. Se viene creato un criterio di NPS utilizzando criteri di rete **criteri di rete**, quindi privacy identità non funzionerà.
    >- Consenti privacy identità EAP è fornita da alcuni metodi EAP in cui un'identità anonima o vuota \ (diverse dall'effettivo identity\) viene inviata in risposta alla richiesta di identità EAP. PEAP invia l'identità due volte durante l'autenticazione. Nella prima fase, l'identità viene inviata in testo normale e questa identità viene utilizzata per scopi di routing, non per l'autenticazione client. L'identità reale, utilizzato per l'autenticazione, viene inviato nella seconda fase dell'autenticazione, all'interno del tunnel sicuro stabilito nella prima fase. Se **Consenti Privacy identità** casella di controllo è selezionata, il nome utente viene sostituito con la voce specificata nella casella di testo. Si supponga ad esempio **Consenti Privacy identità** sia selezionata e l'alias di privacy identità **anonimo** viene specificato nella casella di testo. Per un utente con un alias di identità reale **jdoe@example.com**, l'identità inviato nella prima fase di autenticazione verrà modificato in **anonymous@example.com**. La parte dell'area di autenticazione dell'identità fase 1 non viene modificata come viene usato per il routing.  

20. Fare clic su **OK** per chiudere la **proprietà PEAP** la finestra di dialogo.
21. Fare clic su **OK** per chiudere la **sicurezza** scheda.
22. Se si desidera creare altri profili, fare clic su **Aggiungi**e quindi ripetere i passaggi precedenti, rendendo diverse opzioni per personalizzare ogni profilo per il client wireless e di rete a cui si desidera che il profilo applicato. Una volta aggiunta profili, fare clic su **OK** per chiudere la finestra di dialogo Proprietà criterio rete senza fili.

Nella sezione successiva è possibile ordinare i profili di criteri di protezione ottimale.

#### <a name="bkmk_preferenceorder"></a>Impostare l'ordine di preferenza per i profili di connessione Wireless
Se è stato creato più profili wireless in Criteri di rete wireless e si desidera ordinare i profili per la sicurezza e l'efficacia ottimale, è possibile utilizzare questa procedura.

Per garantire che i client wireless connettersi con il massimo livello di sicurezza che possono supportare, inserire i criteri più restrittivi nella parte superiore dell'elenco.

Ad esempio, se si dispone di due profili, uno per i client che supportano WPA2 e uno per i client che supportano WPA, inserire il profilo di WPA2 superiore nell'elenco. Ciò garantisce che i client che supportano WPA2 utilizzerà tale metodo per la connessione anziché WPA meno sicuro.

Questa procedura include i passaggi per specificare l'ordine in cui vengono utilizzati i profili di connessione wireless per la connessione client wireless membri del dominio alle reti wireless.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Per impostare l'ordine di preferenza per i profili di connessione wireless

1. Nell'editor, nella finestra di dialogo Proprietà rete wireless per i criteri che appena configurato, fare clic su di **generale** scheda.

2. Nel **generale** scheda **Connetti alle reti disponibili secondo l'ordine dei profili elencati di seguito**, selezionare il profilo che si desidera spostare nell'elenco, quindi fare clic su uno di "freccia" pulsante "freccia su o giù" pulsante per spostare il profilo nella posizione desiderata nell'elenco.

3.  Ripetere il passaggio 2 per ogni profilo che si desidera spostare nell'elenco.  

4.  Fare clic su **OK** salvare tutte le modifiche.

Nella sezione seguente, è possibile definire le autorizzazioni di rete per i criteri wireless.

#### <a name="bkmk_permissions"></a>Definire le autorizzazioni di rete
È possibile configurare le impostazioni nel **autorizzazioni reti** scheda dei membri del dominio di rete senza fili \ (IEEE 802.11\) i criteri vengono applicati.

È possibile applicare solo le impostazioni seguenti per reti wireless che non siano configurate nel **generale** nella scheda il **proprietà criterio rete senza fili** pagina:

- Consentire o negare le connessioni a reti senza fili specifiche specificato dal tipo di rete e Service Set Identifier \(SSID\)

- Consentire o negare le connessioni a reti ad hoc

- Consentire o negare le connessioni a reti infrastruttura

- Consentire o negare agli utenti di visualizzare tipi di rete \ (ad hoc o infrastructure\) a cui viene negati

- Consentire o negare agli utenti di creare un profilo che si applica a tutti gli utenti

- Gli utenti possono connettersi solo a reti consentite usando i profili di criteri di gruppo

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare queste procedure.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Per consentire o negare le connessioni a reti senza fili specifiche

1. Nell'editor, nella finestra di dialogo Proprietà rete wireless, fare clic su di **autorizzazioni reti** scheda.

2. Nel **autorizzazioni reti** scheda, fare clic su **Aggiungi **. Il **nuova voce autorizzazione** apre la finestra di dialogo.

3. Nel **nuova voce autorizzazione** della finestra di dialogo di **\(SSID\) nome di rete** campo, digitare la rete SSID della rete per cui si desidera definire le autorizzazioni.

4.  In **tipo di rete**selezionare **infrastruttura** o **Ad hoc **.

    > [!NOTE]  
    > Se non si è certi che la rete di trasmissione è un'infrastruttura di rete ad hoc, è possibile configurare due voci di autorizzazione rete, uno per ogni tipo di rete.

5. In **autorizzazione**selezionare **Consenti** o **Nega **.

6. Fare clic su **OK**, per ripristinare il **autorizzazioni reti** scheda.

##### <a name="to-specify-additional-network-permissions-optional"></a>Per specificare \(Optional\) le autorizzazioni di rete

1.  Nel **autorizzazioni reti**, configurare una o tutte le operazioni seguenti:  

    -   Per negare l'accesso a reti ad hoc i membri di dominio, selezionare **Impedisci connessioni a reti ad-hoc ad\**.

    -   Per negare l'accesso a reti infrastruttura i membri di dominio, selezionare **Impedisci connessioni a reti infrastruttura **.  

    -   Per consentire ai membri del dominio visualizzare i tipi di rete \ (ad hoc o infrastructure\) a cui non hanno accesso, selezionare **consentire all'utente di visualizzare le reti negate **.

    -   Per consentire agli utenti di creare i profili che si applicano a tutti gli utenti, selezionare **Consenti al gruppo everyone di creare tutti i profili utente **.

    -   Per specificare che gli utenti possono connettersi solo a reti consentite usando i profili di criteri di gruppo, selezionare **utilizza solo profili di criteri di gruppo per le reti consentite **.

## <a name="bkmk_nps"></a>Configurare i server dei criteri di rete
Segui questi passaggi per configurare il server dei criteri di rete per eseguire l'autenticazione 802.1 X per l'accesso wireless:

- [Registrazione dei criteri di rete in servizi di dominio Active Directory](#bkmk_npsreg)

- [Configurare un punto di accesso Wireless come Client RADIUS dei criteri di rete](#bkmk_radiusclient)

- [Creare criteri di criteri per 802.1 X Wireless utilizzando una procedura guidata](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrazione dei criteri di rete in servizi di dominio Active Directory
È possibile utilizzare questa procedura per registrare un server che esegue Server dei criteri di rete \(NPS\) \(AD DS\) servizi di dominio Active Directory nel dominio in cui il server dei criteri di rete è un membro. Per i server dei criteri di rete concedere l'autorizzazione per leggere le proprietà di dial\ dell'account utente durante il processo di autorizzazione, ogni server dei criteri di rete deve essere registrato in Active Directory. La registrazione di un server dei criteri di rete consente di aggiungere il server per il **server RAS e IAS** gruppo di sicurezza in Active Directory.

>[!NOTE]
>È possibile installare NPS in un controller di dominio o in un server dedicato. Eseguire il comando seguente di Windows PowerShell per installare il server NPS se non è ancora stato fatto:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

#### <a name="to-register-an-nps-server-in-its-default-domain"></a>Per registrare un server dei criteri di rete nel dominio predefinito

1. Nel server dei criteri di rete, in **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete **. Verrà visualizzata la finestra di NPS snap-in.

2. Fare clic con **dei criteri di rete \(Local\)**, quindi fare clic su **registra Server in Active Directory **. Il **Server dei criteri di rete** apre la finestra di dialogo.

3. In **Server dei criteri di rete**, fare clic su **OK**, quindi fare clic su **OK** nuovamente.

### <a name="bkmk_radiusclient"></a>Configurare un punto di accesso Wireless come Client RADIUS dei criteri di rete
È possibile utilizzare questa procedura per configurare un punto di accesso, noto anche come un *server di accesso di rete \(NAS\)*, come un client di Remote Authentication Dial\-In utente Service \(RADIUS\) utilizzando NPS snap-in. 

>[!IMPORTANT]
>I computer client, ad esempio computer portatili wireless e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless, 802.1X\-commutatori che supportano, rete privata virtuale \(VPN\) Server e server dial\, perché usano il protocollo RADIUS per comunicare con server RADIUS, ad esempio i server dei criteri di rete.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Per aggiungere un server di accesso di rete come un client RADIUS in Criteri di rete

1. Nel server dei criteri di rete, in **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete **. Verrà visualizzata la finestra di NPS snap-in.

2. In NPS snap-in, fare doppio clic **client e server RADIUS **. Fare clic con **client RADIUS**, quindi fare clic su **New **.

3. In **nuovo Client RADIUS**, verificare che il **attiva questo client RADIUS** casella di controllo è selezionata.

4. In **nuovo Client RADIUS**, in **nome descrittivo**, digitare un nome per il punto di accesso wireless.

    Ad esempio, se si desidera aggiungere un accesso senza fili punto \(AP\) denominato AP\-01, digitare **AP\-01**.

5. In **indirizzo \(IP or DNS\)**, digitare l'indirizzo IP o nome completo \(FQDN\) nome di dominio per il server NAS.

    Se si immette il nome FQDN, per verificare che il nome sia corretto e che esegue il mapping a un indirizzo IP valido, fare clic su **verifica**e quindi **verificare indirizzo**, nel **indirizzo** campo, fare clic su **risolvere**. Se il nome FQDN è mappato a un indirizzo IP valido, l'indirizzo IP di tale NAS compariranno automaticamente **indirizzo IP**. Se non viene risolto il nome di dominio completo in un indirizzo IP che verrà visualizzato un messaggio che indica che l'host, è noto. In questo caso, verificare di avere il nome del punto di accesso corretto e che il punto di accesso sia accesa e connessa alla rete.  

    Fare clic su **OK** per chiudere **verificare indirizzo**.  

6. In **nuovo Client RADIUS**, in **segreto condiviso**, effettuare una delle operazioni seguenti:  

    -   Per configurare manualmente un segreto condiviso RADIUS, selezionare **manuale**e quindi **segreto condiviso**, digitare la password complessa che viene inserita anche sul server NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.  

    -   Per generare automaticamente un segreto condiviso, selezionare il **genera** casella di controllo e quindi scegliere il **genera** pulsante. Salvare il segreto condiviso generato e quindi usare tale valore per configurare il server NAS in modo che possa comunicare con il server dei criteri di rete.  

        >[!IMPORTANT]
        >Il segreto condiviso RADIUS immesso per il proprio AP virtuale in Criteri di rete deve corrispondere esattamente il segreto condiviso RADIUS configurato per l'effettivo AP senza fili Se si utilizza l'opzione di criteri di rete per generare un segreto condiviso RADIUS, è necessario configurare AP senza fili effettiva corrispondenza con il segreto condiviso RADIUS che è stato generato da criteri di rete.

7. In **nuovo Client RADIUS**via il **avanzate** scheda **nome del fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.

8. In **opzioni aggiuntive**, se utilizza qualsiasi metodo di autenticazione EAP e PEAP e, se il server NAS supporta l'utilizzo dell'attributo autenticatore messaggio, selezionare **messaggi di richiesta di accesso devono contenere l'attributo autenticatore Message\**.

9. Fare clic su **OK**. Il server NAS viene visualizzato nell'elenco dei client RADIUS configurati nel server dei criteri di rete.

### <a name="bkmk_npspolicy"></a>Creare criteri di criteri per 802.1 X Wireless utilizzando una procedura guidata
È possibile utilizzare questa procedura per creare criteri di richiesta di connessione e di rete criteri necessari per distribuire entrambi 802.1X\-punti di accesso wireless in grado di supportare come \(RADIUS\) Remote Authentication Dial\-In utente Service client al server RADIUS che esegue Server dei criteri di rete \(NPS\).  
Dopo aver eseguito la procedura guidata, vengono creati i seguenti criteri:

- Criteri di richiesta di una connessione

- Un criterio di rete

>[!NOTE]
>Ogni volta che è necessario creare nuovi criteri per l'accesso autenticato 802.1 X, è possibile eseguire la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Creare criteri per 802.1 X wireless autenticato tramite una procedura guidata

1. Aprire NPS snap-in. Se non è già selezionata, fare clic su **dei criteri di rete \(Local\)**. Se si eseguono MMC NPS snap-in e si desidera creare criteri in un server dei criteri di rete remoto, selezionare il server.

2. In **Introduzione**, in **configurazione Standard**selezionare **server RADIUS per connessioni cablate o Wireless 802.1 X**. Il testo e i collegamenti riportati di seguito la modifica di testo in base alle selezioni.

3. Fare clic su **configurare 802.1 X**. Apre la procedura guidata configurazione 802.1 X.

4.  Nel **Seleziona tipo di connessioni 802.1 X** pagina procedura guidata, in **tipo connessioni 802.1 X**selezionare **connessioni Wireless protette**e in **nome**, digitare un nome per il criterio o lasciare il nome predefinito **connessioni Wireless protette**. Fare clic su **Avanti**.

5.  Nel **specificare commutatori 802.1 X** pagina procedura guidata, in **client RADIUS**, tutti 802.1 X commutatori e i punti di accesso wireless che sono stati aggiunti come client RADIUS in NPS snap-in vengono visualizzati. Eseguire una delle operazioni seguenti:

    -   Per aggiungere ulteriori rete accesso server \(NASs\), ad esempio punti di accesso wireless in **client RADIUS**, fare clic su **Aggiungi**e quindi **client RADIUS nuovo**, immettere le informazioni per: **nome descrittivo**, **indirizzo \(IP or DNS\)**, e **segreto condiviso**.

    -   Per modificare le impostazioni per qualsiasi NAS in **client RADIUS**, selezionare il punto di accesso per il quale si desidera modificare le impostazioni e quindi fare clic su **modifica**. Modificare le impostazioni come necessario.

    -   Per rimuovere un server NAS dall'elenco, in **client RADIUS**, selezionare il server NAS e quindi fare clic su **rimuovere**.

        >[!WARNING]
        >Rimozione di un client RADIUS all'interno di **configurazione 802.1 X** guidata Elimina il client dalla configurazione del server dei criteri di rete. Tutte le aggiunte, modifiche ed eliminazioni apportate all'interno di **configurazione 802.1 X** procedura guidata per i client RADIUS vengono riflesse in NPS snap-in, nel **client RADIUS** nodo **dei criteri di rete** \/ **client e server RADIUS**. Ad esempio, se si utilizza la procedura guidata per rimuovere un 802.1 X passare, l'opzione viene rimossa anche da NPS snap-in.

6. Fare clic su **Avanti**. Nel **configurare un metodo di autenticazione** pagina procedura guidata, in **tipo \ (basato sul metodo di configurazione di accesso e rete)**selezionare **Microsoft: PEAP \(PEAP\)**e quindi fare clic su **configura**.

    >[!TIP]
    >Se ricevi un messaggio di errore che indica che non è possibile trovare un certificato per l'utilizzo con il metodo di autenticazione e sono stati configurati Servizi certificati di Active Directory per l'emissione automatica dei certificati per server RAS e IAS sulla rete, assicurarsi che hanno seguito i passaggi per registrare dei criteri di rete in servizi di dominio Active Directory, quindi utilizzare la procedura seguente per aggiornare criteri di gruppo : Fare clic su **avviare**, fare clic su **sistema Windows**, fare clic su **eseguire**e in **aprire**, tipo **gpupdate**, quindi premere INVIO. Quando il comando restituisce i risultati che indicano che sia utente e computer di criteri di gruppo sono aggiornati correttamente, selezionare **Microsoft: PEAP \(PEAP\)** nuovamente, quindi fare clic su **configura**.
    >
    >Se dopo l'aggiornamento dei criteri di gruppo si continua a ricevere il messaggio di errore che indica che è Impossibile trovare un certificato per l'utilizzo con il metodo di autenticazione, il certificato non viene visualizzato perché non soddisfa i requisiti del certificato server minimo come documentato nella Guida complementare alla rete Core: [distribuire certificati Server per le distribuzioni Wireless e cablate 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). In questo caso, è necessario interrompere la configurazione dei criteri di rete, revocare il certificato emesso per il server\(s\) dei criteri di rete e quindi segui le istruzioni per configurare un nuovo certificato utilizzando la Guida alla distribuzione di certificati server.

7.  Nel **Modifica proprietà PEAP** pagina procedura guidata, in **certificato emesso**, assicurarsi che sia selezionato il certificato del server dei criteri di rete corretto e quindi eseguire le operazioni seguenti:

    >[!NOTE]
    >Verificare che il valore in **dell'autorità di certificazione** corretto per il certificato selezionato **certificato emesso**. È ad esempio, l'autorità emittente previsto per un certificato emesso da un'autorità di certificazione che esegue Servizi certificati Active Directory \(AD CS\) denominato corp\DC1, nel dominio contoso.com, **corp \-DC1\-CA**.

    -   Per consentire agli utenti di eseguire il roaming con i computer wireless tra punti di accesso senza richiedere di ripetere l'autenticazione ogni volta che vengono associati a un nuovo punto di accesso, selezionare **Abilita riconnessione rapida**.

    -   Per specificare che la connessione client senza fili terminerà il processo di autenticazione di rete se il server RADIUS non presenta TLV \(TLV\) Type\-Length\-Value, selezionare **disconnettere client senza Cryptobinding**.  

    -   Per modificare i criteri digitare le impostazioni per il protocollo EAP, in **tipi EAP**, fare clic su **modifica**, in **proprietà EAP MSCHAPv2**, modificare le impostazioni in base alle esigenze e quindi fare clic su **OK**.  

8.  Fare clic su **OK**. Chiude la finestra di dialogo Modifica proprietà PEAP, si ritorna il **configurazione 802.1 X** procedura guidata. Fare clic su **Avanti**.

9. In **specificare gruppi di utenti**, fare clic su **Aggiungi**e quindi digitare il nome del gruppo di sicurezza che è configurato per i client wireless in di Active Directory Users e computer snap-in. Ad esempio, se il gruppo di protezione wireless gruppo rete senza fili denominata, digitare **gruppo rete senza fili**. Fare clic su **Avanti**.

10. Fare clic su **configura** per configurare gli attributi RADIUS standard e gli attributi specifici vendor\ per \(VLAN\) LAN virtuale in base alle esigenze e come specificato per la documentazione fornita dal produttore dell'hardware AP senza fili. Fare clic su **Avanti**.

11. Esaminare i dettagli di riepilogo di configurazione, quindi fare clic su **fine**.

È possibile passare per aggiungere i computer wireless al dominio e ora vengono creati i criteri di criteri di rete.

## <a name="bkmk_domain"></a>Aggiungere nuovi computer Wireless al dominio
Il metodo più semplice per aggiungere nuovi computer wireless al dominio consiste nel collegare fisicamente il computer a un segmento di LAN cablata \ (un segmento non controllato da un 802.1 X switch\) prima di aggiungere il computer al dominio. Questo è più semplice perché le impostazioni di criteri di gruppo wireless vengono applicate immediatamente e automaticamente e, se è stato distribuito un PKI specifico, il computer riceve il certificato della CA e lo inserisce nell'archivio certificati Autorità di certificazione radice attendibili, consentendo al client senza fili considerare attendibile il server dei criteri di rete con i certificati server rilasciati dalla CA.

Allo stesso modo, dopo che un nuovo computer wireless è stato aggiunto al dominio, il metodo preferito per gli utenti di accedere al dominio è per eseguire log su tramite una connessione alla rete cablata.

### <a name="other-domain-join-methods"></a>Altri metodi di aggiunta al dominio
Nei casi in cui non è pratico aggiungere computer al dominio utilizzando una connessione Ethernet cablata o nei casi in cui l'utente non può accedere al dominio per la prima volta tramite una connessione cablata, è necessario utilizzare un metodo alternativo.

- **Configurazione di Computer del personale IT**. Un membro del personale IT aggiunge un computer wireless al dominio e configura un profilo Single Sign On bootstrap wireless. Con questo metodo, l'amministratore IT si connette il computer wireless alla rete Ethernet cablata e aggiunge il computer al dominio. L'amministratore distribuisce quindi il computer per l'utente. Quando l'avvio utente nel computer senza utilizzare una connessione cablata, le credenziali di dominio che specificano manualmente per l'accesso dell'utente vengono utilizzati sia stabilire una connessione alla rete wireless e per accedere al dominio.

Per ulteriori informazioni, vedere la sezione [aggiungere il dominio e l'accesso tramite il metodo di configurazione di Computer del personale IT](#bkmk_itstaff)

-   **Configurazione del profilo Wireless da parte degli utenti di bootstrap**. Manualmente, l'utente configura il computer wireless con un profilo wireless bootstrap e viene aggiunto al dominio, in base alle istruzioni acquisiti da un amministratore IT. Il profilo wireless bootstrap consente all'utente di stabilire una connessione wireless e quindi accedere al dominio. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può accedere al dominio utilizzando una connessione wireless e le credenziali di account di dominio.

Per ulteriori informazioni, vedere la sezione [aggiungere il dominio e l'accesso tramite configurazione del profilo Wireless Bootstrap dagli utenti](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Aggiungere il dominio e l'accesso tramite il metodo di configurazione di Computer del personale IT
Gli utenti membri di dominio con i computer appartenenti a un domain \ client wireless è possono utilizzare un profilo wireless temporaneo per connettersi a un 802.1X\-autenticato rete senza fili senza prima una connessione alla rete LAN. Questo profilo wireless temporaneo viene chiamato un *bootstrap profilo wireless*.

Un profilo wireless bootstrap richiede all'utente di specificare manualmente le credenziali di account utente di dominio e non convalida il certificato del server che esegue Server dei criteri di rete \(NPS\) \(RADIUS\) Remote Authentication Dial\-In User Service.

Dopo aver stabilito la connessione wireless, criteri di gruppo viene applicato nel computer client wireless e un nuovo profilo wireless viene generato automaticamente. Il nuovo criterio utilizza le credenziali dell'account utente e computer per l'autenticazione client. 

Inoltre, come parte del PEAP\-MS\-CHAP v2 l'autenticazione reciproca utilizzando il nuovo profilo anziché il profilo di bootstrap, il client convalida le credenziali del server RADIUS.

Dopo avere aggiunto il computer al dominio, è possibile utilizzare questa procedura per configurare un profilo Single Sign On bootstrap wireless, prima di distribuire il computer wireless per l'utente membro di domain \.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Per configurare un profilo Single Sign On bootstrap wireless

1. Creare un profilo di bootstrap utilizzando la procedura riportata in questa guida denominata [configurare un profilo di connessione Wireless per PEAP\-MS\-CHAP v2](#bkmk_configureprofile)e utilizzare le seguenti impostazioni:

    - Autenticazione PEAP\-MS\-CHAP v2

    - Convalidare disabilitato di certificato server RADIUS

    - Single Sign-On abilitato

2. Nelle proprietà del criterio di rete Wireless in cui è stato creato il nuovo profilo di bootstrap, nel **generale** scheda, selezionare il profilo di bootstrap e quindi fare clic su **esportare** per esportare il profilo in una condivisione di rete, unità flash USB, o altra posizione facilmente accessibile. Il profilo viene salvato come file XML nel percorso specificato.

3. Aggiungere il nuovo computer wireless al dominio \ (ad esempio, tramite una connessione Ethernet che non richiede IEEE 802.1 X authentication\) e Aggiungi il profilo wireless bootstrap al computer tramite il **netsh wlan aggiungere profilo** comando.

    >[!NOTE]
    >Per ulteriori informazioni, vedere comandi Netsh per la rete locale Wireless \(WLAN\) in [http:///\/technet.microsoft.com\/library\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuire il nuovo computer wireless per l'utente con la procedura per "Accesso al dominio utilizzando computer che eseguono Windows 10".

Quando l'utente avvia il computer, Windows richiede all'utente di immettere il nome di account utente di dominio e la password. Poiché Single Sign-On è abilitato, il computer utilizza le credenziali dell'account utente di dominio per innanzitutto stabilire una connessione con la rete wireless e quindi accedere al dominio.

#### <a name="bkmk_w10"></a>Accedere al dominio utilizzando computer che eseguono Windows 10

1. Disconnettersi dal computer o riavviare il computer.

2. Premere un tasto qualsiasi sulla tastiera o fai clic sul desktop. Viene visualizzata la schermata di accesso con un nome di account utente locale visualizzato e un campo di immissione password sotto il nome. Non accedere con l'account utente locale.

3. Nell'angolo inferiore sinistro dello schermo, fare clic su **altro utente**. Nella schermata di accesso altro utente viene visualizzato con due campi, uno per il nome utente e una password. Di seguito la password è il testo **accesso a:** e quindi il nome di dominio in cui viene aggiunto il computer. Ad esempio, se il dominio è denominato example.com, il testo legge **accesso a: esempio**.

4. In **nome utente**, digitare il nome utente di dominio.

5. In **Password**, digitare la password di dominio e quindi fare clic sulla freccia o premere INVIO.

>[!NOTE]
>Se il **altro utente** schermata non include il testo **accesso a:** e il nome di dominio, è necessario immettere il nome utente nel formato *dominio\\Nome utente*. Ad esempio, per accedere al dominio con un account denominato example.com **dall'utente-01**, tipo **example\\User\-01**.

### <a name="bkmk_userbootstrap"></a>Aggiungere il dominio e l'accesso tramite configurazione del profilo Wireless Bootstrap dagli utenti
Con questo metodo, la procedura nella sezione passaggi generali, quindi specificare gli utenti membri di domain \ con le istruzioni su come configurare manualmente un computer senza fili con un profilo wireless bootstrap. Il profilo wireless bootstrap consente all'utente di stabilire una connessione wireless e quindi accedere al dominio. Dopo che il computer è aggiunto al dominio e riavviato, l'utente può accedere al dominio tramite una connessione wireless.

#### <a name="general-steps"></a>Passaggi generali

1. Configurare un account di amministratore del computer locale, in **Pannello di controllo**, per l'utente.

    >[!IMPORTANT]
    >Per aggiungere un computer a un dominio, l'utente deve essere connesso al computer con l'account amministratore locale. In alternativa, l'utente deve fornire le credenziali per l'account amministratore locale durante il processo di aggiunta del computer al dominio. Inoltre, l'utente deve disporre di un account utente nel dominio a cui l'utente vuole aggiungere il computer. Durante il processo di aggiunta del computer al dominio, l'utente verrà richiesto per le credenziali dell'account di dominio \ (nome utente e password\).

2. Fornire agli utenti di dominio con le istruzioni per configurare un profilo wireless bootstrap, come descritto nella procedura seguente **per configurare un profilo wireless bootstrap**.
3. Inoltre, fornire agli utenti con entrambe le credenziali del computer locale \ (nome utente e password\), le credenziali di dominio e \ (nome di account utente di dominio e password\) nel formato *nomedominio omeutente*, nonché le procedure "Aggiungere il computer al dominio," e "Accesso al dominio," come indicato in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Per configurare un profilo wireless bootstrap

1. Utilizzare le credenziali fornite dall'amministratore di rete o il supporto IT professional per accedere al computer con account di amministratore del computer locale.

2. Fare clic sull'icona della rete sul desktop e fare clic su sottoporli **centro aprire rete e condivisione**. **Centro rete e condivisione** apre. In **modificare le impostazioni di rete**, fare clic su **impostare una nuova connessione o rete**. Il **impostare una connessione o rete** apre la finestra di dialogo.

3. Fare clic su **connettersi manualmente a una rete wireless**, quindi fare clic su **Avanti**.

4. In **connettersi manualmente a una rete wireless**, in **nome di rete**, digitare il nome SSID del punto di accesso.  

5. In **tipo di protezione**, selezionare l'impostazione fornito dall'amministratore.

6. In **tipo di crittografia** e **chiave di sicurezza**selezionare o digitare le impostazioni fornite dall'amministratore.

7. Selezionare **Avvia questa connessione automaticamente**, quindi fare clic su **Avanti**.

8. In **aggiunto * * * il SSID di rete*, fare clic su **modificare le impostazioni di connessione**.

9. Fare clic su **modificare le impostazioni di connessione **. Il *il SSID di rete* verrà visualizzata la finestra di dialogo Proprietà rete Wireless.

10. Fare clic su di **sicurezza** scheda e quindi **scegliere un metodo di autenticazione di rete**, selezionare **PEAP \(PEAP\)**.

11. Fare clic su **impostazioni**. Il **PEAP \(PEAP\) proprietà** verrà visualizzata la pagina.

12. Nel **PEAP \(PEAP\) proprietà** pagina, assicurarsi che **Convalida certificato server** non è selezionata, fare clic su **OK** due volte, quindi fare clic su **Chiudi **.

13. Windows quindi tenta di connettersi alla rete wireless. Le impostazioni del profilo wireless bootstrap specificano che è necessario fornire le credenziali di dominio. Quando viene richiesto un nome account e password, digitare le credenziali dell'account di dominio come indicato di seguito: *nome di dominio dominio\\Nome*, *dominio Password *.

##### <a name="to-join-a-computer-to-the-domain"></a>Per aggiungere un computer al dominio

1. Accedere al computer con l'account amministratore locale.

2. Nella casella di testo di ricerca, digitare **PowerShell **. Nei risultati della ricerca, fare doppio clic su **Windows PowerShell**, quindi fare clic su **Esegui come amministratore **. Verrà visualizzata la finestra di Windows PowerShell con privilegi elevati.

3. In Windows PowerShell, digitare il comando seguente e quindi premere INVIO. Assicurarsi di sostituire i valori di DomainName variabile con il nome del dominio che si desidera aggiungere.
    
    Add-Computer DomainName
    
4. Quando richiesto, digitare il nome utente di dominio e la password e fare clic su **OK **.
5. Riavviare il computer.
6. Seguire le istruzioni nella sezione precedente [accesso al dominio utilizzando computer che eseguono Windows 10](#bkmk_w10).