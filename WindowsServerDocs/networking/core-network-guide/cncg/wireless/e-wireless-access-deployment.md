---
title: Distribuzione dell'accesso wireless
description: Questo argomento fa parte della Guida alla rete di Windows Server 2016 "Distribuisci con 802.1x basato su Password X Authenticated Wireless Access"
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4b66f517-b17d-408c-828f-a3793086bc1f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b2e237cee6eac6be809add37a2ac29fdf1c92118
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446489"
---
# <a name="wireless-access-deployment"></a>Distribuzione dell'accesso wireless

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Seguire questi passaggi per distribuire accesso senza fili:

- [Distribuire e configurare punti di accesso Wireless](#bkmk_aps)

- [Creare un gruppo di sicurezza utenti Wireless](#bkmk_groups)

- [Configurare reti senza fili \(IEEE 802.11\) criteri](#bkmk_policies)

- [Configurare NPSs](#bkmk_nps)

- [Aggiungere nuovi computer Wireless al dominio](#bkmk_domain)

## <a name="bkmk_aps"></a>Distribuire e configurare punti di accesso Wireless

Seguire questi passaggi per distribuire e configurare punti di accesso wireless:

- [Specificare le frequenze di canale AP senza fili](#bkmk_channel)

- [Configurare i punti di accesso Wireless](#bkmk_wirelessaps)

>[!NOTE]
>Le procedure illustrate in questa guida non includono istruzioni relative ai casi in cui la finestra di dialogo **Controllo dell'account utente** viene visualizzata per richiedere l'autorizzazione a continuare. Se si apre questa finestra di dialogo mentre si eseguono le procedure descritte in questa Guida, se è stata aperta la finestra di dialogo in risposta alle azioni dell'utente, fare clic su **Continue**.

### <a name="bkmk_channel"></a>Specificare le frequenze di canale AP senza fili

Quando si distribuiscono più punti di accesso wireless in un unico sito geografico, è necessario configurare l'accesso senza fili dotati di segnali sovrapposti usare delle frequenze di canale univoca per ridurre le interferenze tra punti di accesso wireless.

È possibile utilizzare le seguenti linee guida per agevolare la scelta delle frequenze di canale che non entrino in conflitto con altre reti wireless nella posizione geografica della rete senza fili.

- Se sono presenti altre organizzazioni con uffici in prossimità o nello stesso edificio dell'organizzazione, identificano se sono presenti reti senza fili alle organizzazioni di proprietà. Scopri la selezione host sia il canale assegnato frequenze delle loro AP senza fili, perché è necessario assegnare le frequenze di canale diversi per il punto di accesso e si desidera determinare il percorso migliore per installare il punto di accesso

- Identificare la sovrapposizione di segnali wireless piani adiacenti all'interno dell'organizzazione. Dopo che identifica la sovrapposizione di aree di copertura esterno e all'interno dell'organizzazione, assegnare le frequenze di canale per AP senza fili, verifica che due punti di accesso wireless con sovrapposizioni nella copertura vengono assegnate le frequenze di canale diversi.

### <a name="bkmk_wirelessaps"></a>Configurare i punti di accesso Wireless

Utilizzare le seguenti informazioni e la documentazione fornita dal produttore del punto di accesso wireless per configurare i punti di accesso wireless.

Questa procedura enumera gli elementi in genere configurati in un punto di accesso wireless. I nomi degli elementi può variare per marca e modello e potrebbe essere diversi da quelli elencati di seguito. Per informazioni dettagliate, vedere la documentazione di AP senza fili.

#### <a name="to-configure-your-wireless-aps"></a>Per configurare l'accesso senza fili  

- **SSID**. Specificare il nome della rete wireless\(s\) \(ad esempio, ExampleWLAN\). Si tratta del nome che verrà annunciato ai client senza fili.

- **Crittografia**. Specificare WPA2\-Enterprise \(preferito\) o WPA\-Enterprise ed entrambi AES \(preferito\) o algoritmo di crittografia TKIP, a seconda di quali versioni sono supportate per le schede di rete di computer client wireless.

- **Indirizzo IP AP wireless \(statico\)** . In ogni punto di accesso, configurare un indirizzo IP statico che rientra nell'intervallo di esclusione dell'ambito DHCP per la subnet. Usa un indirizzo che viene esclusa dall'assegnazione da DHCP impedisce che il server DHCP assegnando lo stesso indirizzo IP a un computer o un altro dispositivo.

- **La subnet mask**. Configurare questa impostazione in base alle impostazioni di subnet mask della rete LAN a cui si è connessi l'AP senza fili.  

- **Nome DNS**. Alcuni AP senza fili può essere configurato con un nome DNS. Il servizio DNS nella rete possa risolvere i nomi DNS a un indirizzo IP. In ogni punto di accesso wireless che supportano questa funzionalità, immettere un nome univoco per la risoluzione DNS.  

- **Servizio DHCP**. Se il punto di accesso wireless è integrato\-nel servizio DHCP, disabilitarla.  

- **Segreto condiviso RADIUS**. Utilizzare un segreto condiviso RADIUS univoco per ogni punto di accesso wireless a meno che non si prevede di configurare i punti di accesso come client RADIUS in Criteri di RETE dal gruppo. Se si prevede di configurare i punti di accesso dal gruppo in Criteri di RETE, il segreto condiviso deve essere lo stesso per ogni membro del gruppo. Inoltre, ogni segreto condiviso che è utilizzare deve essere una sequenza casuale di almeno 22 caratteri che combina lettere maiuscole e minuscole, numeri e segni di punteggiatura. Per assicurare la casualità, è possibile utilizzare un generatore casuale di caratteri, ad esempio il generatore casuale di caratteri disponibile in NPS **Configurazione 802.1 X** procedura guidata per creare i segreti condivisi.

>[!TIP]
>Registrare il segreto condiviso per ogni punto di accesso wireless e archiviarlo in un luogo sicuro, ad esempio un ufficio sicuro. Quando si configurano i client RADIUS in NPS, è necessario conoscere il segreto condiviso per ogni punto di accesso wireless.  

- **Indirizzo IP del server RADIUS**. Digitare l'indirizzo IP del server dei criteri di rete.

- **La porta UDP\(s\)** . Per impostazione predefinita dei criteri di rete Usa le porte UDP 1812 e 1645 per i messaggi di autenticazione e le porte UDP 1813 e 1646 per i messaggi di accounting. È consigliabile usare le stesse porte UDP nei punti di accesso, ma se si dispone di un valido motivo per usare porte diverse, assicurarsi che non solo configurare i punti di accesso con i nuovi numeri di porta ma anche riconfigurare tutti i NPSs da usare gli stessi numeri di porta come i punti di accesso. Se i punti di accesso e la NPSs non sono configurati con le stesse porte UDP, dei criteri di rete non può ricevere o elaborare le richieste di connessione dai punti di accesso e tutti i tentativi di connessione wireless nella rete avrà esito negativo.

- **Gli attributi VSA**. Alcuni punti di accesso wireless richiedono fornitore\-attributi specifici \(VSA\) per fornire funzionalità AP wireless completo. Gli attributi VSA vengono aggiunti in Criteri di rete NPS.

- **Filtri DHCP**. Configurare i punti di accesso wireless per bloccare i computer client wireless dall'invio di pacchetti IP da porta UDP 68 alla rete, come documentato dal produttore del punto di accesso wireless.

- **Il filtro dei DNS**. Configurare i punti di accesso wireless per bloccare i computer client wireless dall'invio di pacchetti IP dalla porta TCP o UDP 53 alla rete, come documentato dal produttore del punto di accesso wireless.

## <a name="create-security-groups-for-wireless-users"></a>Creare gruppi di sicurezza per gli utenti Wireless

Seguire questi passaggi per creare gruppi di sicurezza di uno o più utenti wireless e quindi aggiungere utenti al gruppo di sicurezza utenti wireless appropriati:

- [Creare un gruppo di sicurezza utenti Wireless](#bkmk_groups)

- [Aggiungere utenti al gruppo di sicurezza Wireless](#bkmk_addusers)

### <a name="bkmk_groups"></a>Creare un gruppo di sicurezza utenti Wireless

È possibile utilizzare questa procedura per creare un gruppo di protezione wireless nel computer Microsoft Management Console e di utenti di Active Directory \(MMC\) Blocca\-in.  

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.

#### <a name="to-create-a-wireless-users-security-group"></a>Per creare un gruppo di sicurezza utenti wireless

1. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Utenti e computer di Active Directory**. Nello snap di Active Directory Users and Computers\-verrà avviato. Se non è già selezionato, fare clic sul nodo corrispondente al proprio dominio. Ad esempio, se il dominio è example.com, fare clic su **example.com**.

2. Nel riquadro di destra\-fare clic sulla cartella in cui si desidera aggiungere un nuovo gruppo \(destro del mouse, ad esempio,\-fare clic su **utenti**\), scegliere **nuovo**, e quindi fare clic su **gruppo**.

3. In **nuovo oggetto-gruppo**, in **nome gruppo**, digitare il nome del nuovo gruppo. Ad esempio, digitare **gruppo rete senza fili**.

4. In **Ambito del gruppo** selezionare una delle opzioni seguenti:

    - **Dominio locale**

    - **Global**

    - **Universal**

5. In **tipo di gruppo**, selezionare **sicurezza**.

6. Fare clic su **OK**.

Se è necessario più di un gruppo di sicurezza per gli utenti wireless, ripetere questi passaggi per creare i gruppi di utenti senza fili aggiuntivi. In un secondo momento è possibile creare criteri di rete in Criteri di RETE per applicare diverse condizioni e Constraints a ogni gruppo, offrendo diverse autorizzazioni di accesso e le regole di connettività.

### <a name="bkmk_addusers"></a> Aggiungere utenti al gruppo di sicurezza utenti Wireless

È possibile utilizzare questa procedura per aggiungere un utente, computer o gruppo al gruppo di protezione wireless in un computer Microsoft Management Console e di utenti di Active Directory \(MMC\) Blocca\-in.

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per eseguire questa procedura.

#### <a name="to-add-users-to-the-wireless-security-group"></a>Per aggiungere utenti al gruppo di sicurezza wireless

1. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Utenti e computer di Active Directory**. Viene aperto lo snap-in MMC Utenti e computer di Active Directory. Se non è già selezionato, fare clic sul nodo corrispondente al proprio dominio. Ad esempio, se il dominio è example.com, fare clic su **example.com**.

2. Nel riquadro dei dettagli fare doppio\-fare clic sulla cartella che contiene il gruppo di sicurezza wireless.

3. Nel riquadro di destra\-fare clic sul gruppo di protezione wireless e quindi fare clic su **proprietà**. Il **proprietà** verrà visualizzata la finestra di dialogo per il gruppo di sicurezza.

4. Nel **membri** scheda, fare clic su **Aggiungi**, e quindi completare una delle seguenti procedure per aggiungere un computer o aggiungere un utente o gruppo.

##### <a name="to-add-a-user-or-group"></a>Per aggiungere un utente o gruppo

1. In **Immettere i nomi di oggetto da selezionare**, digitare il nome dell'utente o il gruppo che si desidera aggiungere e quindi fare clic su **OK**.

2. Per assegnare l'appartenenza al gruppo ad altri utenti o gruppi, ripetere il passaggio 1 di questa procedura.  

##### <a name="to-add-a-computer"></a>Per aggiungere un computer

1. Fare clic su **tipi di oggetto**. Il **tipi di oggetto** verrà visualizzata la finestra di dialogo.

2. In **tipi di oggetto**, selezionare **computer**, quindi fare clic su **OK**.

3. In **Immettere i nomi di oggetto da selezionare**, digitare il nome del computer in cui si desidera aggiungere e quindi fare clic su **OK**.

4. Per assegnare l'appartenenza al gruppo ad altri computer, ripetere i passaggi da 1\-3 di questa procedura.

## <a name="bkmk_policies"></a>Configurare reti senza fili \(IEEE 802.11\) criteri

Seguire questi passaggi per configurare la rete senza fili \(IEEE 802.11\) estensione criteri di gruppo:

- [Aprire o aggiungere e aprire un oggetto Criteri di gruppo](#bkmk_opengpme)

- [Attivare la rete senza fili predefinita \(IEEE 802.11\) criteri](#bkmk_activate)

- [Configurare il nuovo criterio di rete Wireless](#bkmk_policyconfig)

### <a name="bkmk_opengpme"></a>Aprire o aggiungere e aprire un oggetto Criteri di gruppo

Per impostazione predefinita, la funzionalità Gestione criteri di gruppo è installata nei computer che eseguono Windows Server 2016 quando servizi di dominio di Active Directory \(Active Directory Domain Services\) ruolo del server è installato e il server è configurato come controller di dominio. La procedura descritta di seguito viene descritto come aprire la Console Gestione criteri di gruppo \(GPMC\) sul controller di dominio. La procedura viene quindi descritto come aprire un dominio esistente\-oggetto Criteri di gruppo a livello di \(oggetto Criteri di gruppo\) per la modifica, o creare un nuovo dominio oggetto Criteri di gruppo e aprirlo e modificarlo.

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.

#### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Per aprire o aggiungere e aprire un oggetto Criteri di gruppo

1. Sul controller di dominio, fare clic su **avviare**, fare clic su **Strumenti di amministrazione di Windows**, quindi fare clic su **Gestione criteri di gruppo**. Verrà visualizzata la finestra di Console Gestione criteri di gruppo.  

2. Nel riquadro a sinistra, fare doppio\-fare clic su insieme di strutture. Ad esempio, fare doppio\-fare clic su **foresta: example.com**.  

3. Nel riquadro a sinistra, fare doppio\-fare clic su **domini**, quindi fare doppio\-fare clic sul dominio per il quale si desidera gestire un oggetto Criteri di gruppo. Ad esempio, fare doppio\-fare clic su **example.com**.  

4. Effettua una delle seguenti operazioni:

    -   **Per aprire un dominio esistente\-il livello di oggetto Criteri di gruppo per la modifica**, fare doppio clic sul dominio che contiene l'oggetto Criteri di gruppo che si desidera gestire, a destra\-fare clic sul criterio di dominio si desidera gestire, ad esempio il criterio dominio predefinito e quindi fare clic su **modificare**. **Editor Gestione criteri di gruppo** apre.

    -   **Per creare un nuovo oggetto Criteri di gruppo e aprire per la modifica**, a destra\-fare clic sul dominio per il quale si desidera creare un nuovo oggetto Criteri di gruppo e quindi fare clic su **Crea un oggetto Criteri di gruppo in questo dominio e collegarlo qui**.

        In **nuovo GPO**, in **nome**, digitare un nome per il nuovo oggetto Criteri di gruppo e quindi fare clic su **OK**.

        Destra\-fare clic su nuovo oggetto Criteri di gruppo e quindi fare clic su **modificare**. **Editor Gestione criteri di gruppo** apre.

Nella sezione successiva si utilizzerà Editor Gestione criteri di gruppo per creare criteri wireless.

### <a name="bkmk_activate"></a>Attivare la rete senza fili predefinita \(IEEE 802.11\) criteri

Questa procedura descrive come attivare l'impostazione predefinita della rete Wireless \(IEEE 802.11\) i criteri usando l'Editor Gestione criteri di gruppo \(editor\).

>[!NOTE]
>Dopo aver attivato il **Windows Vista e versioni successive** versione della rete senza fili \(IEEE 802.11\) criteri o la **Windows XP** versione, l'opzione di versione è automaticamente rimosso dall'elenco delle opzioni quando destra\-fare clic su **rete senza fili \(IEEE 802.11\) criteri**. Ciò si verifica perché dopo aver selezionato una versione di criteri, il criterio viene aggiunto nel riquadro dei dettagli dell'EDITOR quando si seleziona il **rete senza fili \(IEEE 802.11\) criteri** nodo. Questo stato resta a meno che non si elimina il criterio wireless, momento in cui la versione del criterio wireless restituisce a destra\-fare clic sul menu **rete senza fili \(IEEE 802.11\) criteri** Nell'Editor. Inoltre, i criteri wireless sono visualizzati solo nel riquadro dei dettagli di EDITOR quando il **rete senza fili \(IEEE 802.11\) criteri** nodo selezionato.

Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente.

#### <a name="to-activate-default-wireless-network-ieee-80211-policies"></a>Per attivare l'impostazione predefinita della rete Wireless \(IEEE 802.11\) criteri  

1. Utilizzare la procedura precedente, **per aprire o aggiungere e aprire un oggetto Criteri di gruppo** per aprire l'EDITOR.

2. Nell'Editor, nel riquadro a sinistra, fare doppio\-fare clic su **Configurazione Computer**, double\-fare clic su **criteri**, double\-fare clic su **le impostazioni di Windows**, quindi fare doppio\-fare clic su **le impostazioni di sicurezza**.

![Criteri di gruppo di Wireless 802.1 X](../../../media/Wireless-GP/Wireless-GP.jpg)

3. In **le impostazioni di sicurezza**, a destra\-fare clic su **rete senza fili \(IEEE 802.11\) criteri**, quindi fare clic su **creare un nuovo criteri Wireless per Windows Vista e versioni successive**. 

![802.1 x Wireless criteri](../../../media/Wireless-Policy/Wireless-Policy.jpg)

4. Il **nuove proprietà di criteri di rete senza fili** verrà visualizzata la finestra di dialogo. In **Nome criterio**, digitare un nuovo nome per il criterio o mantenere il nome predefinito. Fare clic su **OK** per salvare il criterio. Il criterio viene attivato ed elencato nel riquadro dei dettagli di EDITOR con il nuovo nome fornito o con il nome predefinito **nuovi criteri di rete Wireless**.

![Nuove proprietà di criteri di rete Wireless](../../../media/Wireless-Policy-Properties/Wireless-Policy-Properties.jpg)

5. Nel riquadro dei dettagli fare doppio\-fare clic su **nuovi criteri di rete senza fili** per aprirlo.

Nella sezione successiva è possibile eseguire configurazione dei criteri di ordine di preferenza l'elaborazione dei criteri e autorizzazioni di rete.

### <a name="bkmk_policyconfig"></a>Configurare il nuovo criterio di rete Wireless

È possibile utilizzare le procedure descritte in questa sezione per configurare la rete senza fili \(IEEE 802.11\) criteri. Questo criterio consente di configurare le impostazioni di sicurezza e autenticazione, gestire i profili wireless e specificare le autorizzazioni per le reti wireless che non sono configurate come reti preferite.

- [Configurare un profilo di connessione Wireless per PEAP\-MS\-CHAP v2](#bkmk_configureprofile)  

- [Impostare l'ordine di preferenza per i profili di connessione Wireless](#bkmk_preferenceorder)  

- [Definire le autorizzazioni di rete](#bkmk_permissions)  

#### <a name="bkmk_configureprofile"></a>Configurare un profilo di connessione Wireless per PEAP\-MS\-CHAP v2

Questa procedura include i passaggi necessari per configurare un PEAP\-MS\-CHAP v2 wireless profilo.  

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

##### <a name="to-configure-a-wireless-connection-profile-for-peap-ms-chap-v2"></a>Per configurare un profilo di connessione wireless per PEAP\-MS\-CHAP v2

1. Nell'EDITOR, nella finestra di dialogo Proprietà rete wireless per il criterio appena creato, scegliere il **Generale** scheda e nella **Descrizione**, digitare una breve descrizione per il criterio.

2. Per specificare che il servizio configurazione automatica WLAN viene utilizzato per configurare le impostazioni della scheda di rete wireless, assicurarsi che **servizio configurazione automatica WLAN di Windows utilizza per i client** è selezionata.  

3. In **Connetti alle reti disponibili nell'ordine dei profili elencati di seguito**, fare clic su **Aggiungi**, quindi selezionare **infrastruttura**. Il **le proprietà del nuovo profilo** verrà visualizzata la finestra di dialogo.

4. Nel**le proprietà del nuovo profilo** della finestra di dialogo di **connessione** nella scheda il **nome profilo** digitare un nuovo nome per il profilo. Ad esempio, digitare **Example.com WLAN profilo per Windows 10**.

5. Nelle **nome di rete\(s\) \(SSID\)** , digitare l'identificatore SSID corrispondente al SSID configurato nei punti di accesso wireless e quindi fare clic su **Aggiungi**.

    Se nella distribuzione si usano più SSID e ogni punto di accesso wireless usa le stesse impostazioni di sicurezza wireless, ripetere questo passaggio per aggiungere l'SSID per ogni punto di accesso wireless al quale si vuole applicare questo profilo.

    Se nella distribuzione si usano più SSID e le impostazioni di sicurezza per ogni SSID non corrispondono, configurare un profilo distinto per ogni gruppo di SSID che usa le stesse impostazioni di sicurezza. Ad esempio, se si dispone di un gruppo di punti di accesso wireless configurati per utilizzare WPA2\-Enterprise e AES e un altro gruppo di punti di accesso wireless a utilizzare WPA\-Enterprise e TKIP, configurare un profilo per ogni gruppo di punti di accesso wireless.

6. Se il testo predefinito **NEWSSID** è presente, selezionarlo e quindi fare clic su **rimuovere**.

7. Se è stato distribuito access point wireless che sono configurati per l'esclusione del beacon di trasmissione, selezionare **connettersi anche se la rete non sta trasmettendo**.

    > [!NOTE]
    > L'abilitazione di questa opzione, è possibile creare un rischio per la sicurezza perché i computer client wireless effettueranno la ricerca e tentativi di connessione a qualsiasi rete wireless. Questa impostazione non è abilitata per impostazione predefinita.  

8. Fare clic sulla scheda **Sicurezza** , su **Avanzate**e quindi configurare quanto segue:

    1. Per configurare le impostazioni avanzate di 802.1X, in **IEEE 802.1X** selezionare **Applica impostazioni avanzate 802.1X**.

        Quando l'avanzate per 802.1 X impostazioni vengono applicate, i valori predefiniti per **Max Eapol\-avviare accodati**, **periodo di sospensione**, **periodo di avvio**, e **periodo di autenticazione** sono sufficienti per le distribuzioni wireless tipiche. Per questo motivo, non è necessario modificare le impostazioni predefinite a meno che non esista un motivo specifico per questa operazione.

    2. Per abilitare Single Sign On, selezionare **Attiva Single Sign-On per la rete**.

    3. I valori predefiniti rimanenti **Single Sign-On** sono sufficienti per le distribuzioni wireless tipiche.

    4. In **Roaming veloce**, se il punto di accesso wireless è configurato per pre-\-l'autenticazione, selezionare **la rete utilizza pre\-autenticazione**.

9. Per specificare che le comunicazioni wireless soddisfino FIPS 140\-2 standard, selezionare **eseguire la crittografia FIPS 140\-2 modalità certificata**.

10. Fare clic su **OK** per ritornare alla scheda **Sicurezza**. In Selezionare i metodi di sicurezza per la rete, in autenticazione**, selezionare **WPA2\-Enterprise** se è supportato il punto di accesso wireless e le schede di rete client wireless. In caso contrario, selezionare **WPA\-Enterprise**.

11. In **crittografia**, se supportata dal client senza fili le schede di rete, selezionare AP senza fili **AES-CCMP**. Se si utilizza punti di accesso e schede di rete wireless che supportano 802.11 AC, selezionare **AES-GCMP**. In caso contrario, selezionare **TKIP**.

    > [!NOTE]  
    > Le impostazioni delle opzioni **autenticazione** e **crittografia** deve corrispondere alle impostazioni configurate in punti di accesso wireless. Le impostazioni predefinite per **modalità di autenticazione**, **numero massimo di errori di autenticazione**, e **memorizza informazioni utente per successive connessioni a questa rete** sono sufficienti per le distribuzioni wireless tipiche.  

12. In **Selezionare un metodo di autenticazione di rete**, selezionare **PEAP \(PEAP\)** , quindi fare clic su **proprietà**. Il **Proprietà PEAP** verrà visualizzata la finestra di dialogo.

13. In **Proprietà PEAP**, verificare che **per verificare l'identità del server, la convalida del certificato** è selezionata.

14. Nelle **autorità di certificazione radice attendibili**, selezionare l'autorità di certificazione radice attendibile \(autorità di certificazione\) che ha rilasciato il certificato del server per i criteri di rete.

    > [!NOTE]  
    > Questa impostazione limita le CA radice che i client considerano attendibili a quelle selezionate. Se non sono selezionate Nessuna CA radice attendibili, i client considereranno attendibili che tutte le CA radice elencate nell'archivio dei certificati di autorità di certificazione radice attendibili.  

15. Nel **Selezione metodo di autenticazione** selezionare **password protetta \(EAP\-MS\-CHAP v2\)** .

16. Fare clic su **configurare**. Nel **proprietà EAP MSCHAPv2** finestra di dialogo verificare **utilizza automaticamente il nome di accesso di Windows e la password \(e dominio se qualsiasi\)** sia selezionata e fare clic su **OK**.

17. Per abilitare la riconnessione rapida PEAP, assicurarsi che **Abilita riconnessione rapida** è selezionata.

18. Per richiedere server TLV di Cryptobinding in tentativi di connessione, selezionare **Disconnetti se il server non presenta TLV di Cryptobinding**.

19. Per specificare che l'identità dell'utente viene mascherato in fase di autenticazione, selezionare **Consenti Privacy identità**, e nella casella di testo, digitare un nome di identità anonima, o lasciare vuota la casella di testo.

    > [! NOTE SULLA]
    > - Utilizzando criteri di RETE è necessario creare un criterio di NPS per 802.1 X Wireless **criterio richiesta di connessione**. Se viene creato un criterio di NPS utilizzando criteri di RETE **criteri di rete**, Consenti privacy identità non funzionerà.
    > - Consenti privacy identità EAP è fornita da alcuni metodi EAP in un'identità anonima o vuota \(differente da quella effettiva\) viene inviato in risposta alla richiesta di identità EAP. PEAP invia l'identità due volte durante l'autenticazione. Nella prima fase, l'identità viene inviata in testo normale e questa identità viene usata a scopo di routing, non per l'autenticazione client. L'identità reale, usato per l'autenticazione, viene inviato durante la seconda fase dell'autenticazione, all'interno del tunnel sicuro che viene stabilito nella prima fase. Se **Consenti Privacy identità** casella di controllo è selezionata, il nome utente viene sostituito con la voce specificata nella casella di testo. Si supponga, ad esempio, **Consenti Privacy identità** è selezionata e l'alias di privacy identità **anonimo** specificato nella casella di testo. Per un utente con un alias di identità reale <strong>jdoe@example.com</strong>, l'identità inviato nella prima fase di autenticazione verrà modificato <strong>anonymous@example.com</strong>. La parte dell'area di autenticazione dell'identità del 1 ° fase non viene modificata perché è usato a scopo di routing.  

20. Fare clic su **OK** per chiudere la **Proprietà PEAP** la finestra di dialogo.
21. Fare clic su **OK** per chiudere la **sicurezza** scheda.
22. Se si desidera creare altri profili, fare clic su **Aggiungi**, quindi ripetere i passaggi precedenti, rendendo diverse opzioni per personalizzare ogni profilo per il computer client wireless e la rete a cui si desidera che il profilo applicato. Una volta aggiunta profili, fare clic su **OK** per chiudere la finestra di dialogo Proprietà criterio rete senza fili.

Nella sezione successiva è possibile ordinare i profili di criteri di protezione ottimale.

#### <a name="bkmk_preferenceorder"></a>Impostare l'ordine di preferenza per i profili di connessione Wireless
È possibile utilizzare questa procedura se è stato creato più profili wireless in Criteri di rete wireless e si desidera ordinare i profili per la sicurezza e l'efficacia ottimale.

Per garantire che i client wireless si connettono con il massimo livello di sicurezza che possono supportare, inserire i criteri più restrittivi nella parte superiore dell'elenco.

Ad esempio, se si dispone di due profili, uno per i client che supportano WPA2 e uno per i client che supportano WPA, inserire il profilo di WPA2 superiore nell'elenco. Ciò garantisce che i client che supportano WPA2 utilizzerà tale metodo per la connessione anziché WPA meno sicuro.

Questa procedura include i passaggi per specificare l'ordine in cui vengono usati i profili di connessione wireless per la connessione client wireless membri del dominio alle reti wireless.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

##### <a name="to-set-the-preference-order-for-wireless-connection-profiles"></a>Per impostare l'ordine di preferenza per i profili di connessione wireless

1. Nell'EDITOR, nella finestra di dialogo Proprietà rete wireless per i criteri che appena configurato, fare clic su di **Generale** scheda.

2. Nel **Generale** scheda **Connetti alle reti disponibili nell'ordine dei profili elencati di seguito**, selezionare il profilo che si desidera spostare nell'elenco e quindi fare clic su uno di "freccia" pulsante freccia su o "giù" pulsante per spostare il profilo nella posizione desiderata nell'elenco.

3.  Ripetere il passaggio 2 per ogni profilo che si desidera spostare nell'elenco.  

4.  Fare clic su **OK** salvare tutte le modifiche.

Nella sezione seguente, è possibile definire le autorizzazioni di rete per i criteri wireless.

#### <a name="bkmk_permissions"></a>Definire le autorizzazioni di rete
È possibile configurare le impostazioni nel **autorizzazioni di rete** scheda per membri del dominio di rete senza fili \(IEEE 802.11\) criteri vengono applicati.

È possibile applicare solo le seguenti impostazioni per reti wireless che non sono configurate sul **Generale** nella scheda il **Proprietà criterio rete senza fili** pagina:

- Consentire o negare le connessioni a reti senza fili specifiche specificato dal tipo di rete e Service Set Identifier \(SSID\)

- Consentire o negare le connessioni a reti ad hoc

- Consentire o negare le connessioni alle reti infrastruttura

- Consentire o negare agli utenti di visualizzare tipi di rete \(ad hoc o infrastruttura\) a cui viene negati

- Consentire o negare agli utenti di creare un profilo che si applica a tutti gli utenti

- Gli utenti possono connettersi solo a reti consentite usando i profili di criteri di gruppo

L'appartenenza a **Domain Admins**, o equivalente è il requisito minimo necessario per completare queste procedure.

##### <a name="to-allow-or-deny-connections-to-specific-wireless-networks"></a>Per consentire o negare le connessioni a reti senza fili specifiche

1. Nell'EDITOR, nella finestra di dialogo Proprietà rete wireless, fare clic sui **Autorizzazioni reti** scheda.

2. Nel **Autorizzazioni reti** scheda, fare clic su **Aggiungi**. Il **nuova voce autorizzazione** verrà visualizzata la finestra di dialogo.

3. Nel **nuova voce autorizzazione** della finestra di dialogo di **nome di rete \(SSID\)** campo, digitare la rete SSID della rete per il quale si desidera definire le autorizzazioni.

4.  In **tipo di rete**, selezionare **infrastruttura** o **Ad hoc**.

    > [!NOTE]  
    > Se non si è certi se la rete broadcast è un'infrastruttura o una rete ad hoc, è possibile configurare due voci di autorizzazione rete, uno per ogni tipo di rete.

5. In **autorizzazione**, selezionare **Consenti** o **Deny**.

6. Fare clic su **OK**, per ripristinare il **autorizzazioni di rete** scheda.

##### <a name="to-specify-additional-network-permissions-optional"></a>Per specificare le autorizzazioni di rete aggiuntiva \(facoltativo\)

1.  Nel **autorizzazioni di rete** configurare una o tutte le operazioni seguenti:  

    -   Per negare l'accesso a reti ad hoc i membri del dominio, selezionare **impedire le connessioni ad\-reti ad hoc**.

    -   Per negare l'accesso alle reti infrastruttura i membri del dominio, selezionare **Impedisci connessioni a reti infrastruttura**.  

    -   Per consentire ai membri del dominio visualizzare i tipi di rete \(ad hoc o infrastruttura\) a che non hanno accesso, selezionare **consentire all'utente di visualizzare le reti negate**.

    -   Per consentire agli utenti di creare i profili che si applicano a tutti gli utenti, selezionare **Consenti la creazione di tutti i profili utente**.

    -   Per specificare che gli utenti possono connettersi solo a reti consentite usando i profili di criteri di gruppo, selezionare **utilizza solo profili di criteri di gruppo per le reti consentite**.

## <a name="bkmk_nps"></a>Configurare il NPSs
Seguire questi passaggi per configurare NPSs per eseguire l'autenticazione 802.1x per l'accesso wireless:

- [Registrare NPS in Active Directory Domain Services](#bkmk_npsreg)

- [Configurare un punto di accesso Wireless come Client RADIUS NPS](#bkmk_radiusclient)

- [Creare i criteri NPS per 802.1 X Wireless utilizzando una procedura guidata](#bkmk_npspolicy)

### <a name="bkmk_npsreg"></a>Registrare NPS in Active Directory Domain Services
È possibile usare questa procedura per registrare un server che esegue Server dei criteri di rete \(NPS\) in servizi di dominio Active Directory \(Active Directory Domain Services\) nel dominio in cui i criteri di rete è un membro. Per concedere l'autorizzazione per leggere il quadrante NPSs\-nelle proprietà degli account utente durante il processo di autorizzazione, ogni NPS deve essere registrato in Active Directory Domain Services. La registrazione di un NPS aggiunge il server per il **server RAS e IAS** gruppo di sicurezza in Active Directory Domain Services.

>[!NOTE]
>È possibile installare NPS in un controller di dominio o in un server dedicato. Eseguire il comando Windows PowerShell seguente per installare il server NPS se non si è ancora fatto:
    
    Install-WindowsFeature NPAS -IncludeManagementTools
    
Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

#### <a name="to-register-an-nps-in-its-default-domain"></a>Per registrare un criteri di rete nel dominio predefinito

1. Nei criteri di rete, in **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Blocca NPS\-verrà avviato.

2. Destra\-fare clic su **NPS \(locale\)** , quindi fare clic su **Registra Server in Active Directory**. Il **Server dei criteri di rete** verrà visualizzata la finestra di dialogo.

3. In **Server dei criteri di rete**, fare clic su **OK**, quindi fare clic su **OK** nuovamente.

### <a name="bkmk_radiusclient"></a>Configurare un punto di accesso Wireless come Client RADIUS NPS
È possibile utilizzare questa procedura per configurare un punto di accesso, noto anche come un *server di accesso di rete \(NAS\)* , come un Remote Authentication Dial\-In User Service \(RADIUS\) client utilizzando lo snap NPS\-in. 

>[!IMPORTANT]
>I computer client, ad esempio computer portatili wireless e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless 802.1x\-commutatori che supportano, rete privata virtuale \(VPN\) server e connessione\-backup server, perché usano il protocollo RADIUS per comunicare con server RADIUS, ad esempio NPSs.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

#### <a name="to-add-a-network-access-server-as-a-radius-client-in-nps"></a>Per aggiungere un server di accesso di rete come client RADIUS in Criteri di rete

1. Nei criteri di rete, in **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Blocca NPS\-verrà avviato.

2. In NPS snapshot\-doppio\-fare clic su **client e server RADIUS**. Destra\-fare clic su **client RADIUS**, quindi fare clic su **nuovo**.

3. In **Nuovo Client RADIUS**, verificare che il **attiva questo client RADIUS** casella di controllo è selezionata.

4. In **Nuovo Client RADIUS**, in **nome descrittivo**, digitare un nome per il punto di accesso wireless.

    Ad esempio, se si desidera aggiungere un punto di accesso wireless \(AP\) denominato AP\-01, tipo **AP\-01**.

5. In **indirizzo \(IP o DNS\)** , digitare l'indirizzo IP o nome di dominio completo \(FQDN\) per il server NAS.

    Se si immette il nome FQDN, per verificare che il nome sia corretto e che esegue il mapping a un indirizzo IP valido, fare clic su **verificare**, quindi nella **indirizzo verificare**, nel **indirizzo** campo, fare clic su **risolvere**. Se il nome FQDN è mappato a un indirizzo IP valido, l'indirizzo IP di tale NAS compariranno automaticamente **indirizzo IP**. Se il nome di dominio completo non viene risolto in un indirizzo IP, riceverai un messaggio che indica che l'host è sconosciuto. In questo caso, verificare di avere il nome di punto di accesso corretto e che il punto di accesso sia acceso e connesso alla rete.  

    Fare clic su **OK** per chiudere **verificare indirizzo**.  

6. In **Nuovo Client RADIUS**, in **segreto condiviso**, effettuare una delle operazioni seguenti:  

    -   Per configurare manualmente un segreto condiviso RADIUS, selezionare **manuale**, quindi nella **segreto condiviso**, digitare la password complessa che viene inserita anche sul server NAS. Digitare nuovamente il segreto condiviso in **Conferma segreto condiviso**.  

    -   Per generare automaticamente un segreto condiviso, selezionare il **Genera** casella di controllo e quindi scegliere il **Genera** pulsante. Salvare il segreto condiviso generato e quindi usare tale valore per configurare il server NAS in modo che possa comunicare con i criteri di rete.  

        >[!IMPORTANT]
        >Il segreto condiviso RADIUS immesse per il proprio AP virtuale in Criteri di rete deve corrispondere esattamente il segreto condiviso RADIUS configurato per l'effettivo AP senza fili Se si usa l'opzione dei criteri di rete per generare un segreto condiviso RADIUS, è necessario configurare l'AP senza fili effettiva corrispondenza con il segreto condiviso RADIUS che è stato generato da criteri di rete.

7. In **Nuovo Client RADIUS**, via il **Avanzate** scheda **il nome del fornitore**, specificare il nome del produttore NAS. Se non si è certi del nome del produttore NAS, selezionare **standard RADIUS**.

8. In **Opzioni aggiuntive**, se utilizza qualsiasi metodo di autenticazione EAP e PEAP e, se il server NAS supporta l'utilizzo dell'attributo autenticatore messaggio, selezionare **messaggi di richiesta di accesso devono contenere il messaggio\-attributi di autenticatore**.

9. Fare clic su **OK**. Il server NAS viene visualizzato nell'elenco dei computer client RADIUS configurato su NPS.

### <a name="bkmk_npspolicy"></a>Creare i criteri NPS per 802.1 X Wireless utilizzando una procedura guidata
È possibile utilizzare questa procedura per creare criteri di richiesta di connessione e di rete criteri necessari per distribuire entrambi 802.1 X\-punti di accesso wireless in grado di Remote Authentication Dial\-In User Service \(RADIUS\) client al server RADIUS che esegue Server dei criteri di rete \(NPS\).  
Dopo aver eseguito la procedura guidata, vengono creati i criteri seguenti:

- Criterio di richiesta di una connessione

- Un criterio di rete

>[!NOTE]
>È possibile eseguire la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X ogni volta che è necessario creare nuovi criteri per l'accesso autenticato con 802.1x.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

#### <a name="create-policies-for-8021x-authenticated-wireless-by-using-a-wizard"></a>Creare criteri per 802.1 X wireless autenticato tramite una procedura guidata

1. Blocca NPS aprire\-in. Se non è già selezionata, fare clic su **NPS \(locale\)** . Se si esegue lo snap-NPS MMC\-in e si desidera creare criteri in un server NPS remoto, selezionare il server.

2. In **Introduzione**, in **configurazione Standard**, selezionare **server RADIUS per connessioni cablate o Wireless 802.1 X**. Il testo e i collegamenti riportati di seguito le modifiche del testo in modo da riflettere la selezione.

3. Fare clic su **Configurazione 802.1 X**. Verrà visualizzata la procedura guidata configurazione 802.1 X.

4.  Nel **Selezionare tipo di connessioni 802.1 X** pagina procedura guidata, in **tipo connessioni 802.1 X**, selezionare **connessioni Wireless protette**, e in **nome**, digitare un nome per il criterio o lasciare il nome predefinito **connessioni Wireless protette**. Fare clic su **Avanti**.

5.  Nel **specificare commutatori 802.1 X** pagina procedura guidata, in **client RADIUS**, tutti 802.1 X commutatori e i punti di accesso wireless che è stato aggiunto come client RADIUS nello snap di NPS\-vengono visualizzati. Effettuare una delle operazioni riportate di seguito.

    -   Per aggiungere il server di accesso di rete aggiuntiva \(NAS\), ad esempio punti di accesso wireless, in **client RADIUS**, fare clic su **Aggiungi**, quindi nel **nuovo client RADIUS**, immettere le informazioni per: **Nome descrittivo**, **indirizzi \(IP o DNS\)** , e **segreto condiviso**.

    -   Per modificare le impostazioni per qualsiasi NAS **client RADIUS**, selezionare il punto di accesso per il quale si desidera modificare le impostazioni e quindi fare clic su **modificare**. Modificare le impostazioni in base alle esigenze.

    -   Per rimuovere un server NAS dall'elenco, **client RADIUS**, selezionare il server NAS e quindi fare clic su **rimuovere**.

        >[!WARNING]
        >Rimozione di un client RADIUS all'interno di **configurazione 802.1 X** procedura guidata consente di eliminare il client dalla configurazione dei criteri di rete. Tutte le aggiunte, modifiche ed eliminazioni apportate all'interno di **configurazione 802.1 X** procedura guidata per i client RADIUS vengono riflesse in NPS snap\-in, nel **client RADIUS** nodo  **NPS** \/ **client e server RADIUS**. Ad esempio, se si utilizza la procedura guidata per rimuovere un switch 802.1 X, l'opzione viene rimossa anche da NPS snap\-in.

6. Fare clic su **Avanti**. Nel **configurare un metodo di autenticazione** pagina procedura guidata, in **tipo \(basato sul metodo di configurazione di rete e accesso\)** , selezionare **Microsoft: Protected EAP \(PEAP\)** , quindi fare clic su **configura**.

    >[!TIP]
    >Se si riceve un messaggio di errore che indica che non viene trovato un certificato per l'uso con il metodo di autenticazione e aver configurato i servizi certificati di Active Directory per l'emissione automatica i certificati per server RAS e IAS sulla rete, prima di tutto Assicurarsi di aver seguito i passaggi per registrare NPS in Active Directory Domain Services, quindi utilizzare la procedura seguente per aggiornare criteri di gruppo: Fare clic su **avviare**, fare clic su **Windows System**, fare clic su **eseguire**e nella **Open**, tipo **gpupdate**e quindi premere INVIO. Quando il comando restituisce i risultati che indicano che sia utente e computer dei criteri di gruppo sono aggiornati correttamente, selezionare **Microsoft: Protected EAP \(PEAP\)**  nuovamente, quindi fare clic su **configura**.
    >
    >Se dopo l'aggiornamento dei criteri di gruppo si continuerà a ricevere il messaggio di errore che indica che un certificato non è stato trovato per l'uso con il metodo di autenticazione, il certificato non viene visualizzato perché non soddisfa il certificato server minimo requisiti come documentato nella Guida complementare alla rete Core: [Distribuire i certificati Server per 802.1 X cablate e Wireless distribuzioni](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments). In questo caso, è necessario interrompere la configurazione dei criteri di rete, revocare il certificato emesso per i criteri di rete\(s\)e quindi seguire le istruzioni per configurare un nuovo certificato usando la Guida alla distribuzione di certificati del server.

7.  Nel **Modifica proprietà PEAP** nella pagina procedura guidata **certificato emesso**, assicurarsi che il certificato corretto dei criteri di rete sia selezionato e quindi eseguire le operazioni seguenti:

    >[!NOTE]
    >Verificare che il valore in **dell'autorità di certificazione** corretto per il certificato selezionato **certificato emesso**. Ad esempio, l'autorità emittente previsto per un certificato emesso da un'autorità di certificazione esegue Servizi certificati Active Directory \(Servizi certificati Active Directory\) denominato corp\DC1, nel dominio contoso.com, viene **corp\-DC1\-CA**.

    -   Per consentire agli utenti di effettuare il roaming con i computer wireless tra punti di accesso senza richiedere di ripetere l'autenticazione ogni volta che vengono associati a un nuovo punto di accesso, selezionare **Abilita riconnessione rapida**.

    -   Per specificare che la connessione client senza fili terminerà il processo di autenticazione di rete se il server RADIUS non presenta TLV tipo\-lunghezza\-valore \(TLV\), selezionare **disconnettere client senza Cryptobinding**.  

    -   Per modificare i criteri digitare le impostazioni per il protocollo EAP, in **tipi EAP**, fare clic su **modificare**, in **proprietà EAP MSCHAPv2**, modificare le impostazioni in base alle esigenze e quindi fare clic su **OK**.  

8.  Fare clic su **OK**. Chiude la finestra di dialogo Modifica proprietà PEAP, la restituzione di **Configurazione 802.1 X** procedura guidata. Fare clic su **Avanti**.

9. In **specificare gruppi di utenti**, fare clic su **Aggiungi**, quindi digitare il nome del gruppo di sicurezza che è stato configurato per i client wireless in Active Directory Users e computer blocca\-in. Ad esempio, se il gruppo di protezione wireless gruppo rete senza fili denominata, digitare **gruppo rete senza fili**. Fare clic su **Avanti**.

10. Fare clic su **Configura** per configurare gli attributi RADIUS standard e il fornitore\-attributi specifici per LAN virtuale \(VLAN\) in base alle esigenze e come specificato per la documentazione fornita dal produttore dell'hardware AP wireless. Fare clic su **Avanti**.

11. Esaminare i dettagli di riepilogo di configurazione e quindi fare clic su **Fine**.

Ora vengono creati i criteri di criteri di RETE e sarà possibile passare per aggiungere computer al dominio.

## <a name="bkmk_domain"></a>Aggiungere nuovi computer Wireless al dominio
Il metodo più semplice per aggiungere nuovi computer wireless al dominio consiste nel collegare fisicamente il computer a un segmento di LAN cablata \(un segmento non controllato da un switch 802.1 X\) prima di aggiungere il computer al dominio. Questo è più semplice perché le impostazioni dei criteri di gruppo wireless vengono applicate immediatamente e automaticamente e, se è stato distribuito un PKI specifico, il computer riceve il certificato della CA e lo inserisce nell'archivio certificati Autorità di certificazione radice attendibili, consentendo al client senza fili considerare attendibile NPSs con i certificati server rilasciati dall'autorità di certificazione.

Analogamente, dopo che viene aggiunto un nuovo computer wireless al dominio, il metodo preferito per gli utenti accedere al dominio è eseguire log in usando una connessione cablata per la rete.

### <a name="other-domain-join-methods"></a>Altri metodi di aggiunta al dominio
Nei casi in cui non è pratico aggiungere computer al dominio tramite una connessione Ethernet cablata o nei casi in cui l'utente non può accedere al dominio per la prima volta tramite una connessione cablata, è necessario utilizzare un metodo alternativo.

- **Configurazione del Computer del personale IT**. Un membro del personale IT aggiunge un computer wireless al dominio e configura un profilo Single Sign On bootstrap wireless. Con questo metodo, l'amministratore IT si connette il computer wireless alla rete Ethernet cablata e aggiunge il computer al dominio. L'amministratore distribuisce quindi il computer per l'utente. Quando l'avvio di utente nel computer senza usare una connessione cablata, le credenziali di dominio che si specificano manualmente per l'accesso dell'utente vengono utilizzati per entrambi stabilire una connessione alla rete wireless e per accedere al dominio.

Per ulteriori informazioni, vedere la sezione [raggiungere il dominio e un accesso tramite il metodo di configurazione del Computer del personale IT](#bkmk_itstaff)

-   **Eseguire l'avvio di configurazione del profilo Wireless da parte degli utenti**. L'utente configura il computer senza fili con un profilo wireless bootstrap manualmente e aggiunto al dominio, in base alle istruzioni acquisiti da un amministratore IT. Il profilo wireless bootstrap consente all'utente di stabilire una connessione wireless e quindi aggiunta al dominio. Dopo l'aggiunta del computer al dominio e il riavvio del computer, l'utente può accedere al dominio tramite una connessione wireless e le credenziali di account di dominio.

Per ulteriori informazioni, vedere la sezione [raggiungere il dominio e un accesso tramite configurazione del profilo Wireless Bootstrap dagli utenti](#bkmk_userbootstrap).

### <a name="bkmk_itstaff"></a>Aggiungere il dominio e Log usando il metodo di configurazione di Computer personale IT
Gli utenti membri di dominio con dominio\-computer client wireless collegati possono utilizzare un profilo wireless temporaneo per la connessione a un 802.1 X\-autenticato rete senza fili senza prima una connessione alla rete LAN. Questo profilo wireless temporaneo viene chiamato un *bootstrap profilo wireless*.

Un profilo wireless bootstrap richiede all'utente di specificare manualmente le credenziali di account utente di dominio e non convalida il certificato di Remote Authentication Dial\-In User Service \(RADIUS\) server che esegue Server dei criteri di rete \(NPS\).

Dopo aver stabilita la connettività wireless, criteri di gruppo viene applicato nel computer client wireless e viene generato automaticamente un nuovo profilo wireless. Il nuovo criterio utilizza le credenziali dell'account utente e computer per l'autenticazione client. 

Inoltre, come parte di PEAP\-MS\-CHAP v2 l'autenticazione reciproca utilizzando il nuovo profilo anziché il profilo di bootstrap, il client convalida le credenziali del server RADIUS.

Dopo avere aggiunto il computer al dominio, è possibile utilizzare questa procedura per configurare un profilo Single Sign On bootstrap wireless, prima di distribuire il computer al dominio senza fili\-utente membro.

#### <a name="to-configure-a-single-sign-on-bootstrap-wireless-profile"></a>Per configurare un profilo Single Sign On bootstrap wireless

1. Creare un profilo di bootstrap utilizzando la procedura descritta in questa guida denominata [configurare un profilo di connessione Wireless per PEAP\-MS\-CHAP v2](#bkmk_configureprofile), e utilizzare le seguenti impostazioni:

    - PEAP\-MS\-autenticazione CHAP v2

    - Convalidare disabilitato di certificato server RADIUS

    - Single Sign On abilitato

2. Nelle proprietà del criterio di rete Wireless in cui è creato il nuovo profilo di bootstrap, nel **Generale** scheda, selezionare il profilo di bootstrap e quindi fare clic su **esportare** per esportare il profilo in una condivisione di rete, unità flash USB, o altra posizione facilmente accessibile. Il profilo viene salvato come file XML nel percorso specificato.

3. Aggiungere il nuovo computer wireless al dominio \(tramite una connessione Ethernet che non richiede IEEE 802.1 X, ad esempio l'autenticazione\) e aggiungere il profilo wireless bootstrap al computer utilizzando il **netsh wlan aggiungere profilo** comando.

    >[!NOTE]
    >Per altre informazioni, vedere comandi Netsh per reti WLAN \(WLAN\) alla [http:\/\/technet.microsoft.com\/libreria\/dd744890.aspx](https://technet.microsoft.com/library/dd744890).

4. Distribuire il nuovo computer wireless per l'utente con la procedura per "Accesso al dominio utilizzando computer che eseguono Windows 10".

Quando l'utente avvia il computer, Windows richiede all'utente di immettere il nome di account utente di dominio e la password. Perché è abilitata per Single Sign-On, il computer utilizza le credenziali dell'account utente di dominio prima di tutto stabilire una connessione con la rete wireless e quindi accedere al dominio.

#### <a name="bkmk_w10"></a>Accedere al dominio utilizzando computer che eseguono Windows 10

1. Disconnettersi o riavviare il computer.

2. Premere un tasto sulla tastiera o fare clic sul desktop. Viene visualizzata la schermata di accesso con un nome di account utente locale visualizzato e un campo di immissione password sotto il nome. Non accedere con l'account utente locale.

3. Nell'angolo inferiore sinistro della schermata, fare clic su **altro utente**. Schermata di accesso altro utente viene visualizzato con due campi, uno per il nome utente e una password. Di seguito la password è il testo **accesso a:** e quindi il nome di dominio in cui viene aggiunto il computer. Ad esempio, se il dominio è example.com, il testo legge **accedano a: ESEMPIO**.

4. In **nome utente**, digitare il nome utente di dominio.

5. In **Password** digitare la password per il dominio, quindi fare clic sulla freccia o premere INVIO.

>[!NOTE]
>Se il **altro utente** schermata non include il testo **accesso a:** e il nome di dominio, è necessario immettere il nome utente nel formato *dominio\\utente*. Ad esempio, per accedere al dominio example.com con un account denominato **utente\-01**, tipo **esempio\\utente\-01**.

### <a name="bkmk_userbootstrap"></a>Aggiungere il dominio e l'accesso tramite configurazione del profilo Wireless Bootstrap dagli utenti
Con questo metodo, completare i passaggi nella sezione passaggi generali, quindi specificare il dominio\-gli utenti con le istruzioni su come configurare manualmente un computer senza fili con un profilo wireless bootstrap. Il profilo wireless bootstrap consente all'utente di stabilire una connessione wireless e quindi aggiunta al dominio. Dopo il computer viene aggiunto al dominio e il riavvio, l'utente può accedere al dominio tramite una connessione wireless.

#### <a name="general-steps"></a>Passaggi generali

1. Configurare un account di amministratore del computer locale, in **Pannello di controllo**, per l'utente.

    >[!IMPORTANT]
    >Per aggiungere un computer a un dominio, l'utente deve essere connesso al computer con l'account amministratore locale. In alternativa, l'utente deve fornire le credenziali per l'account amministratore locale durante il processo di aggiunta del computer al dominio. Inoltre, l'utente deve avere un account utente nel dominio a cui l'utente desidera aggiungere il computer. Durante il processo di aggiunta del computer al dominio, l'utente verrà richiesto per le credenziali dell'account di dominio \(nome utente e password\).

2. Fornire agli utenti di dominio con le istruzioni per configurare un profilo wireless bootstrap, come descritto nella procedura seguente **per configurare un profilo wireless bootstrap**.
3. Inoltre, fornire agli utenti sia le credenziali del computer locale \(nome utente e password\), e le credenziali di dominio \(nome di account utente di dominio e la password\) nel formato *DomainName\\nome utente*, nonché le procedure "Aggiungere il computer al dominio," e "Accesso al dominio," come indicato in Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).

#### <a name="to-configure-a-bootstrap-wireless-profile"></a>Per configurare un profilo wireless bootstrap

1. Utilizzare le credenziali fornite dall'amministratore di rete o il supporto IT professional per accedere al computer con account di amministratore del computer locale.

2. Destra\-fare clic sull'icona della rete sul desktop, quindi **Centro aprire rete e condivisione**. Viene visualizzata la pagina **Centro connessioni di rete e condivisione**. In **modificare le impostazioni di rete**, fare clic su  **impostare una nuova connessione o rete**. Il **impostare una connessione o rete** verrà visualizzata la finestra di dialogo.

3. Fare clic su **connettersi manualmente a una rete wireless**, quindi fare clic su **Avanti**.

4. In **connettersi manualmente a una rete senza fili**, in **nome di rete**, digitare il nome SSID del punto di accesso.  

5. In **tipo di sicurezza**, selezionare l'impostazione fornito dall'amministratore.

6. In **tipo di crittografia** e **chiave di sicurezza**, selezionare o digitare le impostazioni fornite dall'amministratore.

7. Selezionare **avviare automaticamente la connessione**, quindi fare clic su **Avanti**.

8. In **sono stati aggiunti * * * il SSID di rete*, fare clic su **modificare le impostazioni di connessione**.

9. Fare clic su **modificare le impostazioni di connessione**. Il *il SSID di rete* verrà visualizzata la finestra di dialogo Proprietà rete Wireless.

10. Fare clic sul **sicurezza** scheda, quindi nella **scegliere un metodo di autenticazione di rete**, selezionare **PEAP \(PEAP\)** .

11. Fare clic su **Impostazioni**. Il **PEAP \(PEAP\) proprietà** verrà visualizzata la pagina.

12. Nel **PEAP \(PEAP\) proprietà** pagina, assicurarsi che **Convalida certificato server** non è selezionata, fare clic su **OK** due volte, quindi fare clic su **Chiudi**.

13. Windows quindi tenta di connettersi alla rete wireless. Le impostazioni del profilo wireless bootstrap specificano che è necessario fornire le credenziali di dominio. Quando Windows richiede un nome dell'account e la password, digitare le credenziali dell'account di dominio come indicato di seguito:  *Nome di dominio\\nome utente*, *Password del dominio*.

##### <a name="to-join-a-computer-to-the-domain"></a>Per aggiungere un computer al dominio

1. Accedere al computer con l'account Administrator locale.

2. Nella casella di testo di ricerca, digitare **PowerShell**. Nei risultati della ricerca, fare doppio clic su **Windows PowerShell**, quindi fare clic su **Esegui come amministratore**. Verrà visualizzata la finestra di Windows PowerShell con privilegi elevati.

3. In Windows PowerShell, digitare il comando seguente e quindi premere INVIO. Assicurarsi di sostituire i valori di DomainName variabile con il nome del dominio che si desidera aggiungere.
    
    Aggiungi Computer DomainName
    
4. Quando richiesto, digitare il nome utente di dominio e la password e fare clic su **OK**.
5. Riavviare il computer.
6. Seguire le istruzioni nella sezione precedente [accedere al dominio utilizzando computer che eseguono Windows 10](#bkmk_w10).