---
title: Configurare i criteri di rete
description: In questo argomento viene fornita una panoramica della configurazione dei criteri di rete per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8a03a6c67ddd59549cefc9742ca92e0702f92a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842002"
---
# <a name="configure-network-policies"></a>Configurare i criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare i criteri di rete in Criteri di rete.

## <a name="add-a-network-policy"></a>Aggiungere un criterio di rete

Server dei criteri di rete \(NPS\) Usa i criteri e le proprietà di connessione remota degli account utente per determinare se una richiesta di connessione è autorizzata a connettersi alla rete di rete.

È possibile utilizzare questa procedura per configurare un nuovo criterio di rete nella console Criteri di rete o la console di accesso remoto.

### <a name="performing-authorization"></a>Esegue l'autorizzazione

Quando si esegue inoltre l'autorizzazione di una richiesta di connessione, confronta la richiesta con ogni criterio di rete nell'elenco ordinato dei criteri, iniziando con il primo criterio e quindi spostando verso il basso l'elenco di criteri configurati. Se Criteri di rete consente di trovare un criterio le cui condizioni corrispondano alla richiesta di connessione, dei criteri di rete Usa i criteri di corrispondenza e le proprietà di connessione remota dell'account utente di eseguire l'autorizzazione. Se la proprietà di connessione remota dell'account utente sono configurate per concedere l'accesso di accesso o il controllo tramite criteri di rete e la richiesta di connessione è autorizzata, dei criteri di rete applica le impostazioni configurate nei criteri di rete per la connessione.

Se Criteri di rete non trova un criterio di rete che corrisponde alla richiesta di connessione, la richiesta di connessione viene rifiutata, a meno che le proprietà dial-in per l'account utente vengono impostate per concedere l'accesso.

Se la proprietà di connessione remota dell'account utente sono impostate per negare l'accesso, la richiesta di connessione viene rifiutata da criteri di rete.

### <a name="key-settings"></a>Impostazioni delle chiavi

Quando si utilizza la procedura guidata nuovo criterio di rete per creare un criterio di rete, il valore specificato nella **metodo di connessione di rete** consente di configurare automaticamente il **tipo di criterio** condizione: 

- Se si mantengono il valore predefinito non specificato, i criteri di rete creato viene valutato da criteri di rete per tutti i tipi di connessione di rete che usano qualsiasi tipo di server di accesso di rete (NAS).
- Se si specifica un metodo di connessione di rete, criteri di rete valuta i criteri di rete solo se la richiesta di connessione proviene dal tipo di rete del server di accesso specificato.

Nel **l'autorizzazione di accesso** pagina, è necessario selezionare **accesso concesso** se si desidera che i criteri per consentire agli utenti di connettersi alla rete. Se si desidera che i criteri per impedire agli utenti di connettersi alla rete, seleziona **accesso negato**. 

Se si desidera che l'autorizzazione di accesso deve essere determinato da dial-in di proprietà dell'account utente in Active Directory&reg; servizi di dominio \(Active Directory Domain Services\), è possibile selezionare il **accesso è determinato dalle proprietà Dial-in utente** casella di controllo.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-add-a-network-policy"></a>Per aggiungere un criterio di rete 

1. Aprire la console Criteri di rete e quindi fare doppio clic su **criteri**.

2. Nell'albero della console, fare doppio clic su **i criteri di rete**, fare clic su **New**. Verrà visualizzata la procedura guidata nuovo criterio di rete.

3. Usare la procedura guidata nuovo criterio di rete per creare un criterio.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Creare i criteri di rete per connessioni remote o VPN con una procedura guidata

È possibile utilizzare questa procedura per creare la connessione ai criteri di richiesta e i criteri necessari per distribuire accesso remoto server o rete privata virtuale di rete \(VPN\) server come Remote Authentication Dial-In User Service \(Raggio\) i client per il server NPS RADIUS.

>[!NOTE]
>I computer client, ad esempio computer portatili e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless, commutatori di autenticazione 802.1X, rete privata virtuale \(VPN\) server Accesso remoto e server, poiché questi dispositivi usano il protocollo RADIUS per comunicare con i server RADIUS, ad esempio NPSs.

Questa procedura viene illustrato come aprire la procedura guidata accesso remoto o connessioni di rete privata virtuale nuova in Criteri di rete.

Dopo aver eseguito la procedura guidata, vengono creati i criteri seguenti:

- Criterio di richiesta di una connessione
- Un criterio di rete

