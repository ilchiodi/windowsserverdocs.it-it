---
title: Configurare i criteri di rete
description: In questo argomento fornisce una panoramica della configurazione di criteri di rete per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abcd9343f10100884f96d17d82704382e2af760
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policies"></a>Configurare i criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per configurare criteri di rete in Criteri di rete.

## <a name="add-a-network-policy"></a>Aggiungere un criterio di rete

Rete Server dei criteri \(NPS\) utilizza criteri di rete e le proprietà di connessione remota degli account utente per determinare se una richiesta di connessione è autorizzato a connettersi alla rete.

È possibile utilizzare questa procedura per configurare un nuovo criterio di rete nella console di criteri di rete o la console di accesso remoto.

### <a name="performing-authorization"></a>Esecuzione di autorizzazione

Quando Criteri di rete esegue l'autorizzazione di una richiesta di connessione, confronta la richiesta con ogni criterio di rete nell'elenco ordinato di criteri, a partire dal primo criterio e quindi lo spostamento verso il basso l'elenco dei criteri configurati. Se Criteri di rete consente di trovare un criterio con le condizioni corrispondono la richiesta di connessione, dei criteri di rete vengono utilizzati i criteri corrispondenti e le chiamate in ingresso dell'account utente di eseguire l'autorizzazione. Se la proprietà di connessione remota dell'account utente sono configurate per concedere l'accesso o controllo accesso tramite criteri di rete e la richiesta di connessione è autorizzata, dei criteri di rete applica le impostazioni configurate nel criterio di rete per la connessione.

Se non trova un criterio di rete che corrisponde alla richiesta di connessione dei criteri di rete, viene rifiutata la richiesta di connessione a meno che non sono impostate le proprietà di connessione remota sull'account utente per concedere l'accesso.

Se la proprietà di connessione remota dell'account utente sono impostate per negare l'accesso, viene rifiutata la richiesta di connessione da criteri di rete.

### <a name="key-settings"></a>Impostazioni delle chiavi

Quando si utilizza la procedura guidata nuovo criterio di rete per creare un criterio di rete, il valore specificato in **metodo di connessione di rete** viene utilizzato per configurare automaticamente il **tipo di criterio** condizione: 

- Se si mantiene il valore predefinito non specificato, i criteri di rete creato viene valutato tramite criteri di rete per tutti i tipi di connessione di rete che utilizzano qualsiasi tipo di server di accesso di rete (NAS).
- Se si specifica un metodo di connessione di rete, dei criteri di rete valuta i criteri di rete solo se la richiesta di connessione deriva dal tipo di server di accesso di rete specificato.

Nel **l'autorizzazione di accesso** pagina, è necessario selezionare **accesso concesso** se si desidera che il criterio per consentire agli utenti di connettersi alla rete. Se si desidera che il criterio per impedire agli utenti di connettersi alla rete, seleziona **accesso negato**. 

Se l'autorizzazione di accesso devono essere determinate da dial-in di proprietà dell'account utente in Active Directory&reg; \(AD DS\) servizi di dominio, è possibile selezionare il **accesso viene determinato dall'utente proprietà di connessione remota** casella di controllo.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-add-a-network-policy"></a>Per aggiungere un criterio di rete 

1. Aprire la console dei criteri di rete e quindi fare doppio clic su **criteri**.

2. Nell'albero della console fare doppio clic su **criteri di rete**e fare clic su **New**. Apre la procedura guidata nuovo criterio di rete.

3. Utilizzare la procedura guidata nuovo criterio di rete per creare un criterio.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Creare criteri di rete per connessioni remote o VPN con una procedura guidata

È possibile utilizzare questa procedura per creare i criteri di richiesta di connessione e criteri di rete necessari per distribuire server di accesso remoto o server di rete privata virtuale \(VPN\) come \(RADIUS\) Remote Authentication Dial-In User Service client al server RADIUS NPS.

>[!NOTE]
>I computer client, ad esempio computer portatili e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless, commutatori di autenticazione 802.1 X, rete privata virtuale \(VPN\) Server e server di connessione remota, perché questi dispositivi utilizzano il protocollo RADIUS per comunicare con server RADIUS, ad esempio i server dei criteri di rete.

Questa procedura viene descritto come aprire la procedura guidata nuovo remote o connessioni di rete privata virtuale in Criteri di rete.

Dopo aver eseguito la procedura guidata, vengono creati i seguenti criteri:

