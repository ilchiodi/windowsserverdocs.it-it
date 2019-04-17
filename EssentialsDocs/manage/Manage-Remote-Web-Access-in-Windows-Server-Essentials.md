---
title: Gestire accesso Web remoto in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: de01b2fd2395377b6e7b3349b9862eb0e51a59b8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Gestire accesso Web remoto in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Accesso Web remoto in Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza di Windows Server Essentials installato, offre un'esperienza di browser semplice tocco per l'accesso alle applicazioni e dati da qualunque luogo che sia disponibile una connessione Internet e con qualsiasi dispositivo. Per utilizzare la funzionalità accesso Web remoto, è necessario innanzitutto attivare tramite la configurazione ovunque guidata di accesso e quindi configurare il router e il nome di dominio.  
  
## <a name="in-this-topic"></a>In questo argomento  
  
-   [Attivare e configurare accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurare il router](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Impostare il nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personalizzare accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Risolvere i problemi di accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a>Attivare e configurare accesso Web remoto  
 Gli argomenti seguenti consentono di attivare e configurare accesso Web remoto:  
  
-   [Panoramica di accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Attivare accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Modificare l'area geografica](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Gestire le autorizzazioni di accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Accesso Web remoto sicuro](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Gestire gli utenti di accesso Web remoto e VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a>Panoramica di accesso Web remoto  
 Quando si è fuori sede, è possibile aprire un web browser e accesso Web remoto da qualsiasi posizione con accesso a Internet. In accesso Web remoto, è possibile:  
  
-   Accedere a file condivisi e cartelle sul server.  
  
-   Accedere al server e computer di rete. Ciò significa che è possibile accedere al desktop di un computer in rete come se stesse usando in ufficio.  
  
  Accesso Web remoto non è attivato per impostazione predefinita. Quando si esegue la configurazione guidata accesso remoto via Internet, la procedura guidata tenta di configurare il router e connettività Internet. Dopo aver attivato accesso Web remoto, è possibile impostare un nome di dominio per il server e personalizzare accesso Web remoto. È possibile inoltre configurare il router nuovamente se si modifica il router.  
  
 Autorizzazione di accesso Web remoto non viene concessa automaticamente quando si aggiunge un nuovo account utente. Quando si aggiunge un account utente, è possibile scegliere di consentire l'accesso alle cartelle condivise, il catalogo multimediale, computer, collegamenti della Home page e al Dashboard del server. È anche possibile specificare che un utente non è autorizzato a usare accesso Web remoto.  
  
 Viene visualizzata l'impostazione di accesso Web remoto per ogni account utente nel **utenti** scheda del Dashboard di Windows Server Essentials. Per modificare l'impostazione di accesso Web remoto, fare doppio clic su account utente, quindi fare clic su **Visualizza proprietà account**.  
  
###  <a name="BKMK_TurnOnRWA"></a>Attivare accesso Web remoto  
 È possibile attivare accesso Web remoto, eseguire la configurazione guidata accesso remoto via Internet dal Dashboard del server.  
  
##### <a name="to-turn-on-remote-web-access"></a>Per attivare accesso Web remoto  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **impostazioni**, quindi fare clic su di **accesso remoto via Internet** scheda.  
  
3.  Fare clic su **configurare**. Verrà visualizzata la configurazione ovunque guidata di accesso.  
  
4.  Nel **funzionalità scegliere accesso remoto via Internet da abilitare** pagina, selezionare il **accesso Web remoto** casella di controllo.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
###  <a name="BKMK_Region"></a>Modificare l'area geografica  
 È necessario essere un amministratore di rete per modificare l'impostazione dell'area geografica in Windows Server Essentials.  
  
##### <a name="to-change-the-region-setting"></a>Per modificare l'impostazione dell'area geografica  
  
1.  In un computer connesso a Windows Server Essentials, aprire il Dashboard.  
  
2.  Fare clic su **impostazioni**.  
  
3.  Nel **generale** scheda, fare clic sull'elenco a discesa nel **paese/regione del server** sezione.  
  
4.  Dall'elenco a discesa, selezionare la nuova area e quindi fare clic su **applica** per accettare la nuova impostazione.  
  
###  <a name="BKMK_ManagePerms"></a>Gestire le autorizzazioni di accesso Web remoto  
 Quando si aggiunge un account utente in Windows Server Essentials, il nuovo utente è consentito per impostazione predefinita usare accesso Web remoto. Se si sceglie di non consentire l'accesso Web remoto per un account utente e quindi individuare l'utente deve usare accesso Web remoto, è possibile aggiornare le proprietà s dell'account utente.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Per gestire le autorizzazioni di accesso Web remoto per un account utente  
  
1.  Accedere al Dashboard, quindi fare clic su **utenti**.  
  
2.  Fare clic sull'account utente che si desidera gestire, quindi fare clic su **Visualizza proprietà account** nel **attività** riquadro.  
  
3.  Nel **proprietà** la finestra di dialogo, fare clic su di **accesso remoto via Internet** scheda.  
  
4.  Nel **accesso remoto via Internet**, selezionare il **Consenti accesso Web remoto e Accedi ai servizi Web** casella di controllo per consentire all'utente di connettersi al server tramite accesso Web remoto.  
  
5.  Fare clic su **applica**, quindi fare clic su **OK**.  
  
 Per ulteriori informazioni, vedere [gestire gli account utente](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecureRWA"></a>Accesso Web remoto sicuro  
 Windows Server Essentials Usa un certificato di sicurezza per proteggere le informazioni scambiate tra il software e un web browser. Quando si installa il software connettore nei computer, il certificato di sicurezza per Windows Server Essentials viene aggiunto all'elenco di certificati attendibili nel computer. Il modo migliore per gli utenti di accesso Web remoto quando sono fuori sede consiste nell'usare un computer portatile che ha installato il software connettore.  
  
> [!WARNING]
>  Gli utenti che usano accesso Web remoto da luoghi pubblici o ad altri computer non attendibili dovrebbero assicurarsi che essi disconnettersi dal sito Web prima di lasciare il computer incustodito o quando hanno concluso la sessione.  
  
###  <a name="BKMK_ManageRWAVPN"></a>Gestire gli utenti di accesso Web remoto e VPN  
 È possibile utilizzare VPN per connettersi a Windows Server Essentials e accedere a tutte le risorse che sono archiviate nel server. Ciò è particolarmente utile se si dispone di un computer client che viene configurato con account di rete che può essere utilizzato per connettersi a un server Windows Server Essentials ospitato tramite una connessione VPN. Tutti gli account utente appena creati nel server Windows Server Essentials ospitato devono usare VPN per accedere al computer client per la prima volta.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Per impostare le autorizzazioni di accesso Web remoto e VPN per gli utenti di rete  
  
1.  Aprire il Dashboard.  
  
2.  Sulla barra di spostamento, fare clic su **utenti**.  
  
3.  Nell'elenco di account utente, selezionare l'account utente che si desidera concedere le autorizzazioni per accedere al desktop in remoto.  
  
4.  Nel **attività < utente Account\ >** riquadro, fare clic su **proprietà**.  
  
5.  In **< utente Account\ > proprietà**, fare clic su di **accesso remoto via Internet** scheda.  
  
6.  Nel **accesso remoto via Internet** scheda, eseguire le operazioni seguenti:  
  
    1.  Per consentire all'utente di connettersi al server tramite la VPN, selezionare il **Consenti privata rete virtuale (VPN)** casella di controllo.  
  
    2.  Per consentire all'utente di connettersi al server tramite accesso Web remoto, selezionare il **Consenti accesso Web remoto e Accedi ai servizi Web** casella di controllo.  
  
7.  Fare clic su **applica**, quindi fare clic su **OK**.  
  
##  <a name="BKMK_2"></a>Configurare il router  
 Quando si configura il server per accesso Web remoto, la configurazione ovunque accesso guidata tenta di configurare il router. Se si cambia il router o modificare le impostazioni sul router, è necessario eseguire nuovamente la configurazione del Router guidata. Per ulteriori informazioni, vedere gli argomenti seguenti:  
  
-   [Configurare il router](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Sostituire un router](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Definita dei percorsi di rete](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Abilitare i controlli ActiveX di Servizi Desktop remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a>Configurare il router  
 Durante questo passaggio, Windows Server Essentials tenta di configurare automaticamente il router utilizzando i comandi UPnP. A tale scopo, il router deve supportare gli standard UPnP e l'impostazione UPnP deve essere abilitata nel router.  
  
> [!NOTE]
>  La configurazione di rete deve seguire i requisiti di rete supportati per Windows Server Essentials. Dovrebbe esserci un solo router nella rete.  
  
 Se il router non è configurato per la configurazione del dominio nome guidata, è necessario inoltrare manualmente la porta 443. Per informazioni su come impostare il port forwarding sul router, vedere [configurazione del Router](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="BKMK_ReplaceRouter"></a>Sostituire un router  
 Sostituire il router in base alle istruzioni di s produttore e quindi eseguire la configurazione del Router guidata per configurare il nuovo router.  
  
##### <a name="to-set-up-your-new-router"></a>Per configurare il nuovo router  
  
1.  Nel Dashboard di Windows Server Essentials, fare clic su **impostazioni**.  
  
2.  Fare clic su di **accesso remoto via Internet** scheda e quindi il **Router** fare clic su **impostare**. Avvia la configurazione del Router guidata.  
  
3.  Seguire le istruzioni della procedura guidata per completare l'installazione del nuovo router.  
  
###  <a name="BKMK_NetworkLocation"></a>Definita dei percorsi di rete  
 Un percorso di rete è una raccolta di impostazioni di rete Windows si applica quando ci si connette a una rete. Le impostazioni variano e possono essere personalizzate in base al tipo di rete che usi. Le impostazioni per un percorso di rete determinano se alcune funzionalità (ad esempio file e la condivisione della stampante, l'individuazione di rete e condivisione delle cartelle pubbliche) sono attivato o disattivato. Percorsi di rete sono utili quando è necessario connettersi a reti diverse.  
  
 Ad esempio, si potrebbe proprietari di un computer portatile che si utilizza a casa e il processo. Quando sei in ufficio, connettono alla rete dell'ufficio. Tuttavia, quando si arriva home, si usa il portatile per accedere e riprodurre video e musica archiviati sul server principale. Quando si connette a una nuova rete e specifica il tipo di percorso, Windows assegna un profilo di rete preimpostato per questo tipo di posizione. Alla successiva che connessione alla rete, Windows riconosce e assegna automaticamente le impostazioni corrette. Aggiunge un livello di sicurezza per proteggere le informazioni sul computer, e vengono attivate solo le funzionalità di rete che è necessario per tale posizione.  
  
 Esistono quattro tipi di percorsi di rete:  
  
-   **Rete domestica** scegliere questa rete per le reti domestiche o quando si conoscono e si considerano attendibili le persone e dispositivi nella rete. I computer in una rete domestica possono appartenere a un gruppo home. Individuazione di rete è attivato per le reti domestiche, che consente di vedere altri computer e dispositivi nella rete e consente ad altri utenti di rete nel computer.  
  
-   **Rete aziendale** scegliere questa rete per piccoli uffici o altre reti all'area di lavoro. Individuazione di rete, che consente di vedere altri computer e dispositivi in una rete e consente ad altri utenti di rete in uso, è attivata per impostazione predefinita, ma è possibile creare o partecipare a un gruppo home.  
  
-   **Rete pubblica** scegliere questa rete per luoghi pubblici (ad esempio bar o aeroporti). Questo percorso è progettato per mantenere il computer sia visibile ad altri computer e per proteggere il computer da software dannoso da Internet. Gruppo home non è disponibile per reti pubbliche e l'individuazione di rete è disattivata. È consigliabile scegliere questa opzione anche se si è connessi direttamente a Internet senza un router o se si dispone di una connessione mobile broadband.  
  
-   **Dominio** scegliere questa rete per domini come le aree di lavoro aziendali. Questo tipo di percorso di rete è controllato dall'amministratore di rete e non può essere selezionato o modificato.  
  
###  <a name="BKMK_ActiveX"></a>Abilitare i controlli ActiveX di Servizi Desktop remoto  
 I controlli ActiveX di Servizi Desktop remoto consente di accedere al computer domestico o aziendale, tramite Internet, da un altro computer usando accesso Web remoto.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Per abilitare i controlli ActiveX di Servizi Desktop remoto  
  
1.  In Internet Explorer, fare clic su **strumenti**, quindi fare clic su **Opzioni Internet**.  
  
2.  Nel **sicurezza** scheda, fare clic su **livello personalizzato**.  
  
3.  Nel **ActiveX controlli e plug-in** sezione, eseguire le operazioni seguenti:  
  
    1.  In **Scarica controlli ActiveX con firma**, fare clic su **prompt dei comandi**.  
  
    2.  In **Esegui controlli ActiveX e plug-in**, fare clic su **abilitare**.  
  
4.  Fare clic su **OK** due volte per accettare le modifiche e chiudere la finestra di dialogo.  
  
##  <a name="BKMK_3"></a>Impostare il nome di dominio  
 Dopo aver attivato accesso Web remoto, è possibile impostare un nome di dominio per il server che esegue Windows Server Essentials. Questo è un passaggio necessario se si prevede di usare accesso Web remoto da un computer remoto. Per ulteriori informazioni, vedere gli argomenti seguenti:  
  
-   [Panoramica di nomi di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Comprendere i nomi di dominio personalizzati Microsoft](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Utilizzare un nome di dominio nuovo o esistente](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurare un nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Scegliere un provider di servizi di dominio nome](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Scegliere un nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Scegliere un prefisso del nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Scegliere un'estensione del nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Aggiornare o aggiornare il servizio del nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Esportare o importare il certificato nel server](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurare manualmente un nome di dominio](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Trovare il provider di servizi di dominio nome](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a>Panoramica di nomi di dominio  
 Un nome di dominio identifica in modo univoco il server su Internet. I nomi di dominio sono costituite da almeno due parti: un nome di dominio di livello superiore (TLD) e un secondo nome di dominio di livello. Ad esempio, in contoso.com, com è il TLD e contoso è il secondo nome di dominio di livello.  
  
 Mentre si è fuori sede, è possibile utilizzare il nome di dominio per accedere a file condivisi nel server o computer della rete. È inoltre possibile gestire il server ci si trovi. Ad esempio, registrare contoso.com per il server. Quando si è fuori sede, è possibile aprire un web browser nel portatile e digitare **contoso.com** nella casella di testo indirizzo per connettersi all'istanza di accesso Web remoto è configurato in Windows Server Essentials.  
  
###  <a name="BKMK_PersonalizedNames"></a>Comprendere i nomi di dominio personalizzati Microsoft  
 Un nome di dominio personalizzato Microsoft include le funzionalità seguenti:  
  
-   Un nome di dominio personalizzato per l'accesso Web remoto (ad esempio, *nomepropriohost*. remotewebaccess.com). Il nome di dominio è associato l'indirizzo IP pubblico.  
  
-   Un dinamico DNS aggiornare servizio di protocollo, in modo che accesso Web remoto usando il nome di dominio non verrà interrotto se l'indirizzo IP pubblico cambia. In genere, il provider di servizi Internet (ISP) per le connessioni a banda larga s di organizzazione forniscono indirizzi IP pubblici dinamici che possono essere modificate.  
  
-   Un certificato attendibile associato al nome di dominio.  
  
 Per integrare un nome di dominio personalizzato Microsoft con il server, è necessario un account Microsoft (precedentemente noto come Windows Live ID). Se non hai un account Microsoft, è possibile iscriversi per una per il [Microsoft Hotmail](https://login.live.com/) sito Web.  
  
> [!IMPORTANT]
>  Windows Live consente la password dell'account Microsoft che il server non supporta i caratteri speciali. Se si utilizza un dominio personalizzato Microsoft, assicurarsi che la password dell'account Microsoft contenga solo caratteri supportati dal server. Il server non supporta l'utilizzo dei caratteri $, /, ' e %.  
  
###  <a name="BKMK_UseNewName"></a>Utilizzare un nome di dominio nuovo o esistente  
 Per configurare automaticamente il nome di dominio in un server che esegue Windows Server Essentials, è necessario utilizzare un provider di servizi di nome di dominio elencato nella configurazione del dominio nome guidata. È possibile scegliere di ottenere un nuovo nome di dominio o utilizzare un nome di dominio esistente. Eseguire una delle operazioni seguenti:  
  
-   Se si desidera ottenere un nuovo nome di dominio a un dominio di provider di nomi di servizio che sono elencati nella procedura guidata, fare clic su **si desidera configurare un nuovo nome di dominio**.  
  
-   Se si dispone di un nome di dominio acquistato da uno dei provider di servizi di dominio supportati nome, è possibile utilizzare la configurazione del dominio nome guidata per configurare il nome di dominio per il server. Fare clic su **si desidera utilizzare un nome di dominio dispone già di**e quindi digitare il nome di dominio nel **imposta nome di dominio** casella di testo. È necessario fornire il nome utente e password utilizzata per acquistare il nome di dominio.  
  
-   Se si dispone di un nome di dominio acquistato da un provider di servizi di nome di dominio che non è supportato da Windows Server Essentials e si desidera utilizzare la configurazione del dominio nome guidata per configurare il nome di dominio per il server, è possibile trasferire il nome di dominio a uno dei provider di servizi di nomi di dominio elencati nella procedura guidata. Fare clic su **si desidera utilizzare un nome di dominio dispone già di**, digitare il nome di dominio nel **nome di dominio** testo casella e quindi segui le istruzioni nel dominio nome servizio provider s sito Web per trasferire il nome di dominio.  
  
###  <a name="BKMK_SetUpName"></a>Configurare un nome di dominio  
 Quando si attiva accesso Web remoto, è possibile configurare il nome di dominio Internet del server.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Per impostare o gestire un nome di dominio Internet  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **le impostazioni del Server**, quindi fare clic su di **accesso remoto via Internet** scheda.  
  
3.  Nel **nome di dominio** fare clic su **impostare**.  
  
4.  Seguire le istruzioni per completare la procedura guidata. Se non si possiede già un nome di dominio e un certificato, la procedura guidata consente di trovare un provider di nomi di dominio per l'acquisto di un nome di dominio e un certificato oppure è possibile ottenere un nome di dominio Microsoft personalizzato.  
  
###  <a name="BKMK_ChooseProvider"></a>Scegliere un provider di servizi di dominio nome  
 È consigliabile scegliere un provider di servizi di nome di dominio che supporta l'estensione del nome di dominio che si desidera utilizzare. La configurazione del dominio nome guidata include un elenco dei provider completo che è possibile utilizzare con un collegamento al sito Web s ogni provider. Fare clic su di **più Info** collegamento accanto a ogni nome di provider s per ottenere informazioni sui servizi e i prezzi offerti dai provider di servizi.  
  
> [!NOTE]
>  Alcuni provider di servizi di dominio nome servire ampie aree geografiche internazionali e altri servono mercati più piccoli. Per questo motivo, alcuni provider potrebbero non offrire un sito Web tradotto nella propria lingua preferita.  
  
 Quando si acquista il nome di dominio, è anche possibile acquistare il servizio di protocollo di aggiornamento dinamico del sistema DNS (Domain Name) dal provider di servizi di nomi di dominio. Protocollo di aggiornamento dinamico DNS è un servizio che consente a chiunque su Internet di accedere alle risorse in una rete locale quando l'indirizzo IP di tale rete cambia continuamente. Oppure è possibile acquistare un indirizzo IP statico dal Provider di servizi Internet (ISP) per garantire che l'indirizzo IP non cambia.  
  
###  <a name="BKMK_ChooseDomainName"></a>Scegliere un nome di dominio  
 Scegliere un nome che identifica in modo univoco il server aziendale. Ad esempio, se il nome dell'organizzazione è Contoso Ltd, è possibile scegliere Contoso per identificare il server domestico o aziendale su Internet. Se il nome di dominio non è disponibile, prova a un'altra variazione di tale nome oppure con qualcosa completamente diversa.  
  
 Il nome che immesso può contenere quanto segue:  
  
-   Massimo 63 caratteri.  
  
-   Lettere (in inglese o i caratteri localizzati), numeri o trattini (-). Il nome deve iniziare e finire con una lettera o un numero.  
  
    > [!NOTE]
    >  I nomi di dominio non sono tra maiuscole e minuscole.  
  
###  <a name="BKMK_Prefixes"></a>Scegliere un prefisso del nome di dominio  
 Un nome di dominio è costituito da etichette gerarchiche.  
  
 **L'estensione di dominio di primo livello** è l'etichetta all'estrema destra nel nome di dominio. Ad esempio, in www.contoso.com, com è l'estensione del nome di dominio di primo livello.  
  
 **Il nome di dominio di secondo livello** è l'etichetta accanto l'estensione del nome di dominio di primo livello. Il nome di dominio di secondo livello viene spesso creato in base il nome della società, prodotti o servizi. In www.contoso.com, ad esempio, contoso è il nome di dominio di secondo livello ed è stato scelto per il nome della società Contoso Pharmaceuticals. Il dominio di secondo livello è detta anche il nome host, che è associato un indirizzo IP.  
  
 **Il prefisso del nome di dominio** identifica un sottodominio. Il nome del sottodominio può essere utilizzato per identificare servizi, dispositivi o aree geografiche. Ad esempio, Contoso Pharmaceuticals vuole consentire agli utenti remoti di accedere ad accesso Web remoto, ma non desidera che il sito Web sia disponibile al pubblico, quindi crea un sottodominio che consenta solo gli utenti con autorizzazioni appropriate per accedere al sito Web. Contoso Pharmaceuticals Configura remote.contoso.com come sottodominio e remoto è il prefisso del nome di dominio.  
  
> [!TIP]
>  È consigliabile utilizzare il valore predefinito **remoto** come prefisso del nome di dominio.  
  
###  <a name="BKMK_Extension"></a>Scegliere un'estensione del nome di dominio  
 Quando si sceglie un nome di dominio per il sito Web Internet, devi anche specificare l'estensione del nome di dominio che si desidera utilizzare. Identificata dalle lettere che seguono il punto finale di qualsiasi nome di dominio. (Il termine ufficiale usato per l'estensione è il dominio di primo livello o TLD).  
  
 Esistono due tipi principali di estensioni di dominio che è possibile utilizzare: generico e con codice paese.  
  
#### <a name="generic-top-level-domains"></a>Domini di primo livello generici  
 Estensioni di dominio generiche sono tre o più lettere di lunghezza e sono in genere usate da determinati tipi di organizzazioni.  
  
 **Esempi di domini di primo livello generici**  
  
|Estensione di dominio|Descrizione|  
|----------------------|-----------------|  
|. com|In genere usato da organizzazioni commerciali, ma può essere usata da chiunque.|  
|.NET|Progettato per le aziende che offrono servizi di infrastruttura di rete.|  
|org|In origine usata da enti no profit e altre attività che non rientrano in un'altra categoria di dominio di primo livello generico. Può essere usata da chiunque.|  
|. edu|Può essere usata solo da organizzazioni didattiche.|  
  
#### <a name="country-code-top-level-domains"></a>Domini di primo livello di codice paese  
 Queste estensioni di dominio sono due lettere lunghezza. Sono progettate per essere utilizzati da organizzazioni nel paese o area geografica che è associata al codice. Alcuni domini di primo livello di codice paese sono limitati per l'uso solo dai cittadini di quel paese o area geografica. Altri sono disponibili a tutti gli utenti.  
  
 **Esempi di domini di primo livello di codice paese**  
  
|Estensione di dominio|Descrizione|  
|----------------------|-----------------|  
|CA|Usare per siti Web in Canada|  
|.CN|Usare per siti Web in Cina|  
|.de|Usare per siti Web in Germania|  
|. co.uk|Usare per siti Web nel Regno Unito|  
  
 Per visualizzare l'elenco completo dei domini di primo livello, vedere il [sito Web di Internet Assigned Numbers Authority](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Se un'estensione di dominio non è disponibile per selezionare la configurazione guidata nome dominio  
 Quando si esegue la configurazione guidata nome dominio, la procedura guidata esamina le informazioni di sistema per determinare il paese o area geografica. La procedura guidata visualizza quindi solo le estensioni di dominio che supportano i provider partecipanti nell'area. Se l'estensione di dominio che si desidera non viene visualizzato nell'elenco, è necessario scegliere un'estensione di dominio diverso per continuare. Selezionare un'estensione nell'elenco restituito dalla procedura guidata.  
  
###  <a name="BKMK_UpdateService"></a>Aggiornare o aggiornare il servizio del nome di dominio  
 Potrebbe essere necessario aggiornare o aggiornare il servizio del nome di dominio se acquistato un nome di dominio, ma non è stato acquistato un certificato. È necessario un certificato per il nome di dominio dal provider di servizi di nomi di dominio.  
  
> [!NOTE]
>  Funziona con il provider di servizi di nome di dominio per determinare il tipo di certificato che è necessario. Il certificato può essere uno dei certificati meno costosi offerti. Tuttavia, rivedere la documentazione e le funzionalità dei certificati di sicurezza di livello superiore per stabilire se soddisfano meglio le esigenze aziendali.  
  
###  <a name="BKMK_ExportCert"></a>Esportare o importare il certificato nel server  
 Se si desidera creare una copia di backup di un certificato o usarlo in un altro server, è necessario esportare il certificato. Per informazioni sull'esportazione di certificati, vedere [esportare un certificato](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="BKMK_SetNameManually"></a>Configurare manualmente un nome di dominio  
 Se si sceglie questa opzione, il server di monitoraggio e di mantenere il nome di dominio e non ricevere un avviso se esiste un problema di configurazione. Questa opzione è possibile anche in presenza di una delle operazioni seguenti:  
  
-   Provider di nomi di dominio non partner sono elencati per il paese o area geografica.  
  
-   Il provider di dominio partner elencati non supportano l'estensione del nome di dominio.  
  
-   Si dispone di un nome di dominio esistente da un provider di nomi di dominio che non è attualmente un partner e non si desidera trasferire tale nome di dominio a un provider di nomi di dominio supportati in Windows Server Essentials.  
  
-   La procedura guidata non viene elencato l'estensione del nome di dominio che si desidera utilizzare, ma l'estensione è disponibile da un provider di nomi di dominio che non è attualmente un partner.  
  
 Se si sceglie di configurare manualmente il nome di dominio, con il provider di servizi di nome di dominio per creare un Record per il dominio.  
  
##### <a name="to-create-an-a-record"></a>Per creare un Record  
  
1.  Scegliere un nome host, ad esempio remoto. Questo è il prefisso del nome di dominio. Il prefisso del nome di dominio e il nome di dominio definiscono l'URL per aprire la pagina di accesso di accesso Web remoto; ad esempio, **http://remote.contoso.com**.  
  
2.  Nel dominio nome servizio provider configurazione dashboard (in genere nella relativa pagina Web), creare il record per il nome host scelto nel passaggio 1. Assicurarsi che l'indirizzo IP specificato nel record è l'indirizzo IP del lato WAN del router (lato per Internet). Consultare la documentazione del router per trovare l'indirizzo IP WAN.  
  
3.  Si consiglia di contattare il Provider di servizi Internet (ISP) per acquistare un indirizzo IP statico per la rete. Ciò garantisce che l'indirizzo IP non cambi e che la voce del DNS sia sempre aggiornata.  
  
     Se non hai la possibilità di ottenere un indirizzo IP statico dal provider di servizi Internet, è anche possibile acquisto del servizio di protocollo di aggiornamento dinamico del sistema DNS (Domain Name) dal provider di servizi di nomi di dominio o un altro provider di servizi. Protocollo di aggiornamento dinamico DNS è un servizio che mantiene l'indirizzo IP WAN della rete aggiornato in modo che l'indirizzo IP può essere risolto nel nome di dominio, anche se l'indirizzo IP cambia.  
  
4.  Importare un certificato attendibile quando viene richiesto. Se non hai un certificato attendibile, puoi richiederne uno a uno dei provider di nomi di dominio supportati elencati nella procedura guidata o acquistarne uno dal provider attendibile desiderato. Per ulteriori informazioni sui certificati attendibili, rivolgersi al servizio di nome di dominio.  
  
###  <a name="BKMK_Find"></a>Trovare il provider di servizi di dominio nome  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Per trovare il provider di servizi di dominio nome per il nome di dominio  
  
1.  Aprire un web browser e digitare **www.internic.com** nella barra degli indirizzi per passare alla home page Internic.  
  
2.  Nella home page Internic fare clic su **Whois**.  
  
3.  Nel **Whois**, digitare il nome di dominio (ad esempio contoso.com).  
  
4.  Fare clic su di **dominio** opzione, quindi fare clic su **Invia**.  
  
5.  Nei risultati della ricerca, il nome del provider di servizi di nomi di dominio è elencato in **Registrar**.  
  
##  <a name="BKMK_4"></a>Personalizzare accesso Web remoto  
 È possibile personalizzare il sito accesso Web remoto mediante l'aggiunta di un'immagine di sfondo o logo personale. È anche possibile aggiungere collegamenti nella Home page in modo che queste informazioni sono disponibili per tutti gli utenti. Per ulteriori informazioni, vedere gli argomenti seguenti:  
  
-   [Personalizzare accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personalizzare le immagini per sfondi e logo](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Ripristinare accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a>Personalizzare accesso Web remoto  
 È possibile personalizzare accesso Web remoto cambiando il titolo del sito Web, modifica l'immagine di sfondo e il logo e aggiungendo collegamenti ad altri siti Web nella home page.  
  
##### <a name="to-customize-remote-web-access"></a>Per personalizzare accesso Web remoto  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **impostazioni**, quindi fare clic su di **accesso remoto via Internet** scheda.  
  
3.  Nel **Impostazioni sito Web** fare clic su **Personalizza**.  
  
4.  Al termine della personalizzazione di accesso Web remoto, fare clic su **OK**. Testare le modifiche apportate ad accesso Web remoto.  
  
###  <a name="BKMK_CustomizeImages"></a>Personalizzare le immagini per sfondi e logo  
 In questa sezione fornisce informazioni sulle immagini che è possibile utilizzare per personalizzare accesso Web remoto.  
  
#### <a name="image-size"></a>Dimensioni dell'immagine  
 **Immagini di logo**  
  
 È consigliabile usare immagini di logo di 32x32 pixel. Le immagini più grandi vengono ridotte a 32x32 e quelle più piccole vengono allargate 32x32, con una possibile distorsione dell'immagine.  
  
 **Immagini di sfondo**  
  
 Anche se non esiste alcun limite di dimensione per immagini di sfondo, per ottenere risultati ottimali, è consigliabile usare immagini approssimativamente di 800x500 pixel. L'immagine di sfondo viene posizionata al centro (orizzontale e verticale) della pagina di accesso. Per rendere il testo nella pagina di accesso facile da leggere, il centro dell'immagine di sfondo dovrebbe essere un colore chiaro.  
  
#### <a name="image-file-types"></a>Tipi di file di immagine  
 I seguenti tipi di file di immagine possono essere utilizzati per sostituire il logo di sfondo e sito Web predefinito:  
  
-   Bitmap (bmp, \*.dib, \*.rle)  
  
-   GIF (GIF)  
  
-   PNG (PNG)  
  
-   JPG (jpg)  
  
###  <a name="BKMK_RepairRWA"></a>Ripristinare accesso Web remoto  
 La procedura guidata Ripristina consente di rilevare e risolvere i problemi del router o un nome di dominio. Esistono due modi per individuare i problemi di accesso Web remoto:  
  
-   Nelle impostazioni del Server nel Dashboard, nella scheda accesso remoto via Internet, viene visualizzata un'icona con una X rossa e una descrizione del problema.  
  
-   Un avviso nel Visualizzatore avvisi.  
  
> [!NOTE]
>  La procedura guidata Ripristina non è disponibile fino a quando non si attiva accesso Web remoto. Per informazioni sull'attivazione di accesso Web remoto, vedere [attivare accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Per ripristinare accesso Web remoto  
  
1.  Accedere al Dashboard.  
  
2.  Fare clic su **impostazioni**, quindi fare clic su di **accesso remoto via Internet** scheda.  
  
3.  Fare clic su **riparazione**. Il **Ripristina accesso Web remoto** inizia procedura guidata.  
  
4.  Fare clic su **Avanti**. La procedura guidata analizza accesso Web remoto, identifica il problema e quindi tenta di risolvere il problema.  
  
5.  Se si riceve un avviso quando è completato la procedura guidata, è possibile fare clic su **riprovare** per provare a ripristinare nuovamente il problema. Se si continua a ricevere un avviso, controllare l'avviso per ulteriori informazioni sul problema e risoluzione dei problemi.  
  
##  <a name="BKMK_5"></a>Risolvere i problemi di accesso Web remoto  
  
-   [Risolvere i problemi di connettività di accesso Web remoto](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Risolvere i problemi relativi al firewall](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Risoluzione dei problemi in un punto qualsiasi accesso](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Opzioni desktop remoto](Remote-desktop-options.md)  
  
-   [Usare accesso Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire l'accesso remoto via Internet](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