È possibile eseguire la procedura guidata di nuove connessioni remote o connessioni di rete privata virtuale ogni volta che è necessario creare nuovi criteri per i server di accesso remoto e server VPN.

Esecuzione della procedura guidata di nuove connessioni remote o connessioni di rete privata virtuale non è l'unico passaggio necessario per distribuire accesso remoto o server VPN come client RADIUS ai criteri di rete. Entrambi metodi di accesso di rete richiedono la distribuzione di componenti hardware e software aggiuntivi.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Per creare i criteri per connessioni remote o VPN con una procedura guidata

1. Aprire la console Criteri di rete. Se non è già selezionata, fare clic su **NPS \(locale\)**. Se si desidera creare criteri in un server NPS remoto, selezionare il server.

2. Nelle **Guida introduttiva** e **Standard di configurazione**, selezionare **server RADIUS per connessioni remote o VPN**. Il testo e collegamenti nella sezione di modifica del testo in modo da riflettere la selezione.

3. Fare clic su **configurazione VPN o connessioni remote con una procedura guidata**. Verrà visualizzata la creazione guidata nuova remote o a connessioni di rete privata virtuale.

4. Seguire le istruzioni della procedura guidata per completare la creazione dei nuovi criteri.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Creare criteri di rete per 802.1 X cablata o Wireless con una procedura guidata

È possibile utilizzare questa procedura per creare il criterio di richiesta di connessione e criteri di rete che sono necessari per distribuire i commutatori di autenticazione 802.1x o punti di accesso wireless 802.1 X come client RADIUS Remote Authentication Dial-In User Service () per i criteri di rete Server RADIUS.

Questa procedura viene illustrato come avviare la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X in Criteri di rete.

Dopo aver eseguito la procedura guidata, vengono creati i criteri seguenti:

- Criterio di richiesta di una connessione
- Un criterio di rete

Ogni volta che è necessario creare nuovi criteri per l'accesso 802.1X, è possibile eseguire la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X.

Esecuzione della procedura guidata le connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X non è l'unico passaggio obbligatorio per la distribuzione di 802.1 X commutatori di autenticazione e punti di accesso wireless come client RADIUS ai criteri di rete. Entrambi metodi di accesso di rete richiedono la distribuzione di componenti hardware e software aggiuntivi.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Per creare i criteri per 802.1 X cablata o wireless con una procedura guidata

1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete. 

2. Se non è già selezionata, fare clic su **NPS \(locale\)**. Se si desidera creare criteri in un server NPS remoto, selezionare il server.

3. Nelle **Guida introduttiva** e **Standard di configurazione**, selezionare **server RADIUS per connessioni cablate o Wireless 802.1 X**. Il testo e collegamenti nella sezione di modifica del testo in modo da riflettere la selezione.

4. Fare clic su **configurazione 802.1 X tramite una procedura guidata**. Verrà visualizzata la procedura guidata connessioni Wireless e cablate proteggere di nuovo IEEE 802.1 X.

5. Seguire le istruzioni della procedura guidata per completare la creazione dei nuovi criteri.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurare criteri di rete per ignorare la proprietà di connessione remota Account utente

Utilizzare questa procedura per configurare un criterio di rete NPS per ignorare le proprietà di connessione remota degli account utente in Active Directory durante il processo di autorizzazione. Gli account utente in Active Directory Users and Computers hanno proprietà di connessione remota che valuta dei criteri di rete durante il processo di autorizzazione, a meno che il **l'autorizzazione di accesso di rete** dell'account utente è impostata su **controllo accesso tramite criteri di rete NPS**. 

Esistono due casi in cui è possibile configurare criteri di rete per ignorare le proprietà di connessione remota degli account utente in Active Directory:

- Quando si vuole semplificare autorizzazione NPS utilizzando criteri di rete, ma non tutti gli account utente hanno il **l'autorizzazione di accesso di rete** proprietà impostata su **controllare l'accesso tramite criteri di rete NPS**. Ad esempio, potrebbe avere alcuni account utente di **l'autorizzazione di accesso di rete** proprietà dell'account utente impostato su **negare l'accesso** o **consentire l'accesso**.

- Altre proprietà di connessione remota degli account utente quando non sono applicabili al tipo di connessione configurata nei criteri di rete. Ad esempio, proprietà, ad eccezione di **autorizzazione di accesso alla rete** impostazione sono applicabili solo alle chiamate in ingresso o le connessioni VPN, ma i criteri di rete che si sta creando per le connessioni del commutatore di rete wireless o che esegue l'autenticazione.