- Criteri di richiesta di una connessione
- Un criterio di rete

È possibile eseguire la procedura guidata nuovo remote o connessioni di rete privata virtuale ogni volta che è necessario creare nuovi criteri per i server di accesso remoto e server VPN.

Eseguire la procedura guidata di nuove connessioni remote o connessioni di rete privata virtuale non è l'unico passaggio necessario per distribuire accesso remoto o server VPN come client RADIUS del server dei criteri di rete. Entrambi i metodi accesso rete richiedono la distribuzione di componenti hardware e software aggiuntivi.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Per creare criteri per connessioni remote o VPN con una procedura guidata

1. Aprire la console dei criteri di rete. Se non è già selezionata, fare clic su **dei criteri di rete \(Local\)**. Se si desidera creare criteri in un server dei criteri di rete remoto, selezionare il server.

2. In **Introduzione** e **configurazione Standard**selezionare **server RADIUS per connessioni remote o VPN**. Il testo e i collegamenti in modifica di testo in base alle selezioni.

3. Fare clic su **configurazione VPN o remote con una procedura guidata**. Apre la creazione guidata di connessione remota o connessioni di rete privata virtuale.

4. Seguire le istruzioni della procedura guidata per completare la creazione dei criteri di nuovo.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Creare criteri di rete per 802.1 X cablata o Wireless con una procedura guidata

È possibile utilizzare questa procedura per creare i criteri di richiesta di connessione e criteri di rete necessari per distribuire i commutatori di autenticazione 802.1 X o punti di accesso wireless 802.1 X come client RADIUS Remote Authentication Dial-In User Service () per il server RADIUS dei criteri di rete.

Questa procedura illustra come avviare la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X in Criteri di rete.

Dopo aver eseguito la procedura guidata, vengono creati i seguenti criteri:

- Criteri di richiesta di una connessione
- Un criterio di rete

Ogni volta che è necessario creare nuovi criteri per l'accesso 802.1 X, è possibile eseguire la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X.

Eseguire la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X non è l'unico passaggio necessario per distribuire 802.1 X commutatori di autenticazione e punti di accesso wireless come client RADIUS per il server dei criteri di rete. Entrambi i metodi accesso rete richiedono la distribuzione di componenti hardware e software aggiuntivi.

Appartenenza al gruppo **Domain Admins**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Per creare criteri per 802.1 X cablata o wireless con una procedura guidata

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete. 

2. Se non è già selezionata, fare clic su **dei criteri di rete \(Local\)**. Se si desidera creare criteri in un server dei criteri di rete remoto, selezionare il server.

3. In **Introduzione** e **configurazione Standard**selezionare **server RADIUS per connessioni cablate o Wireless 802.1 X**. Il testo e i collegamenti in modifica di testo in base alle selezioni.

4. Fare clic su **configurazione 802.1 X usando una procedura guidata**. Apre la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X.

5. Seguire le istruzioni della procedura guidata per completare la creazione dei criteri di nuovo.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurare i criteri di rete per ignorare Dial-in proprietà dell'Account utente

Utilizzare questa procedura per configurare un criterio di rete dei criteri di rete per ignorare le proprietà di connessione dell'account utente in Active Directory durante il processo di autorizzazione. Gli account utente in Active Directory Users and Computers sono proprietà di connessione remota che valuta dei criteri di rete durante il processo di autorizzazione, a meno che il **autorizzazione di accesso alla rete** proprietà dell'account utente è impostata su **controllare l'accesso tramite criteri di rete NPS**. 

Esistono due casi in cui si desidera configurare i criteri di rete per ignorare le proprietà di connessione dell'account utente in Active Directory:

- Quando si desidera per semplificare l'autorizzazione dei criteri di rete tramite criteri di rete, ma non tutti gli account utente hanno il **autorizzazione di accesso alla rete** proprietà impostata su **controllare l'accesso tramite criteri di rete NPS**. Ad esempio, alcuni account utente disponga di **autorizzazione di accesso alla rete** impostare proprietà dell'account utente **negare l'accesso** o **consentire l'accesso**.

- Altre proprietà di connessione remota degli account utente quando non sono applicabili al tipo di connessione configurati nel criterio di rete. Proprietà, ad esempio, ad eccezione di **autorizzazione di accesso alla rete** impostazione sono applicabili solo a chiamate in ingresso o le connessioni VPN, ma i criteri di rete che si sta creando per connessioni wireless o l'autenticazione tramite commutatore.

