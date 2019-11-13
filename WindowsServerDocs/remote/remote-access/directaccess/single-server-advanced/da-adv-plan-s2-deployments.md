---
title: Passaggio 2 pianificare le distribuzioni avanzate di DirectAccess
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo con impostazioni avanzate per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3bba28d4-23e2-449f-8319-7d2190f68d56
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b093c4cbf5ceb06e84d5e07c8735106797932bc1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404930"
---
# <a name="step-2-plan-advanced-directaccess-deployments"></a>Passaggio 2 pianificare le distribuzioni avanzate di DirectAccess

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Dopo aver pianificato l'infrastruttura DirectAccess, il passaggio successivo della distribuzione di DirectAccess avanzato in un server singolo con IPv4 e IPv6 prevede la pianificazione delle impostazioni per la Configurazione guidata Accesso remoto.  
  
|Attività|Descrizione|  
|----|--------|  
|[2,1 piano per la distribuzione client](#21-plan-for-client-deployment)|Pianificare la modalità di connessione dei computer client con DirectAccess. Individuare i computer gestiti da configurare come client DirectAccess e pianificare la distribuzione di Assistente connettività di rete o di DirectAccess Connectivity Assistant nei computer client.|  
|[2,2 pianificare la distribuzione del server DirectAccess](#22-plan-for-directaccess-server-deployment)|Pianificare la modalità di distribuzione del server DirectAccess.|  
|[2,3 pianificare i server di infrastruttura](#23-plan-infrastructure-servers)|Pianificare il server dell'infrastruttura per la distribuzione DirectAccess, incluso il server dei percorsi di rete DirectAccess, i server DNS (Domain Name System) e i server di gestione DirectAccess.|  
|[2,4 pianificare i server applicazioni](#24-plan-application-servers)|Pianificare i server applicazioni IPv4 e IPv6 e, facoltativamente, valutare se è necessaria l'autenticazione end-to-end tra i computer client DirectAccess e i server applicazioni interni.|  
|[2,5 pianificare DirectAccess e client VPN di terze parti](#25-plan-directaccess-and-third-party-vpn-clients)|Quando si distribuisce DirectAccess con client VPN di terze parti, può essere necessario impostare un valore del Registro di sistema per consentire la coesistenza delle due soluzioni di accesso remoto.|  
  
## <a name="21-plan-for-client-deployment"></a>2.1 Pianificare la distribuzione del client  
Nel pianificare la distribuzione del client sarà necessario prendere tre decisioni:  
  
1.  DirectAccess sarà disponibile solo ai computer portatili o a qualsiasi computer?  
  
    Quando si configurano i client DirectAccess nella Configurazione guidata client DirectAccess, è possibile scegliere se consentire solo ai computer portatili nei gruppi di sicurezza specificati di connettersi con DirectAccess. Se si limita l'accesso ai computer portatili, Accesso remoto configura automaticamente un filtro WMI per assicurarsi che l'oggetto Criteri di gruppo del client DirectAccess venga applicato solo ai computer portatili nei gruppi di sicurezza specificati. L'amministratore di Accesso remoto richiede le autorizzazioni di creazione o modifica sicurezza per creare o modificare i filtri WMI dell'oggetto Criteri di gruppo WMI per abilitare l'impostazione.  
  
2.  Quali gruppi di sicurezza conterranno i computer client DirectAccess?  
  
    Le impostazioni del client DirectAccess sono contenute nell'oggetto Criteri di gruppo del client DirectAccess. L'oggetto Criteri di gruppo viene applicato ai computer che fanno parte dei gruppi di sicurezza specificati Configurazione guidata client DirectAccess. È possibile specificare i gruppi di sicurezza contenuti in qualsiasi dominio supportato. Per ulteriori informazioni, vedere la sezione [1,7 piano Active Directory Domain Services](da-adv-plan-s1-infrastructure.md#17-plan-active-directory-domain-services).  
  
    Prima di configurare DirectAccess, occorre creare i gruppi di sicurezza. È possibile aggiungere computer al gruppo di sicurezza dopo aver completato la distribuzione DirectAccess, ma si tenga presente che se si aggiungono computer client residenti in un dominio differente dal gruppo di sicurezza, l'oggetto Criteri di gruppo del client non verrà applicato a questi client. Ad esempio, se è stato creato SG1 nel dominio A per i client DirectAccess e in seguito si aggiungono i client dal dominio B a questo gruppo, l'oggetto Criteri di gruppo del client non verrà applicato ai client del dominio B. Per evitare questo problema, creare un nuovo gruppo di sicurezza del client per ciascun dominio che contiene computer client DirectAccess. In alternativa, se non si vuole creare un nuovo gruppo di sicurezza, eseguire il cmdlet di Windows PowerShell **Add-DAClient** con il nome del nuovo oggetto Criteri di gruppo per il nuovo dominio.  
  
3.  Quali impostazioni vengono configurate per l'Assistente connettività di rete?  
  
    L'Assistente connettività di rete viene eseguito nei computer client e fornisce informazioni aggiuntive sulla connessione di DirectAccess agli utenti finali. Nella Configurazione guidata client DirectAccess, è possibile configurare quanto segue:  
  
    -   **Sistemi di verifica della connettività**  
  
        Viene creato probe Web predefinito che i client usano per convalidare la connettività alla rete interna. Il nome predefinito è:  
  
        https://directaccess-WebProbeHost.<domain_name>  
  
        Il nome deve essere registrato manualmente in DNS. È possibile creare altri strumenti di verifica della connettività con altri indirizzi Web su HTTP o **ping**. Per ogni strumento di verifica della connettività deve esistere una voce DNS.  
  
    -   **Indirizzo di posta elettronica del supporto tecnico**  
  
        Se si verificano dei problemi di connettività DirectAccess, gli utenti finali possono inviare un messaggio di posta elettronica contenente informazioni diagnostiche all'amministratore DirectAccess per contribuire alla risoluzione del problema.  
  
    -   **Nome della connessione DirectAccess**  
  
        Specificare il nome di una connessione DirectAccess per aiutare gli utenti finali a identificare la connessione DirectAccess nei propri computer.  
  
    -   **Consenti ai client DirectAccess di usare la risoluzione dei nomi locali**  
  
        I client necessitano di un metodo per la risoluzione dei nomi locali. Se si consente ai client DirectAccess di usare la risoluzione dei nomi locali, gli utenti finali possono usare i server DNS locali per risolvere i nomi. Quando gli utenti finali scelgono di usare i server DNS locali per le risoluzioni dei nomi, DirectAccess non invia le richieste di risoluzione per i nomi con etichetta singola al server DNS aziendale interno. Usa invece la risoluzione dei nomi locali (con i protocolli LLMNR, Link-Local Multicast Name Resolution, e NetBIOS su TCP/IP).  
  
## <a name="22-plan-for-directaccess-server-deployment"></a>2.2 Pianificare la distribuzione del server DirectAccess  
Prendere in considerazione le seguenti opzioni quando si pianifica la distribuzione del server DirectAccess:  
  
-   **Topologia di rete**  
  
    Durante la distribuzione del server DirectAccess sono disponibili due topologie:  
  
    -   **Due schede di rete**. Con due schede di rete, DirectAccess può essere configurato con una scheda di rete connessa direttamente a Internet e l'altra connessa alla rete interna. In alternativa, il server viene installato dietro un dispositivo perimetrale, come un firewall o un router. In questa configurazione, una scheda di rete è connessa alla rete perimetrale e l'altra è connessa alla rete interna.  
  
    -   **Una scheda di rete**. In questa configurazione il server DirectAccess viene installato dietro un dispositivo perimetrale, come un firewall o un router. La scheda di rete viene connessa alla rete interna.  
  
    Per ulteriori informazioni sulla selezione della topologia per la distribuzione, vedere [1,1 pianificare la topologia di rete e le impostazioni](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings).  
  
-   **Indirizzo ConnectTo**  
  
    I computer client usano l'indirizzo ConnectTo per connettersi al server DirectAccess. L'indirizzo scelto dall'utente deve corrispondere al nome soggetto del certificato IP-HTTPS distribuito per la connessione IP-HTTPS ed essere disponibile nel DNS pubblico.  
  
-   **Schede di rete**  
  
    La Configurazione guidata del server di Accesso remoto rileva automaticamente le schede di rete configurate nel server DirectAccess. È necessario assicurarsi di aver selezionato le schede corrette.  
  
-   **Certificato IP-HTTPS**  
  
    La Configurazione guidata del server di Accesso remoto rileva automaticamente un certificato idoneo alla connessione IP-HTTPS. Il nome soggetto del certificato selezionato deve corrispondere all'indirizzo ConnectTo. Se si usano certificati autofirmati, è possibile scegliere di usare un certificato creato automaticamente dal server di Accesso remoto.  
  
-   **Prefissi IPv6**  
  
    Se la Configurazione guidata del server di Accesso remoto rileva che IPv6 è stato distribuito sulle schede di rete, vengono automaticamente popolati i prefissi IPv6 per la rete interna, un prefisso IPv6 da assegnare ai computer client DirectAccess e un prefisso IPv6 da assegnare ai computer client VPN. Se i prefissi generati automaticamente non sono corretti per l'infrastruttura IPv6 nativa, è necessario modificarli manualmente. Per ulteriori informazioni, vedere [1,1 pianificare la topologia di rete e le impostazioni](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings).  
  
-   **Autenticazione**  
  
    Decidere la modalità di autenticazione dei client DirectAccess nel server DirectAccess:  
  
    -   **Autenticazione utente**. È possibile abilitare l'autenticazione utente con le credenziali Active Directory o con un'autenticazione a due fattori. Per altre informazioni sull'autenticazione con autenticazione a due fattori, vedere [distribuire accesso remoto con l'autenticazione OTP](https://technet.microsoft.com/library/hh831379.aspx).  
  
    -   **Autenticazione computer**. È possibile configurare l'autenticazione computer in modo che usi i certificati o il server DirectAccess come proxy Kerberos per conto del client. Per ulteriori informazioni, vedere [1,3 pianificare i requisiti dei certificati](da-adv-plan-s1-infrastructure.md#13-plan-certificate-requirements).  
  
    -   **Client Windows 7**. Per impostazione predefinita, i computer client che eseguono Windows 7 non possono connettersi a una distribuzione DirectAccess di Windows Server 2012 R2 o Windows Server 2012. Se nell'organizzazione sono presenti client che eseguono Windows 7 e richiedono l'accesso remoto alle risorse interne, è possibile consentirne la connessione. Qualsiasi computer client al quale si intende concedere l'accesso alle risorse interne deve essere membro di un gruppo di sicurezza specificato nella Configurazione guidata client DirectAccess.  
  
        > [!NOTE]  
        > Per consentire ai client che eseguono Windows 7 di connettersi tramite DirectAccess, è necessario utilizzare l'autenticazione del certificato del computer.  
  
-   **Configurazione VPN**  
  
    Prima di configurare DirectAccess, decidere se si intende fornire l'accesso VPN a client remoti che non supportano DirectAccess. Se nell'organizzazione sono presenti computer client che non supportano la connettività DirectAccess (perché non sono gestiti o perché eseguono un sistema operativo che non supporta DirectAccess), fornire l'accesso VPN. La Configurazione guidata del server di Accesso remoto consente di configurare la modalità di assegnazione degli indirizzi IP (con DHCP o da un pool di indirizzi statici) e di autenticazione dei client VPN tramite un server Active Directory o RADIUS (Remote Authentication Dial-Up Service).  
  
## <a name="23-plan-infrastructure-servers"></a>2.3 Pianificare i server dell'infrastruttura  
DirectAccess richiede tre tipi di server dell'infrastruttura:  
  
-   **Server DNS**. Per altre informazioni, vedere [1.4 Pianificare i requisiti DNS](da-adv-plan-s1-infrastructure.md#14-plan-dns-requirements)  
  
-   **Server dei percorsi di rete**. Per altre informazioni, vedere [1.5 Pianificare il server dei percorsi di rete](da-adv-plan-s1-infrastructure.md#15-plan-the-network-location-server)  
  
-   **Server di gestione**. Per altre informazioni, vedere [1.6 Pianificare i server di gestione](da-adv-plan-s1-infrastructure.md#16-plan-management-servers)  
  
## <a name="24-plan-application-servers"></a>2.4 Pianificare i server applicazioni  
I server applicazioni sono server della rete aziendale accessibili dai computer client tramite una connessione DirectAccess. I server applicazioni vengono identificati aggiungendoli a un gruppo di sicurezza. L'oggetto Criteri di gruppo del server applicazioni viene quindi applicato ai server del gruppo.  
  
> [!NOTE]  
> L'aggiunta del server applicazioni a un gruppo di sicurezza è richiesta solo se sono necessarie l'autenticazione end-to-end e la crittografia.  
  
Facoltativamente, è possibile richiedere l'autenticazione end-to-end e la crittografia tra il client DirectAccess e i server applicazioni interni selezionati. Se si configura l'autenticazione end-to-end, i client DirectAccess usano i criteri di trasporto IPsec. Questi criteri richiedono l'interruzione dell'autenticazione e della protezione del traffico delle sessioni IPsec nei server applicazioni specificati. In questo caso, il server di Accesso remoto inoltra le sessioni IPsec autenticate e protette al server applicazioni.  
  
Per impostazione predefinita, quando si estende l'autenticazione ai server applicazioni, il payload dati viene crittografato tra il client DirectAccess e il server applicazioni. Si può scegliere di non crittografare il traffico e usare solo l'autenticazione. Tuttavia, questo è meno sicuro dell'uso dell'autenticazione e della crittografia ed è supportato solo per i server applicazioni che eseguono i sistemi operativi Windows Server 2008 R2 o Windows Server 2012.  
  
## <a name="25-plan-directaccess-and-third-party-vpn-clients"></a>2.5 Pianificare DirectAccess e i client VPN di terze parti  
Alcuni client VPN di terzi non creano connessioni nella cartella Connessioni di rete. DirectAccess potrebbe quindi determinare l'assenza di connettività Intranet quando viene stabilita la connessione VPN ed è disponibile la connettività alla Intranet. Questa condizione si verifica quando i client VPN di terze parti registrano le proprie interfacce definendole come tipi di endpoint NDIS (Network Device Interface Specification). È possibile abilitare la coesistenza con questi tipi di client VPN impostando il seguente valore del Registro di sistema su 1 nei client DirectAccess:  
  
**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\ShowDomainEndpointInterfaces (REG_DWORD)**  
  
Alcuni client VPN di terze parti utilizzano una configurazione split tunneling, che consente al computer client VPN di accedere direttamente a Internet senza dover inviare il traffico tramite la connessione VPN alla Intranet.  
  
Le configurazioni split tunneling mantengono in genere l'impostazione gateway predefinita nel client VPN come non configurata oppure 0.0.0.0. È possibile confermare questo comportamento stabilendo una connessione VPN alla Intranet e usando lo strumento da riga di comando Ipconfig.exe per visualizzare la configurazione risultante.  
  
Se la connessione VPN elenca il gateway predefinito come vuoto o 0.0.0.0, il client VPN viene configurato in questo modo. Per impostazione predefinita, il client DirectAccess non identifica le configurazioni split tunneling. Per configurare i client DirectAccess per il rilevamento di questi tipi di configurazioni del client VPN e per la coesistenza, impostare il seguente valore del Registro di sistema su 1:  
  
**HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\Internet\ EnableNoGatewayLocationDetection (REG_DWORD)**  
  
## <a name="previous-step"></a>Passaggio precedente  
  
-   [Passaggio 1: pianificare l'infrastruttura DirectAccess](da-adv-plan-s1-infrastructure.md)  
  