È possibile utilizzare questa procedura per configurare criteri di rete per ignorare la proprietà di connessione remota account utente. Se una richiesta di connessione corrisponde al criterio di rete in cui è selezionata questa casella di controllo, criteri di rete non usa le proprietà di connessione remota dell'account utente per determinare se l'utente o il computer è autorizzato ad accedere alla rete; solo le impostazioni nei criteri di rete vengono utilizzate per determinare l'autorizzazione.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.

2. Fare doppio clic su **politiche**, fare clic su **criteri di rete**e quindi nel riquadro dei dettagli fare doppio clic sul criterio che si desidera configurare.

3. Nel criterio **proprietà** finestra di dialogo il **Panoramica** nella scheda **l'autorizzazione di accesso**, selezionare il **ignorare dial-in di proprietà dell'account utente**casella di controllo e quindi fare clic su **OK**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Per configurare criteri di rete per ignorare la proprietà di connessione remota account utente



## <a name="configure-nps-for-vlans"></a>Configurare criteri di rete per le VLAN

Con server di accesso alla rete VLAN riconoscere e dei criteri di rete in Windows Server 2016, è possibile fornire i gruppi di utenti con accesso solo alle risorse di rete appropriate per le relative autorizzazioni di sicurezza. Ad esempio, è possibile fornire ai visitatori con accesso wireless a Internet senza consentire loro l'accesso alla rete dell'organizzazione. 

Inoltre, le VLAN consentono in modo logico risorse di rete di gruppo sono presenti in posizioni fisiche diverse o in diverse subnet fisiche. Ad esempio, i membri del reparto vendito e le relative risorse di rete, ad esempio i computer client, server e le stampanti, potrebbero trovarsi in vari edifici diversi nell'organizzazione, ma è possibile inserire tutte le risorse in una rete VLAN che utilizza lo stesso indirizzo IP intervallo di indirizzi. Le VLAN quindi le funzioni, dal punto di vista dell'utente finale, come una singola subnet.

È anche possibile usare reti VLAN quando si desidera separare una rete tra diversi gruppi di utenti. Dopo aver stabilito come si vuole definire i gruppi, è possibile creare gruppi di sicurezza in Active Directory gli utenti lo snap-in computer e quindi aggiungere i membri ai gruppi.

### <a name="configure-a-network-policy-for-vlans"></a>Configurare un criterio di rete per le VLAN

È possibile utilizzare questa procedura per configurare un criterio di rete che assegna gli utenti a una VLAN. Quando si usa hardware rete compatibili con VLAN, ad esempio i router, commutatori e i controller di accesso, è possibile configurare criteri di rete per indicare i server di accesso per posizionare i membri di gruppi di Active Directory specifici su VLAN specifica. La possibilità di raggruppare le risorse di rete in modo logico con VLAN offre flessibilità durante la progettazione e implementazione di soluzioni di rete.

Quando si configurano le impostazioni dei criteri di rete dei criteri di rete per l'uso con le VLAN, è necessario configurare gli attributi **Tunnel-Media-Type**, **Tunnel-Pvt-Group-ID**, **Tunnel-Type**, e **Tunnel-Tag**. 

Questa procedura viene fornita come linea guida; la configurazione di rete potrebbe richiedere impostazioni diverse rispetto a quelli descritti di seguito.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-a-network-policy-for-vlans"></a>Per configurare un criterio di rete per le VLAN

1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.

2. Fare doppio clic su **politiche**, fare clic su **criteri di rete**e quindi nel riquadro dei dettagli fare doppio clic sul criterio che si desidera configurare.

3. Nel criterio **delle proprietà** nella finestra di dialogo fare clic sul **impostazioni** scheda.

4. Nei criteri **delle proprietà**, in **impostazioni**, in **attributi RADIUS**, assicurarsi che **Standard** sia selezionata.

5. Nel riquadro dei dettagli, in **attributi**, il **tipo di servizio** attributo è configurato con valore predefinito è **Framed**. Per impostazione predefinita, per i criteri con i metodi di accesso di VPN e accesso remoto, il **Framed-Protocol** attributo è configurato con il valore **PPP**. Per specificare attributi aggiuntivi per la connessione necessari per le VLAN, fare clic su **Add**. Il **Aggiungi attributo RADIUS Standard** verrà visualizzata la finestra di dialogo.

