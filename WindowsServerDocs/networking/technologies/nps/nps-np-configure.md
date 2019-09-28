---
title: Configurare i criteri di rete
description: Questo argomento fornisce una panoramica della configurazione dei criteri di rete per server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fe77655a-e2be-4949-92e1-aaaa215d86ea
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2bde42ba9b9489ddcd8fb3673ec5ddf1fd4d970
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396356"
---
# <a name="configure-network-policies"></a>Configurare i criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per configurare i criteri di rete in server dei criteri di rete.

## <a name="add-a-network-policy"></a>Aggiungere un criterio di rete

Server dei criteri di rete \(NPS @ no__t-1 USA i criteri di rete e le proprietà di accesso remoto degli account utente per determinare se una richiesta di connessione è autorizzata a connettersi alla rete.

È possibile utilizzare questa procedura per configurare un nuovo criterio di rete nella console server dei criteri di rete o nella console di accesso remoto.

### <a name="performing-authorization"></a>Esecuzione dell'autorizzazione

Quando server dei criteri di rete esegue l'autorizzazione di una richiesta di connessione, confronta la richiesta con ciascun criterio di rete nell'elenco ordinato dei criteri, a partire dal primo criterio, quindi spostandolo verso il basso nell'elenco dei criteri configurati. Se NPS rileva un criterio le cui condizioni corrispondono alla richiesta di connessione, NPS usa i criteri di corrispondenza e le proprietà di connessione dell'account utente per eseguire l'autorizzazione. Se le proprietà di connessione dell'account utente sono configurate per concedere l'accesso o controllare l'accesso tramite criteri di rete e la richiesta di connessione è autorizzata, NPS applica le impostazioni configurate nei criteri di rete alla connessione.

Se Server dei criteri di rete non trova un criterio di rete corrispondente alla richiesta di connessione, la richiesta di connessione viene rifiutata, a meno che le proprietà del dial-in nell'account utente non siano impostate per concedere l'accesso.

Se le proprietà di connessione dell'account utente sono impostate per negare l'accesso, la richiesta di connessione viene rifiutata dal server dei criteri di gruppo.

### <a name="key-settings"></a>Impostazioni chiave

Quando si utilizza la creazione guidata nuovo criterio di rete per creare un criterio di rete, il valore specificato in **metodo di connessione di rete** viene utilizzato per configurare automaticamente la condizione del tipo di **criteri** : 

- Se si mantiene il valore predefinito non specificato, i criteri di rete creati vengono valutati da server dei criteri di rete per tutti i tipi di connessione di rete che utilizzano qualsiasi tipo di server di accesso alla rete (NAS).
- Se si specifica un metodo di connessione di rete, server dei criteri di rete valuta i criteri di rete solo se la richiesta di connessione proviene dal tipo di server di accesso alla rete specificato.

Nella pagina **autorizzazioni di accesso** è necessario selezionare **accesso concesso** se si desidera che i criteri consentano agli utenti di connettersi alla rete. Per impedire che gli utenti si connettano alla rete, selezionare **accesso negato**. 

Se si desidera che le autorizzazioni di accesso siano determinate dalle proprietà di connessione remota dell'account utente in Active Directory @ no__t-0 Domain Services \(AD DS @ no__t-2, è possibile selezionare la casella **di controllo accesso determinato da proprietà di connessione utente** .

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-add-a-network-policy"></a>Per aggiungere un criterio di rete 

1. Aprire la console NPS, quindi fare doppio clic su **criteri**.

2. Nell'albero della console fare clic con il pulsante destro del mouse su **criteri di rete**e scegliere **nuovo**. Verrà visualizzata la procedura guidata nuovo criterio di rete.

3. Utilizzare la creazione guidata nuovo criterio di rete per creare un criterio.

## <a name="create-network-policies-for-dial-up-or-vpn-with-a-wizard"></a>Creare criteri di rete per la connessione remota o VPN con una procedura guidata

È possibile utilizzare questa procedura per creare i criteri di richiesta di connessione e i criteri di rete necessari per distribuire server di connessione remota o rete privata virtuale \(VPN @ no__t-1 server come Remote Authentication Dial-In User Service \(RADIUS @ no__t-3 client per il server RADIUS NPS.

>[!NOTE]
>I computer client, ad esempio computer portatili e altri computer che eseguono sistemi operativi client, non sono client RADIUS. I client RADIUS sono server di accesso alla rete, ad esempio punti di accesso wireless, commutatori di autenticazione 802.1 X, rete privata virtuale \(VPN @ no__t-1 server e server di connessione remota, perché questi dispositivi usano il protocollo RADIUS per comunicare con RADIUS Server, ad esempio NPSs.

In questa procedura viene illustrato come aprire la creazione guidata nuova connessione remota o rete privata virtuale in server dei criteri di rete.

Dopo aver eseguito la procedura guidata, vengono creati i seguenti criteri:

- Un criterio di richiesta di connessione
- Un criterio di rete

È possibile eseguire la nuova procedura guidata connessione remota o connessione di rete privata virtuale ogni volta che è necessario creare nuovi criteri per i server di connessione remota e i server VPN.

L'esecuzione della nuova procedura guidata connessione remota o connessione di rete privata virtuale non è l'unico passaggio necessario per distribuire server VPN o remote come client RADIUS al server dei criteri di rete. Entrambi i metodi di accesso alla rete richiedono la distribuzione di componenti hardware e software aggiuntivi.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-create-policies-for-dial-up-or-vpn-with-a-wizard"></a>Per creare criteri per la connessione remota o VPN con una procedura guidata

1. Aprire la console NPS. Se non è già selezionata, fare clic su **NPS \(locale\)** . Se si desidera creare criteri in un server dei criteri di server remoto, selezionare il server.

2. In **Introduzione** e **configurazione standard**Selezionare **server RADIUS per connessioni remote o VPN**. Il testo e i collegamenti sotto il testo cambiano per riflettere la selezione.

3. Fare clic su **Configura VPN o connessione remota con una procedura guidata**. Verrà visualizzata la procedura guidata nuova connessione remota o connessione di rete privata virtuale.

4. Seguire le istruzioni della procedura guidata per completare la creazione dei nuovi criteri.

## <a name="create-network-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Creazione di criteri di rete per 802.1 X cablata o wireless con una procedura guidata

È possibile utilizzare questa procedura per creare i criteri di richiesta di connessione e i criteri di rete necessari per distribuire i commutatori di autenticazione 802.1 X o i punti di accesso wireless 802.1 X come client Remote Authentication Dial-In User Service (RADIUS) al server dei criteri di rete Server RADIUS.

In questa procedura viene illustrato come avviare la nuova procedura guidata per le connessioni cablate e wireless protette tramite IEEE 802.1 X nel server dei criteri di rete.

Dopo aver eseguito la procedura guidata, vengono creati i seguenti criteri:

- Un criterio di richiesta di connessione
- Un criterio di rete

È possibile eseguire la nuova procedura guidata per le connessioni cablate e wireless sicure IEEE 802.1 X ogni volta che è necessario creare nuovi criteri per l'accesso 802.1 X.

L'esecuzione della nuova procedura guidata per le connessioni cablate e wireless sicure IEEE 802.1 X non è l'unico passaggio necessario per distribuire i commutatori di autenticazione 802.1 X e i punti di accesso wireless come client RADIUS per NPS. Entrambi i metodi di accesso alla rete richiedono la distribuzione di componenti hardware e software aggiuntivi.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

### <a name="to-create-policies-for-8021x-wired-or-wireless-with-a-wizard"></a>Per creare criteri per 802.1 X cablata o wireless con una procedura guidata

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS. 

2. Se non è già selezionata, fare clic su **NPS \(locale\)** . Se si desidera creare criteri in un server dei criteri di server remoto, selezionare il server.

3. In **Introduzione** e **configurazione standard**Selezionare **server RADIUS per connessioni wireless o cablate 802.1 x**. Il testo e i collegamenti sotto il testo cambiano per riflettere la selezione.

4. Fare clic su **Configura 802.1 x tramite una procedura guidata**. Si apre la nuova procedura guidata per le connessioni cablate e wireless sicure IEEE 802.1 X.

5. Seguire le istruzioni della procedura guidata per completare la creazione dei nuovi criteri.

## <a name="configure-nps-to-ignore-user-account-dial-in-properties"></a>Configurare NPS per ignorare le proprietà di connessione remota dell'account utente

Utilizzare questa procedura per configurare un criterio di rete NPS per ignorare le proprietà di accesso remoto degli account utente in Active Directory durante il processo di autorizzazione. Gli account utente in Active Directory utenti e computer dispongono di proprietà di connessione remota che i server dei criteri di rete valutano durante il processo di autorizzazione, a meno che la proprietà **autorizzazioni di accesso alla rete** dell'account utente non sia impostata per **controllare l'accesso tramite criteri di rete NPS** . 

Esistono due casi in cui potrebbe essere necessario configurare NPS per ignorare le proprietà di accesso remoto degli account utente in Active Directory:

- Quando si desidera semplificare l'autorizzazione NPS utilizzando i criteri di rete, ma non tutti gli account utente dispongono della proprietà **autorizzazioni di accesso alla rete** impostata per **controllare l'accesso tramite criteri di rete NPS**. Ad esempio, alcuni account utente potrebbero avere la proprietà **autorizzazioni di accesso alla rete** del set di account utente per **negare l'accesso** o **consentire l'accesso**.

- Quando altre proprietà del dial-in degli account utente non sono applicabili al tipo di connessione configurato nei criteri di rete. Ad esempio, le proprietà diverse dall'impostazione delle **autorizzazioni di accesso alla rete** sono applicabili solo alle connessioni remote o VPN, ma i criteri di rete creati sono per le connessioni wireless o per l'autenticazione del commutatore.

È possibile utilizzare questa procedura per configurare NPS per ignorare le proprietà di connessione remota dell'account utente. Se una richiesta di connessione corrisponde ai criteri di rete in cui questa casella di controllo è selezionata, NPS non utilizzerà le proprietà di connessione dell'account utente per determinare se l'utente o il computer è autorizzato ad accedere alla rete. per determinare l'autorizzazione vengono utilizzate solo le impostazioni nei criteri di rete.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS.

2. Fare doppio clic su **criteri**, fare clic su **criteri di rete**, quindi nel riquadro dei dettagli fare doppio clic sul criterio che si desidera configurare.

3. Nella finestra di dialogo **Proprietà** criterio, nella scheda **Panoramica** , in **autorizzazione di accesso**, selezionare la casella di controllo **Ignora proprietà connessione account utente** e quindi fare clic su **OK**.

### <a name="to-configure-nps-to-ignore-user-account-dial-in-properties"></a>Per configurare NPS per ignorare le proprietà di connessione remota dell'account utente



## <a name="configure-nps-for-vlans"></a>Configurare NPS per le VLAN

Con i server di accesso alla rete compatibili con VLAN e i server dei criteri di rete in Windows Server 2016, è possibile fornire gruppi di utenti con accesso solo alle risorse di rete appropriate per le autorizzazioni di sicurezza. Ad esempio, è possibile fornire ai visitatori l'accesso wireless a Internet senza consentire l'accesso alla rete aziendale. 

Inoltre, le VLAN consentono di raggruppare logicamente le risorse di rete presenti in posizioni fisiche diverse o in subnet fisiche diverse. Ad esempio, i membri del reparto vendite e le risorse di rete, ad esempio i computer client, i server e le stampanti, potrebbero trovarsi in diversi edifici dell'organizzazione, ma è possibile inserire tutte queste risorse in una VLAN che usa lo stesso IP intervallo di indirizzi. La VLAN quindi funziona, dal punto di vista dell'utente finale, come una singola subnet.

È inoltre possibile utilizzare le VLAN quando si desidera separare una rete tra diversi gruppi di utenti. Dopo aver determinato come si desidera definire i gruppi, è possibile creare gruppi di sicurezza nello snap-in Active Directory utenti e computer, quindi aggiungere membri ai gruppi.

### <a name="configure-a-network-policy-for-vlans"></a>Configurare un criterio di rete per le VLAN

È possibile utilizzare questa procedura per configurare un criterio di rete che assegna utenti a una VLAN. Quando si utilizza hardware di rete compatibile con VLAN, ad esempio router, commutatori e controller di accesso, è possibile configurare i criteri di rete per indicare ai server di accesso di inserire membri di gruppi di Active Directory specifici in VLAN specifiche. Questa possibilità di raggruppare logicamente le risorse di rete con VLAN offre flessibilità durante la progettazione e l'implementazione di soluzioni di rete.

Quando si configurano le impostazioni di un criterio di rete NPS da usare con le VLAN, è necessario configurare gli attributi **Tunnel-Medium Type**, **Tunnel-Pvt-Group-ID**, **Tunnel-Type**e **tunnel-Tag**. 

Questa procedura viene fornita come linee guida. la configurazione di rete potrebbe richiedere impostazioni diverse da quelle descritte di seguito.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-a-network-policy-for-vlans"></a>Per configurare un criterio di rete per le VLAN

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS.

2. Fare doppio clic su **criteri**, fare clic su **criteri di rete**, quindi nel riquadro dei dettagli fare doppio clic sul criterio che si desidera configurare.

3. Nella finestra di dialogo **Proprietà** criteri fare clic sulla scheda **Impostazioni** .

4. In **Proprietà**dei criteri, in **Impostazioni**, in **attributi RADIUS**, verificare che sia selezionata l'opzione **standard** .

5. Nel riquadro dei dettagli, in **attributi**, l'attributo del **tipo di servizio** viene configurato con un valore predefinito di **Framed**. Per impostazione predefinita, per i criteri con metodi di accesso VPN e connessione remota, l'attributo del **protocollo con frame** viene configurato con un valore **PPP**. Per specificare gli attributi di connessione aggiuntivi richiesti per le VLAN, fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo **Aggiungi attributo RADIUS standard** .

6. In **Aggiungi attributo RADIUS standard**, in attributi, scorrere fino a e aggiungere gli attributi seguenti:

    - **Tunnel-Media Type**. Selezionare un valore appropriato per le selezioni precedenti effettuate per il criterio. Se, ad esempio, i criteri di rete configurati sono criteri wireless, selezionare **Value: 802 (include tutti i formati di formato canonico di 802 Media Plus Ethernet)** .

    - **Tunnel-Pvt-Group-ID**. Immettere il numero intero che rappresenta il numero VLAN a cui verranno assegnati i membri del gruppo. 

    - **Tipo di tunnel**. Selezionare **LAN virtuale (VLAN)** .


7. In **Aggiungi attributo RADIUS standard**fare clic su **Chiudi**. 

8. Se il server di accesso alla rete (NAS) richiede l'uso dell'attributo **tunnel-Tag** , attenersi alla procedura seguente per aggiungere l'attributo **tunnel-Tag** ai criteri di rete. Se la documentazione del NAS non menziona questo attributo, non aggiungerla ai criteri. Se necessario, aggiungere gli attributi come indicato di seguito:

    - In **Proprietà**dei criteri, **in impostazioni**, in **attributi RADIUS**, fare clic su **specifico del fornitore**. 

    - Nel riquadro dei dettagli fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo **Aggiungi attributo specifico del fornitore** .

    - In **attributi**scorrere verso il basso e selezionare **tunnel-Tag**, quindi fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo **informazioni sugli attributi** . 

    - In **valore attributo**Digitare il valore ottenuto dalla documentazione dell'hardware.

## <a name="configure-the-eap-payload-size"></a>Configurare le dimensioni del payload EAP

In alcuni casi, i router o i firewall eliminano i pacchetti perché sono configurati per rimuovere i pacchetti che richiedono la frammentazione.

Quando si distribuisce il server dei criteri di rete con i criteri di rete che usano il protocollo di autenticazione estendibile \(EAP @ no__t-1 con Transport Layer Security \(TLS @ no__t-3 o EAP-TLS, come metodo di autenticazione, unità di trasmissione massima predefinita \(MTU @ No_ _T-5 che NPS USA per i payload EAP è 1500 byte. 

Questa dimensione massima per il payload EAP può creare messaggi RADIUS che richiedono la frammentazione da parte di un router o di un firewall tra il server dei criteri di server e un client RADIUS. In tal caso, un router o un firewall posizionato tra il client RADIUS e il server dei criteri di rete potrebbe eliminare automaticamente alcuni frammenti, causando un errore di autenticazione e l'impossibilità del client di accesso di connettersi alla rete.

Utilizzare la procedura seguente per ridurre le dimensioni massime utilizzate da server dei criteri di rete per i payload EAP modificando l'attributo Framed-MTU in un criterio di rete su un valore non maggiore di 1344.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-the-framed-mtu-attribute"></a>Per configurare l'attributo Framed-MTU

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS.
 
2. Fare doppio clic su **criteri**, fare clic su **criteri di rete**, quindi nel riquadro dei dettagli fare doppio clic sul criterio che si desidera configurare.

3. Nella finestra di dialogo **Proprietà** criteri fare clic sulla scheda **Impostazioni** .

4. In **Impostazioni**, in **attributi RADIUS**, fare clic su **standard**. Nel riquadro dei dettagli fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo **Aggiungi attributo RADIUS standard** .

5. In **attributi**scorrere verso il basso e fare clic su **Framed-MTU**, quindi fare clic su **Aggiungi**. Verrà visualizzata la finestra di dialogo **informazioni sugli attributi** .

6. In **valore attributo**Digitare un valore minore o uguale a **1344**. Fare clic su **OK**, fare clic su **Chiudi**e quindi su **OK**.



Per ulteriori informazioni sui criteri di rete, vedere [criteri di rete](nps-np-overview.md).

Per esempi di sintassi di corrispondenza dei modelli per specificare gli attributi dei criteri di rete, vedere [usare le espressioni regolari in NPS](nps-crp-reg-expressions.md).

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