È possibile utilizzare questa procedura per configurare criteri di rete per ignorare dial-in di proprietà dell'account utente. Se la richiesta di connessione soddisfa i criteri di rete in cui questa casella di controllo è selezionata, dei criteri di rete non utilizza la proprietà di connessione remota dell'account utente per determinare se l'utente o il computer è autorizzato ad accedere alla rete; solo le impostazioni nei criteri di rete vengono utilizzate per determinare le autorizzazioni.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.

2. Fare doppio clic su **criteri**, fare clic su **criteri di rete**e quindi nel riquadro dei dettagli fare doppio clic su criteri che si desidera configurare.

3. Nei criteri di **proprietà** della finestra di dialogo di **Panoramica** nella scheda **l'autorizzazione di accesso**, selezionare il **dial-in di proprietà dell'account utente di ignorare** casella di controllo, quindi fare clic su **OK**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Per configurare criteri di rete per ignorare dial-in di proprietà dell'account utente



## <a name="configure-nps-for-vlans"></a>Configurare i criteri di rete per le VLAN

Con server di accesso di rete compatibile con VLAN e dei criteri di rete in Windows Server 2016, è possibile fornire i gruppi di utenti con accesso solo alle risorse di rete appropriate per le relative autorizzazioni di sicurezza. Ad esempio, è possibile fornire ai visitatori con accesso wireless a Internet senza autorizzare l'accesso alla rete dell'organizzazione. 

Inoltre, le VLAN consentono alle logicamente gruppo risorse di rete che esiste in posizioni fisiche diverse o in diverse subnet fisiche. Ad esempio, i membri del reparto vendito e le risorse di rete, ad esempio i computer client, server e stampanti, potrebbero trovarsi in diversi diversi edifici dell'organizzazione, ma è possibile distribuire tutte le risorse in una rete VLAN che utilizza stesso intervallo di indirizzi IP. Le VLAN quindi funzioni, dal punto di vista degli utenti finali, come una singola subnet.

È anche possibile utilizzare VLAN per separare una rete tra i diversi gruppi di utenti. Dopo aver stabilito la modalità di definire i gruppi, puoi creare gruppi di sicurezza in Active Directory Users e computer lo snap-in e quindi aggiungere membri ai gruppi.

### <a name="configure-a-network-policy-for-vlans"></a>Configurare un criterio di rete per le VLAN

È possibile utilizzare questa procedura per configurare un criterio di rete che assegna gli utenti a una VLAN. Quando si utilizza una VLAN con hardware di rete, ad esempio router, commutatori e i controller di accesso, è possibile configurare criteri di rete per richiedere i server di accesso per posizionare i membri dei gruppi di Active Directory specifico su VLAN specifiche. La possibilità di risorse di rete di gruppo in modo logico con VLAN offre flessibilità durante la progettazione e implementazione di soluzioni di rete.

Quando si configurano le impostazioni dei criteri di rete dei criteri di rete per l'utilizzo con le VLAN, è necessario configurare gli attributi **medio-tipo di Tunnel**, **Tunnel-Pvt-Group-ID**, **tipo di Tunnel**, e **Tunnel Tag**. 

Questa procedura viene fornita come linee guida; la configurazione di rete potrebbe richiedere impostazioni diverse rispetto a quelle descritte di seguito.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-a-network-policy-for-vlans"></a>Per configurare un criterio di rete per le VLAN

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.

2. Fare doppio clic su **criteri**, fare clic su **criteri di rete**e quindi nel riquadro dei dettagli fare doppio clic su criteri che si desidera configurare.

3. Nei criteri di **proprietà** la finestra di dialogo, fare clic su di **impostazioni** scheda.

4. In Criteri di **proprietà**, in **impostazioni**, in **attributi RADIUS**, assicurarsi che **Standard** sia selezionata.

5. Nel riquadro dei dettagli, in **attributi**, **tipo di servizio** attributo è configurato con un valore predefinito di **frame**. Per impostazione predefinita, per i criteri con metodi di accesso della VPN e accesso remoto, il **frame protocollo** attributo è configurato con un valore di **PPP**. Per specificare gli attributi di connessione aggiuntivi necessari per le VLAN, fare clic su **Aggiungi**. Il **Aggiungi attributo RADIUS Standard** apre la finestra di dialogo.