6. Nelle **Aggiungi attributo RADIUS Standard**, negli attributi, scorrere verso il basso e aggiungere gli attributi seguenti:

    - **Tunnel-Media-Type**. Selezionare un valore appropriato per le selezioni precedenti che sono state apportate per i criteri. Ad esempio, se i criteri di rete che si sta configurando un criterio wireless, selezionare **valore: 802 (include 802 tutti i supporti più formato canonico Ethernet)**.

    - **Tunnel-Pvt-Group-ID**. Immettere il valore integer che rappresenta il numero VLAN a cui verranno assegnati membri del gruppo. 

    - **Tipo di tunnel**. Selezionare **LAN virtuali (VLAN)**.


7. Nelle **Aggiungi attributo RADIUS Standard**, fare clic su **Chiudi**. 

8. Se il server di accesso di rete (NAS) richiede l'utilizzo dei **Tunnel-Tag** dell'attributo, utilizzare la procedura seguente per aggiungere il **Tunnel-Tag** attributo ai criteri di rete. Se la relativa documentazione non è specificato questo attributo, non aggiungerlo ai criteri. Se necessario, aggiungere gli attributi nel modo seguente:

    - Nei criteri **delle proprietà**, nel **impostazioni**, in **attributi RADIUS**, fare clic su **fornitore specifico**. 

    - Nel riquadro dei dettagli, fare clic su **Add**. Il **aggiungere attributi specifici fornitore** verrà visualizzata la finestra di dialogo.

    - Nelle **attributi**, scorrere verso il basso e selezionare **Tunnel-Tag**, quindi fare clic su **Add**. Il **le informazioni sugli attributi** verrà visualizzata la finestra di dialogo. 

    - Nelle **valore dell'attributo**, digitare il valore ottenuto dalla documentazione dell'hardware.

## <a name="configure-the-eap-payload-size"></a>Configurare le dimensioni del Payload EAP

In alcuni casi, i router o firewall eliminare pacchetti perché sono configurati per scartare i pacchetti che richiedono la frammentazione.

Quando si distribuiscono criteri di rete con i criteri di rete che usano Extensible Authentication Protocol \(EAP\) con Transport Layer Security \(TLS\), o EAP-TLS, come un metodo di autenticazione, il valore massimo predefinito unità di trasmissione \(MTU\) che usa criteri di rete per i payload EAP è 1500 byte. 

Queste dimensioni massime per il payload EAP possono creare messaggi RADIUS che richiedono la frammentazione da router o firewall tra i criteri di rete e un client RADIUS. In questo caso, un router o firewall posizionate tra il client RADIUS e i criteri di rete potrebbe scartano alcuni frammenti, causando un errore di autenticazione e l'impossibilità del client di accesso per connettersi alla rete.

Utilizzare la seguente procedura per ridurre le dimensioni massime che usa criteri di rete per i payload EAP modificando l'attributo Framed MTU nei criteri di rete su un valore non superiore a 1344.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-the-framed-mtu-attribute"></a>Per configurare l'attributo Framed MTU

1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.
 
2. Fare doppio clic su **politiche**, fare clic su **criteri di rete**e quindi nel riquadro dei dettagli fare doppio clic sul criterio che si desidera configurare.

3. Nel criterio **delle proprietà** nella finestra di dialogo fare clic sul **impostazioni** scheda.

4. Nelle **le impostazioni**, nel **attributi RADIUS**, fare clic su **Standard**. Nel riquadro dei dettagli, fare clic su **Add**. Il **Aggiungi attributo RADIUS Standard** verrà visualizzata la finestra di dialogo.

5. Nelle **attributi**, scorrere verso il basso e fare clic su **Framed MTU**, quindi fare clic su **Add**. Il **le informazioni sugli attributi** verrà visualizzata la finestra di dialogo.

6. Nelle **valore dell'attributo**, digitare un valore uguale o inferiore rispetto a **1344**. Fare clic su **OK**, fare clic su **Close**, quindi fare clic su **OK**.



Per altre informazioni sui criteri di rete, vedere [i criteri di rete](nps-np-overview.md).

Per esempi di caratteri jolly per specificare gli attributi di criteri di rete, vedere [utilizzo delle espressioni regolari in Criteri di rete](nps-crp-reg-expressions.md).

Per altre informazioni sui criteri di rete, vedere [Strumentazione gestione Windows (NPS, Network Policy Server)](nps-top.md).
