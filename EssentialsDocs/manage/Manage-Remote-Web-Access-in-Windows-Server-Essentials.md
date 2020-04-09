---
title: Gestire Accesso Web remoto in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3457eb28c05bd79f0de3a982da77ca01ffe9b52d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852764"
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Gestire Accesso Web remoto in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Accesso Web remoto in Windows Server Essentials, o in Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, offre un'esperienza di browser semplice e intuitiva per l'accesso alle applicazioni e ai dati da qualsiasi luogo in cui sia disponibile una connessione Internet e con quasi tutti i dispositivi. Per utilizzare la funzionalità Accesso Web remoto, è innanzitutto necessario attivarla mediante la Configurazione guidata di Accesso remoto via Internet e quindi configurare il router e il nome di dominio.  
  
## <a name="in-this-topic"></a>In questo argomento  
  
-   [Attivare e configurare Accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurare il router](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurare il nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personalizzare Accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Risolvere i problemi di Accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="turn-on-and-configure-remote-web-access"></a><a name="BKMK_1"></a>Attivare e configurare Accesso Web remoto  
 Negli argomenti seguenti vengono fornite informazioni sull'attivazione e la configurazione di Accesso Web remoto:  
  
-   [Panoramica di Accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Attiva Accesso Web remota](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Modificare l'area](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Gestisci autorizzazioni Accesso Web Remote](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Accesso Web remoto sicuro](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Gestire Accesso Web remoto e utenti VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="remote-web-access-overview"></a><a name="BKMK_Overview"></a>Panoramica di Accesso Web remoto  
 Quando si è fuori sede, è possibile aprire un Web browser e accedere a Accesso Web remoto da qualsiasi luogo con accesso a Internet. In Accesso Web remoto è possibile:  
  
- Accedere a cartelle e file condivisi nel server.  
  
- Accedere al server e ai computer della rete. Questo significa che è possibile accedere al desktop di un computer collegato in rete come se lo si stesse usando in ufficio.  
  
  Il Accesso Web remoto non è attivato per impostazione predefinita. Quando si esegue la procedura guidata Configura Accesso remoto via Internet, viene effettuato il tentativo di configurare il router e la connettività Internet. Una volta attivato Accesso Web remoto, è possibile configurare un nome di dominio per il server e personalizzare Accesso Web remoto. È anche possibile configurare di nuovo il router se viene sostituito.  
  
  L'autorizzazione per l'accesso Accesso Web remoto non viene concessa automaticamente quando si aggiunge un nuovo account utente. Quando si aggiunge un account utente, è possibile scegliere se consentire l'accesso alle cartelle condivise, al catalogo multimediale, ai computer, ai collegamenti della home page e al dashboard del server. È inoltre possibile specificare che un utente non è autorizzato a utilizzare Accesso Web remote.  
  
  L'impostazione Accesso Web remota viene visualizzata per ogni account utente nella scheda **utenti** del dashboard di Windows Server Essentials. Per modificare l'impostazione di Accesso Web remota, fare clic con il pulsante destro del mouse sull'account utente, quindi scegliere **Visualizza proprietà account**.  
  
###  <a name="turn-on-remote-web-access"></a><a name="BKMK_TurnOnRWA"></a>Attiva Accesso Web remota  
 È possibile attivare la funzionalità Accesso Web remoto mediante l'esecuzione della Configurazione guidata di Accesso remoto via Internet dal dashboard del server.  
  
##### <a name="to-turn-on-remote-web-access"></a>Per attivare Accesso Web remoto  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **Impostazioni**, quindi fare clic sulla scheda **Accesso remoto via Internet**.  
  
3.  Fare clic su **Configura**. Verrà visualizzata la Configurazione guidata di Accesso remoto via Internet.  
  
4.  Nella pagina **Scegli le funzionalità di Accesso remoto via Internet da abilitare** selezionare la casella di controllo **Accesso Web remoto**.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
###  <a name="change-your-region"></a><a name="BKMK_Region"></a>Modificare l'area  
 Per cambiare l'impostazione dell'area geografica in Windows Server Essentials, è necessario essere un amministratore di rete.  
  
##### <a name="to-change-the-region-setting"></a>Per cambiare l'impostazione dell'area geografica  
  
1.  In un computer connesso a Windows Server Essentials aprire il dashboard.  
  
2.  Fare clic su **Impostazioni**.  
  
3.  Nella scheda **Generale** fare clic sull'elenco a discesa della sezione **Paese/Regione del server**.  
  
4.  Selezionare la nuova area geografica nell'elenco a discesa e quindi fare clic su **Applica** per accettare la nuova impostazione.  
  
###  <a name="manage-remote-web-access-permissions"></a><a name="BKMK_ManagePerms"></a>Gestisci autorizzazioni Accesso Web Remote  
 Quando si aggiunge un account utente in Windows Server Essentials, per impostazione predefinita il nuovo utente è autorizzato a usare Accesso Web remoto. Se si sceglie di non consentire Accesso Web remoto per un account utente e quindi si scopre che l'utente deve usare Accesso Web remoto, è possibile aggiornare le proprietà dell'account utente.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Per gestire le autorizzazioni di Accesso Web remoto per un account utente  
  
1. Accedere al dashboard, quindi fare clic su **Utenti**.  
  
2. Fare clic sull'account utente da gestire, quindi su **Visualizza proprietà account** nel riquadro **Attività**.  
  
3. Nella finestra di dialogo **Proprietà** fare clic sulla scheda **Accesso remoto via Internet**.  
  
4. Nella scheda **Accesso remoto via Internet** selezionare la casella di controllo **Consenti Accesso Web remoto e accedi ai servizi Web** per consentire a un utente di connettersi al server tramite Accesso Web remoto.  
  
5. Fare clic su **Applica** e quindi su **OK**.  
  
   Per altre informazioni, vedere [gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="secure-remote-web-access"></a><a name="BKMK_SecureRWA"></a>Accesso Web remoto sicuro  
 Windows Server Essentials usa un certificato di sicurezza per proteggere le informazioni scambiate tra il software e un Web browser. Quando si installa il software Connettore nei computer, il certificato di sicurezza di Windows Server Essentials viene aggiunto all'elenco di certificati attendibili nel computer. Il modo migliore per accedere ad Accesso Web remoto da fuori sede consiste nell'usare un computer portatile in cui è installato il software Connettore.  
  
> [!WARNING]
>  Gli utenti che usano Accesso Web remoto da luoghi pubblici o da altri computer non attendibili dovrebbero assicurarsi di disconnettersi dal sito Web prima di lasciare il computer incustodito o quando hanno concluso la sessione.  
  
###  <a name="manage-remote-web-access-and-vpn-users"></a><a name="BKMK_ManageRWAVPN"></a>Gestire Accesso Web remoto e utenti VPN  
 È possibile usare una VPN per connettersi a Windows Server Essentials e accedere a tutte le risorse archiviate nel server. Ciò è particolarmente utile se si usa un computer client configurato con account di rete che possono essere usati per connettersi a un server Windows Server Essentials ospitato tramite una connessione VPN. Tutti i nuovi account utente creati nel server Windows Server Essentials ospitato devono usare la VPN per accedere al computer client per la prima volta.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Per impostare le autorizzazioni per la VPN e per Accesso Web remoto per gli utenti di rete  
  
1.  Aprire il Dashboard.  
  
2.  Sulla barra di spostamento fare clic su **UTENTI**.  
  
3.  Nell'elenco di account utente selezionare quello a cui si vogliono concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel riquadro **attività < account utente\>** fare clic su **Proprietà**.  
  
5.  In **< account utente\> proprietà**, fare clic sulla scheda **accesso remoto via Internet** .  
  
6.  Nella scheda **Accesso remoto via Internet** eseguire le operazioni seguenti:  
  
    1.  Per permettere a un utente di connettersi al server tramite la VPN, selezionare la casella di controllo **Consenti rete privata virtuale (VPN)** .  
  
    2.  Per permettere a un utente di connettersi al server tramite Accesso Web remoto, selezionare la casella di controllo **Consenti Accesso Web remoto e accedi ai servizi Web**.  
  
7.  Fare clic su **Applica** e quindi su **OK**.  
  
##  <a name="set-up-your-router"></a><a name="BKMK_2"></a>Configurare il router  
 Quando si configura il server per Accesso Web remoto, la procedura guidata Configura Accesso remoto via Internet tenta di configurare il router. Se si cambia il router o si modificano le impostazioni sul router, è necessario eseguire di nuovo la procedura guidata Configura router. Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [Configurare il router](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Sostituire un router](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Percorso di rete definito](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Abilita Servizi Desktop remoto controlli ActiveX](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="set-up-your-router"></a><a name="BKMK_SetUpRouter"></a>Configurare il router  
 In questo passaggio Windows Server Essentials tenta di configurare automaticamente il router in uso mediante i comandi UPnP. A questo scopo, il router in uso deve supportare gli standard UPnP e l'impostazione UPnP deve essere abilitata nel router.  
  
> [!NOTE]
>  La configurazione della rete deve essere conforme ai requisiti di rete supportati per Windows Server Essentials. Nella rete deve essere presente un solo router.  
  
 Se il router non viene configurato dalla procedura guidata Imposta nome di dominio, è necessario eseguire manualmente l'inoltro della porta 443. Per altre informazioni su come impostare il port forwarding nel router, vedere [Configurazione del router](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="replace-a-router"></a><a name="BKMK_ReplaceRouter"></a>Sostituire un router  
 Sostituire il router in base alle istruzioni del produttore e quindi eseguire la procedura guidata Configura router per configurare il nuovo router.  
  
##### <a name="to-set-up-your-new-router"></a>Per configurare il nuovo router  
  
1.  Nel dashboard di Windows Server Essentials fare clic su **Impostazioni**.  
  
2.  Fare clic sulla scheda **Accesso remoto via Internet** e quindi, nella sezione **Router**, fare clic su **Configura**. Verrà avviata la procedura guidata Configura router.  
  
3.  Seguire le istruzioni della procedura guidata per completare la configurazione del nuovo router.  
  
###  <a name="network-location-defined"></a><a name="BKMK_NetworkLocation"></a>Percorso di rete definito  
 Un percorso di rete è un insieme di impostazioni di rete applicate da Windows quando ci si connette a una rete. Le impostazioni variano e possono essere personalizzate in base al tipo di rete in uso. Le impostazioni di un percorso di rete determinano se specifiche funzionalità, ad esempio la condivisione di file e stampanti, l'individuazione della rete e la condivisione di cartelle pubbliche, sono attivate o disattivate. I percorsi di rete sono utili quando è necessario connettersi a reti diverse.  
  
 Si supponga ad esempio di avere un computer portatile che si usa a casa e al lavoro. In ufficio ci si connette alla rete dell'ufficio. A casa, invece, si usa il portatile per accedere e riprodurre video e musica archiviati nel server domestico. Quando ci si connette a una nuova rete e si specifica il tipo di posizione, Windows assegna un profilo di rete preimpostato per questo tipo di posizione. La volta successiva che ci si connette alla stessa rete, Windows la riconosce e assegna automaticamente le impostazioni corrette. In questo modo viene aggiunto un livello di sicurezza per proteggere le informazioni del computer e vengono attivate solo le funzionalità di rete necessarie per quella posizione.  
  
 Ci sono quattro tipi di posizioni di rete:  
  
-   **Rete domestica** Scegliere questa impostazione per le reti domestiche o quando si conoscono e si considerano attendibili le persone e i dispositivi connessi alla rete. I computer di una rete domestica possono appartenere a un gruppo home. L'individuazione di rete è attivata per le reti domestiche, consentendo all'utente di vedere altri computer e dispositivi della rete e ad altri utente di rete di vedere il suo computer.  
  
-   **Rete aziendale** Scegliere questa rete per piccoli uffici o altre reti dell'ambiente di lavoro. L'individuazione della rete, che consente all'utente di vedere altri computer e dispositivi di una rete e ad altri utenti di rete di vedere il suo computer, è attivata per impostazione predefinita, ma non è possibile creare o partecipare a un gruppo home.  
  
-   **Rete pubblica** Scegliere questa rete per luoghi pubblici, ad esempio bar o aeroporti. Questa posizione è progettata per evitare che il computer sia visibile ad altri computer e per contribuire a proteggerlo da software dannoso proveniente da Internet. Il gruppo home non è disponibile per reti pubbliche e l'individuazione della rete è disattivata. È anche consigliabile scegliere questa opzione se si è connessi direttamente a Internet senza un router oppure se si ha una connessione a banda larga mobile.  
  
-   **Dominio** Scegliere questa rete per domini come le aree di lavoro aziendali. Questo tipo di posizione di rete è controllato dall'amministratore di rete e non può essere selezionato né modificato.  
  
###  <a name="enable-remote-desktop-services-activex-controls"></a><a name="BKMK_ActiveX"></a>Abilita Servizi Desktop remoto controlli ActiveX  
 I controlli ActiveX Servizi Desktop remoto consentono di accedere al computer domestico o aziendale, tramite Internet, da un altro computer utilizzando Accesso Web remoto.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Per abilitare i controlli ActiveX di Servizi Desktop remoto  
  
1.  In Internet Explorer fare clic su **Strumenti** e quindi su **Opzioni Internet**.  
  
2.  Nella scheda **Sicurezza** fare clic su **Livello personalizzato**.  
  
3.  Nella sezione **Controlli ActiveX e plug-in** eseguire le operazioni seguenti:  
  
    1.  In **Scarica controlli ActiveX con firma elettronica** fare clic su **Chiedi conferma**.  
  
    2.  In **Esegui controlli ActiveX e plug-in** fare clic su **Attiva**.  
  
4.  Fare clic su **OK** due volte per accettare le modifiche e chiudere la finestra di dialogo.  
  
##  <a name="set-up-your-domain-name"></a><a name="BKMK_3"></a>Configurare il nome di dominio  
 Dopo l'attivazione di Accesso Web remoto, è possibile configurare un nome di dominio per il server che esegue Windows Server Essentials. Si tratta di un'operazione necessaria se si prevede di utilizzare Accesso Web remoto da un computer remoto. Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [Panoramica dei nomi di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Informazioni sui nomi di dominio personalizzati Microsoft](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Usa un nome di dominio nuovo o esistente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurare un nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Scegliere un provider di servizi di nomi di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Scegliere un nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Scegliere un prefisso del nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Scegliere un'estensione del nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Aggiornare o aggiornare il servizio dei nomi di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Esportare o importare il certificato nel server](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurare manualmente un nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Trovare il provider di servizi dei nomi di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="domain-names-overview"></a><a name="BKMK_DNOverview"></a>Panoramica dei nomi di dominio  
 Il nome di dominio identifica in modo univoco il server su Internet. I nomi di dominio sono costituiti da almeno due parti, ossia un nome di dominio di primo livello e un nome di dominio di secondo livello. Ad esempio, in contoso.com, com è il TLD e Contoso è il nome di dominio di secondo livello.  
  
 Da fuori sede, è possibile usare il nome di dominio per accedere ai file condivisi nel server o nei computer della rete, nonché gestire il server. È ad esempio possibile registrare contoso.com per il server. Quando si è fuori sede, è possibile aprire un Web browser nel portatile e digitare **contoso.com** nella casella di testo dell'indirizzo per connettersi all'istanza di Accesso Web remoto configurata in Windows Server Essentials.  
  
###  <a name="understand-microsoft-personalized-domain-names"></a><a name="BKMK_PersonalizedNames"></a>Informazioni sui nomi di dominio personalizzati Microsoft  
 Un nome di dominio personalizzato Microsoft include le funzionalità seguenti:  
  
- Nome di dominio personalizzato per Accesso Web remoto, ad esempio *nomepropriohost*. remotewebaccess.com. Il nome di dominio è associato al proprio indirizzo IP pubblico.  
  
- Un servizio del protocollo di aggiornamento dinamico DNS in modo che Accesso Web remoto che usa il nome di dominio non venga interrotto se l'indirizzo IP pubblico cambia. In genere, i provider di servizi Internet (ISP) per le connessioni a banda larga dell'organizzazione forniscono indirizzi IP pubblici dinamici che possono cambiare.  
  
- Un certificato attendibile associato la nome di dominio.  
  
  Per integrare un nome di dominio personalizzato Microsoft con il server, è necessario un account Microsoft (in precedenza Windows Live ID). Se non si ha un account Microsoft, è possibile iscriversi per riceverne uno nel sito Web [Microsoft Hotmail](https://login.live.com/) .  
  
> [!IMPORTANT]
>  Windows Live consente l'uso di caratteri speciali nella password dell'account Microsoft che non sono supportati dal server. Se si usa un dominio personalizzato Microsoft, assicurarsi che la password dell'account Microsoft contenga solo caratteri supportati dal server. Il server non supporta l'uso dei caratteri $, /, ' e %.  
  
###  <a name="use-a-new-or-existing-domain-name"></a><a name="BKMK_UseNewName"></a>Usa un nome di dominio nuovo o esistente  
 Per configurare automaticamente il nome di dominio in un server che esegue Windows Server Essentials, è necessario usare un provider di servizi di nomi di dominio elencato nella procedura guidata Imposta nome di dominio. Si può scegliere di ottenere un nuovo nome di dominio o di usarne uno esistente. Esegui una delle operazioni seguenti:  
  
-   Se si vuole ottenere un nuovo nome di dominio da uno dei provider di servizi di nomi di dominio elencati nella procedura guidata, fare clic su **Desidero impostare un nuovo nome di dominio**.  
  
-   Se si ha già un nome di dominio acquistato da uno dei provider di servizi di nomi di dominio supportati, è possibile usare la procedura guidata Imposta nome di dominio per configurare il nome di dominio per il server. Fare clic su **Desidero utilizzare un nome di dominio già di mia proprietà** e quindi immettere un nome di dominio nella casella di testo **Imposta nome di dominio**. È necessario specificare il nome utente e la password usati per acquistare il nome di dominio.  
  
-   Se si ha già un nome di dominio acquistato da un provider di servizi di nomi di dominio non supportato da Windows Server Essentials e si vuole usare la procedura guidata Imposta nome di dominio per il server, è possibile trasferire il nome di dominio in uno del provider di servizi di nomi di dominio elencati nella procedura guidata. Fare clic su **desidero utilizzare un nome di dominio di cui si è già proprietari**, digitare il nome di dominio nella casella di testo **nome dominio** e quindi seguire le istruzioni nel sito Web del provider di servizi dei nomi di dominio per trasferire il nome di dominio.  
  
###  <a name="set-up-a-domain-name"></a><a name="BKMK_SetUpName"></a>Configurare un nome di dominio  
 Quando si attiva Accesso Web remoto, è possibile scegliere di configurare il nome di dominio Internet del server.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Per configurare e gestire un nome di dominio Internet  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **Impostazioni server** e quindi sulla scheda **Accesso remoto via Internet**.  
  
3.  Nella sezione **Nome dominio** fare clic su **Configura**.  
  
4.  Seguire le istruzioni per completare la procedura guidata. Se non si dispone già di un nome di dominio e di un certificato, la procedura guidata consente di trovare un provider di nomi di dominio da cui acquistarli. In alternativa, è possibile ottenere un nome di dominio Microsoft personalizzato.  
  
###  <a name="choose-a-domain-name-service-provider"></a><a name="BKMK_ChooseProvider"></a>Scegliere un provider di servizi di nomi di dominio  
 È consigliabile scegliere un provider di servizi di nomi di dominio che supporta l'estensione del nome di dominio che si vuole usare. La procedura guidata imposta nome di dominio include un elenco di provider qualificati che è possibile usare con un collegamento al sito Web di ogni provider. Fare clic sul collegamento **altre informazioni** accanto al nome di ogni provider per ottenere informazioni sui servizi e sui prezzi offerti dal provider.  
  
> [!NOTE]
>  Alcuni provider di servizi di nomi di dominio si rivolgono a un mercato internazionale che copre ampie aree geografiche, mentre altri servono mercati più piccoli. Per questo motivo, alcuni provider potrebbero non offrire un sito Web tradotto nella propria lingua preferita.  
  
 Quando si acquista il nome di dominio, è inoltre consigliabile acquistare il servizio del protocollo di aggiornamento dinamico del DNS (Domain Name System) dal provider di servizi di nomi di dominio. Si tratta di un servizio che consente a chiunque sia connesso a Internet di accedere alle risorse di una rete locale quando l'indirizzo IP di tale rete cambia continuamente. In alternativa, è possibile acquistare un indirizzo IP statico dal provider di servizi Internet per assicurarsi che il proprio indirizzo IP non cambi.  
  
###  <a name="choose-a-domain-name"></a><a name="BKMK_ChooseDomainName"></a>Scegliere un nome di dominio  
 Scegliere un nome che identifica in modo univoco il server aziendale. Se ad esempio il nome dell'organizzazione è Contoso Ltd, è possibile scegliere Contoso per identificare in modo univoco il server di casa o aziendale su Internet. Se il nome di dominio non è disponibile, provare con un'altra variante oppure con qualcosa di completamente diverso.  
  
 Il nome immesso può contenere quanto segue:  
  
-   Massimo 63 caratteri.  
  
-   Lettere (caratteri in inglese o localizzati), numeri o trattini (-). Il nome deve iniziare e finire con una lettera o con un numero.  
  
    > [!NOTE]
    >  Per i nomi di dominio non si fa distinzione tra maiuscole e minuscole.  
  
###  <a name="choose-a-domain-name-prefix"></a><a name="BKMK_Prefixes"></a>Scegliere un prefisso del nome di dominio  
 Un nome di dominio è costituito da etichette gerarchiche.  
  
 **L'estensione di dominio di primo livello** è l'etichetta all'estrema destra del nome di dominio. Ad esempio, in www\.contoso.com, com è l'estensione del nome di dominio di primo livello.  
  
 **Il nome di dominio di secondo livello** è l'etichetta accanto all'estensione di dominio di primo livello. Il nome di dominio di secondo livello viene spesso creato in base al nome, ai prodotti o ai servizi della società. Ad esempio, in www\.contoso.com, Contoso è il nome di dominio di secondo livello ed è stato scelto per il nome della società Contoso Pharmaceuticals. Il dominio di secondo livello viene a volte chiamato nome host, a cui è associato un indirizzo IP.  
  
 **Il prefisso del nome di dominio** identifica un sottodominio. Il nome del sottodominio può essere usato per identificare servizi, dispositivi o aree geografiche. Ad esempio, Contoso Pharmaceuticals vuole consentire agli utenti remoti di accedere ad Accesso Web remoto ma non vuole che il sito Web sia disponibile al pubblico, quindi crea un sottodominio che permette l'accesso al sito Web solo agli utenti dotati di apposite autorizzazioni. Contoso Pharmaceuticals configura remote.contoso.com come sottodominio, dove remote è il prefisso del nome di dominio.  
  
> [!TIP]
>  È consigliabile usare l'impostazione predefinita **Remote** come prefisso del nome di dominio.  
  
###  <a name="choose-a-domain-name-extension"></a><a name="BKMK_Extension"></a>Scegliere un'estensione del nome di dominio  
 Quando si sceglie un nome di dominio per il sito Web, è anche necessario specificare l'estensione da usare, identificata dalle lettere che seguono il punto finale di un nome di dominio. (Il termine formale per l'estensione è il dominio di primo livello o il TLD).  
  
 Ci sono due tipi principali di estensioni di dominio che è possibile usare, generico e con codice paese.  
  
#### <a name="generic-top-level-domains"></a>Domini di primo livello generici  
 Le estensioni di dominio generiche sono costituite da almeno tre lettere e vengono in genere usate in specifici tipi di organizzazione.  
  
 **Esempi di domini di primo livello generici**  
  
|Estensione di dominio|Descrizione|  
|----------------------|-----------------|  
|.com|Usata in genere da organizzazioni commerciali, ma in realtà può essere usata da chiunque.|  
|.net|Destinata a organizzazioni che offrono servizi di infrastruttura di rete.|  
|.org|In origine usata da enti no profit e altre organizzazioni che non rientrano in un'altra categoria di dominio di primo livello generico. Può essere usata da chiunque.|  
|.edu|Può essere usata solo da organizzazioni didattiche.|  
  
#### <a name="country-code-top-level-domains"></a>Domini di primo livello con codice paese  
 Queste estensioni di dominio, costituite da due lettere, sono destinate a organizzazioni che operano nel paese o nell'area geografica associata al codice. Alcuni domini di primo livello con codice paese possono essere usati solo dai cittadini di quel paese o area geografica. Altri sono disponibili per tutti.  
  
 **Esempi di domini di primo livello con codice paese**  
  
|Estensione di dominio|Descrizione|  
|----------------------|-----------------|  
|.ca|Da usare per siti Web in Canada|  
|.cn|Da usare per siti Web in Cina|  
|.de|Da usare per siti Web in Germania|  
|.co.uk|Da usare per siti Web nel Regno Unito|  
  
 Per l'elenco completo di domini di primo livello, vedere il [sito Web di Internet Assigned Numbers Authority](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Estensione di dominio non disponibile per la scelta nella procedura guidata Imposta nome di dominio  
 Quando si esegue la procedura guidata Imposta nome di dominio, vengono esaminate le informazioni di sistema per determinare il paese o l'area geografica dell'utente corrente. La procedura guidata visualizza quindi solo le estensioni di dominio supportate dai provider partecipanti nell'area. Se l'estensione di dominio che si vuole scegliere non è riportata nell'elenco, è necessario selezionarne un'altra per continuare. Selezionare un'estensione nell'elenco restituito dalla procedura guidata.  
  
###  <a name="update-or-upgrade-your-domain-name-service"></a><a name="BKMK_UpdateService"></a>Aggiornare o aggiornare il servizio dei nomi di dominio  
 Può essere necessario aggiornare il servizio dei nomi di dominio se è stato acquistato un nome di dominio ma non un certificato. È necessario ricevere un certificato per il nome di dominio dal provider di servizi di nomi di dominio.  
  
> [!NOTE]
>  Rivolgersi al provider di servizi di nomi di dominio per determinare il tipo di certificato necessario, che può essere uno dei certificati meno costosi offerti. Tuttavia, è consigliabile consultare la documentazione e le funzionalità dei certificati di sicurezza di livello più alto per determinare se rispondono maggiormente alle proprie esigenze aziendali.  
  
###  <a name="export-or-import-your-certificate-on-your-server"></a><a name="BKMK_ExportCert"></a>Esportare o importare il certificato nel server  
 Se si vuole creare una copia di backup di un certificato o usarlo in un altro server, è necessario esportarlo. Per informazioni sull'esportazione di certificati, vedere [Esportare un certificato](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="set-up-a-domain-name-manually"></a><a name="BKMK_SetNameManually"></a>Configurare manualmente un nome di dominio  
 Se si sceglie questa opzione, il server non esegue il monitoraggio e la manutenzione del nome di dominio e non invia alcun avviso qualora si verifichi un problema di configurazione. È consigliabile scegliere questa opzione nei casi seguenti:  
  
- Se non sono presenti provider di nomi di dominio partner per il proprio paese o regione.  
  
- Se i provider di domini partner disponibili non supportano l'estensione del nome di dominio in uso.  
  
- Se si ha un nome di dominio esistente fornito da un provider di nomi di dominio non partner e non si vuole trasferire tale nome di dominio a un provider di nomi di dominio supportati da Windows Server Essentials.  
  
- Se nella procedura guidata non sono disponibili estensioni dii nomi di dominio che si desidera utilizzare e l'estensione desiderata è disponibile presso un provider di nomi di dominio non partner.  
  
  Se si sceglie di configurare manualmente il nome di dominio, collaborare con il provider di servizi di nomi di dominio per creare un record A per il dominio.  
  
##### <a name="to-create-an-a-record"></a>Per creare un record A  
  
1.  Decidere un nome host, ad esempio remoto. Si tratta del prefisso del nome di dominio. Il prefisso del nome di dominio e il nome di dominio definiranno l'URL per aprire la pagina di accesso Accesso Web remoto; ad esempio, **http://remote.contoso.com** .  
  
2.  Nel dashboard di configurazione dei provider di servizi dei nomi di dominio, in genere nella relativa pagina Web, creare il record A per il nome host scelto nel passaggio 1. Verificare che l'indirizzo IP specificato nel record A sia l'indirizzo IP sul lato WAN del router (lato Internet). Per individuare l'indirizzo IP della WAN, consultare la documentazione fornita con il router.  
  
3.  È consigliabile contattare il proprio provider di servizi Internet per acquistare un indirizzo IP statico per la rete in uso. Questa soluzione garantisce che l'indirizzo IP non cambi e che la voce del DNS sia sempre aggiornata.  
  
     Se non è possibile ottenere un indirizzo IP statico dal proprio provider di servizi Internet, valutare la possibilità di acquistare un servizio di protocollo di aggiornamento dinamico del DNS dal provider di servizi dei nomi di dominio o da un altro provider di servizi. Il protocollo di aggiornamento dinamico del DNS è un servizio che mantiene l'indirizzo IP WAN della rete aggiornato in modo che venga risolto nel nome di dominio in uso anche se cambia.  
  
4.  Quando la procedura guidata lo richiede, importare un certificato attendibile. Se non si dispone di un certificato attendibile, è possibile richiederne uno a uno dei provider di nomi di dominio elencati nella procedura guidata o acquistarne uno dal provider attendibile desiderato. Per ulteriori informazioni sui certificati attendibili, rivolgersi al proprio provider di nomi di dominio.  
  
###  <a name="find-your-domain-name-service-provider"></a><a name="BKMK_Find"></a>Trovare il provider di servizi dei nomi di dominio  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Per trovare il provider di servizi del proprio nome di dominio  
  
1. Aprire un Web browser e digitare <strong>www.internic.com</strong> nella barra degli indirizzi per passare alla home page Internic.  
  
2. Nella home page Internic fare clic su **Whois**.  
  
3. Nella casella **Whois** immettere il nome di dominio, ad esempio contoso.com.  
  
4. Fare clic sull'opzione **Domain** e quindi su **Submit**.  
  
5. Nei risultati della ricerca il nome del provider di servizi di nomi di domino è elencato sotto **Registrar**.  
  
##  <a name="customize-remote-web-access"></a><a name="BKMK_4"></a>Personalizzare Accesso Web remoto  
 È possibile personalizzare il sito Accesso Web remoto mediante l'aggiunta di un logo personale o di un'immagine di sfondo. È inoltre possibile aggiungere collegamenti alla pagina iniziale per rendere disponibili le informazioni per tutti gli utenti. Per altre informazioni, vedere gli argomenti seguenti:  
  
-   [Personalizzare Accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personalizzare le immagini per sfondi e logo](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Ripristina Accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="customize-remote-web-access"></a><a name="BKMK_CustomizeRWA"></a>Personalizzare Accesso Web remoto  
 È possibile personalizzare Accesso Web remoto cambiando il titolo del sito Web, l'immagine di sfondo e il logo e aggiungendo collegamenti ad altri siti Web nella home page.  
  
##### <a name="to-customize-remote-web-access"></a>Per personalizzare Accesso Web remoto  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **Impostazioni**, quindi fare clic sulla scheda **Accesso remoto via Internet**.  
  
3.  Nella sezione **Impostazioni sito Web** fare clic su **Personalizza**.  
  
4.  Al termine, fare clic su **OK**. Testare le modifiche apportate ad Accesso Web remoto.  
  
###  <a name="customize-images-for-backgrounds-and-logos"></a><a name="BKMK_CustomizeImages"></a>Personalizzare le immagini per sfondi e logo  
 Questa sezione fornisce informazioni sulle immagini che è possibile usare per personalizzare Accesso Web remoto.  
  
#### <a name="image-size"></a>Dimensioni immagine  
 **Immagini logo**  
  
 È consigliabile usare immagini di logo di 32x32 pixel. Le immagini più grandi vengono ridotte a 32x32 e quelle più piccole vengono allargate, con una possibile distorsione dell'immagine.  
  
 **Immagini di sfondo**  
  
 Anche se non sono previsti limiti per le dimensioni delle immagini di sfondo, per risultati ottimali è consigliabile usare immagini approssimativamente di 800x500 pixel. L'immagine di sfondo viene posizionata al centro (in orizzontale e in verticale) della pagina di accesso. Per rendere leggibile il testo nella pagina di accesso, il centro dell'immagine di sfondo dovrebbe essere di un colore chiaro.  
  
#### <a name="image-file-types"></a>Tipi di file di immagine  
 È possibile usare i tipi di file di immagine seguenti per sostituire lo sfondo predefinito e il logo del sito Web:  
  
-   Bitmap (*. bmp, \*. DIB, \*. RLE)  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="repair-remote-web-access"></a><a name="BKMK_RepairRWA"></a>Ripristina Accesso Web remoto  
 La procedura guidata di ripristino consente di individuare e risolvere i problemi del router o del nome di dominio. È possibile individuare in due modi i problemi di Accesso Web remoto:  
  
-   In Impostazioni server nel dashboard, nella scheda Accesso remoto via Internet, viene visualizzata un'icona con una X rossa e una descrizione del problema.  
  
-   Viene riportato un avviso nel Visualizzatore avvisi.  
  
> [!NOTE]
>  La procedura guidata di ripristino è disponibile solo dopo l'attivazione di Accesso Web remoto. Per informazioni sull'attivazione di Accesso Web remoto, vedere [Attivare Accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Per ripristinare Accesso Web remoto  
  
1.  Accedere al dashboard.  
  
2.  Fare clic su **Impostazioni**, quindi fare clic sulla scheda **Accesso remoto via Internet**.  
  
3.  Fare clic su **Ripristina**. Verrà avviata la procedura guidata **Ripristina Accesso Web remoto**.  
  
4.  Fare clic su **Avanti**. La procedura guidata analizza Accesso Web remoto, identifica il problema e quindi tenta di risolverlo.  
  
5.  Se al termine della procedura guidata viene visualizzato un avviso, è possibile fare clic su **Riprova** per tentare di nuovo di risolvere il problema. Se l'avviso continua a essere visualizzato, verificarne il contenuto per altre informazioni sul problema e le procedure di risoluzione.  
  
##  <a name="troubleshoot-remote-web-access"></a><a name="BKMK_5"></a>Risolvere i problemi di Accesso Web remoto  
  
-   [Risolvere i problemi di connettività Accesso Web remota](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Risolvere i problemi del firewall](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Risolvere i problemi di accesso remoto via Internet](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Opzioni desktop remoto](Remote-desktop-options.md)  
  
-   [USA Accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