6. In **Aggiungi attributo RADIUS Standard**, negli attributi, scorrere verso il basso e aggiungere gli attributi seguenti:

    - **Tipo di supporto tunnel**. Selezionare un valore appropriato per le selezioni effettuate per i criteri. Ad esempio, se si sta configurando i criteri di rete sono un criterio wireless, selezionare **valore: 802 (supporti comprende tutti 802 più formato canonico Ethernet)**.

    - **ID gruppo di Pvt tunnel**. Immettere il numero intero che rappresenta il numero VLAN a cui verranno assegnati i membri del gruppo. 

    - **Tipo di tunnel**. Selezionare **della LAN virtuale (VLAN)**.


7. In **Aggiungi attributo RADIUS Standard**, fare clic su **Chiudi**. 

8. Se il server di accesso di rete (NAS) richiede l'utilizzo del **Tunnel Tag** dell'attributo, attenersi alla seguente procedura per aggiungere il **Tunnel Tag** attributo per i criteri di rete. Se la relativa documentazione questo attributo non è specificato, non aggiungerlo ai criteri. Se necessario, aggiungere gli attributi come segue:

    - In Criteri di **proprietà**, in **impostazioni**, in **attributi RADIUS**, fare clic su **specifici del fornitore**. 

    - Nel riquadro dei dettagli, fare clic su **Aggiungi**. Il **aggiungere attributi specifici del fornitore** apre la finestra di dialogo.

    - In **attributi**, scorrere verso il basso e selezionare **Tunnel Tag**, quindi fare clic su **Aggiungi**. Il **informazioni sugli attributi** apre la finestra di dialogo. 

    - In **valore dell'attributo**, digitare il valore proveniente dalla documentazione dell'hardware.

## <a name="configure-the-eap-payload-size"></a>Configurare la dimensione del Payload EAP

In alcuni casi, router o firewall ignorare i pacchetti, perché sono configurati per scartare i pacchetti che richiedono la frammentazione.

Quando si distribuiscono i criteri di rete con i criteri di rete che utilizzano il \(EAP\) Extensible Authentication Protocol con Transport Layer Security \(TLS\) o EAP-TLS, come un metodo di autenticazione, l'unità massima di trasmissione predefinita \(MTU\) che utilizza criteri di rete per il payload EAP è 1500 byte. 

Questo valore massimo per il payload EAP è possibile creare messaggi RADIUS che richiedono la frammentazione da un router o un firewall tra il server dei criteri di rete e un client RADIUS. In questo caso, un router o firewall posizionato tra il client RADIUS e il server dei criteri di rete potrebbe eliminare automaticamente alcuni frammenti, generando un errore di autenticazione e l'impossibilità del client di accesso per connettersi alla rete.

Utilizzare la procedura seguente per ridurre la dimensione massima dei criteri di rete viene utilizzato per il payload EAP regolando l'attributo frame MTU in un criterio di rete su un valore non superiore a 1344.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-the-framed-mtu-attribute"></a>Per configurare l'attributo di MTU di frame

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.
 
2. Fare doppio clic su **criteri**, fare clic su **criteri di rete**e quindi nel riquadro dei dettagli fare doppio clic su criteri che si desidera configurare.

3. Nei criteri di **proprietà** la finestra di dialogo, fare clic su di **impostazioni** scheda.

4. In **impostazioni**, in **attributi RADIUS**, fare clic su **Standard**. Nel riquadro dei dettagli, fare clic su **Aggiungi**. Il **Aggiungi attributo RADIUS Standard** apre la finestra di dialogo.

5. In **attributi**, scorrere verso il basso e fare clic su **frame MTU**, quindi fare clic su **Aggiungi**. Il **informazioni sugli attributi** apre la finestra di dialogo.

6. In **valore dell'attributo**, digitare un valore pari o inferiore rispetto a **1344**. Fare clic su **OK**, fare clic su **Chiudi**, quindi fare clic su **OK**.



Per ulteriori informazioni sui criteri di rete, vedere [criteri di rete](nps-np-overview.md).

Per esempi di caratteri jolly per specificare gli attributi dei criteri di rete, vedere [utilizzare espressioni regolari in Criteri di rete](nps-crp-reg-expressions.md).

Per ulteriori informazioni sui criteri di rete, vedere [Server dei criteri di rete (NPS)](nps-top.md).
