---
title: Distribuire un server DirectAccess singolo con impostazioni avanzate
description: Questo argomento fa parte della Guida distribuire un server DirectAccess singolo con impostazioni avanzate per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: b211a9ca-1208-4e1f-a0fe-26a610936c30
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7f6a6724a2ab7bb6da48a11d31fb04461912e388
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859524"
---
# <a name="deploy-a-single-directaccess-server-with-advanced-settings"></a>Distribuire un server DirectAccess singolo con impostazioni avanzate

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento fornisce un'introduzione allo scenario DirectAccess in cui viene usato un server DirectAccess singolo e consente di distribuire DirectAccess con impostazioni avanzate.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Prima di iniziare la distribuzione vedere l'elenco di configurazioni non supportate, problemi noti e prerequisiti  
È possibile utilizzare gli argomenti seguenti per esaminare i prerequisiti e altre informazioni prima di distribuire DirectAccess.  
  
-   [Configurazioni non supportate da DirectAccess](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Prerequisiti per la distribuzione di DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrizione dello scenario  
In questo scenario, un singolo computer che esegue Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, viene configurato come server DirectAccess con impostazioni avanzate.  
  
> [!NOTE]  
> Per configurare una distribuzione di base solo con impostazioni semplici, vedere [Deploy a Single DirectAccess Server Using the Getting Started Wizard](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md). Nello scenario semplice, DirectAccess viene configurato con le impostazioni predefinite usando una procedura guidata, senza dover configurare le impostazioni dell'infrastruttura, ad esempio un'autorità di certificazione (CA) o i gruppi di sicurezza Active Directory.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Per impostare un server DirectAccess singolo con impostazioni avanzate, completare alcuni passaggi di pianificazione e distribuzione.  
  
### <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare, è possibile controllare i seguenti requisiti.  
  
-   Windows Firewall deve essere abilitato in tutti i profili.  
  
-   Il server DirectAccess è il server dei percorsi di rete.  
  
-   Tutti i computer wireless nel dominio in cui viene installato il server DirectAccess devono essere abilitati per DirectAccess. Quando si distribuisce, DirectAccess viene automaticamente abilitato in tutti i computer portatili nel dominio corrente.  
  
> [!IMPORTANT]  
> Alcune tecnologie e configurazioni non sono supportate quando si distribuisce DirectAccess.  
>   
> -   Il protocollo ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) non è supportato nella rete aziendale. Se si usa la tecnologia ISATAP, rimuoverla e usare la connettività IPv6 nativa.  
  
### <a name="planning-steps"></a>Passaggi per la pianificazione  
La pianificazione è suddivisa in due fasi:  
  
1.  **Pianificazione dell'infrastruttura DirectAccess**. In questa fase viene descritta la pianificazione necessaria per impostare l'infrastruttura di rete prima di iniziare la distribuzione DirectAccess. Ciò include la pianificazione della rete e della topologia del server, la pianificazione dei certificati, DNS, la configurazione di Active Directory e degli oggetti Criteri di gruppo e il server dei percorsi di rete DirectAccess.  
  
2.  **Pianificazione della distribuzione DirectAccess**. In questa fase vengono descritti i passaggi di pianificazione necessari per la preparazione della distribuzione di DirectAccess. Include la pianificazione per i computer client DirectAccess, i requisiti per l'autenticazione client e server, le impostazioni VPN, i server dell'infrastruttura e i server applicazioni e di gestione.  
  
### <a name="deployment-steps"></a>Fasi di distribuzione  
La distribuzione è suddivisa in tre fasi:  
  
1.  **Configurazione dell'infrastruttura DirectAccess**. Questa fase comprende la configurazione di rete e routing, delle impostazioni firewall (se necessario), dei certificati, dei server DNS, delle impostazioni Active Directory e degli oggetti Criteri di gruppo e del server dei percorsi di rete DirectAccess.  
  
2.  **Configurazione delle impostazioni del server DirectAccess**. Questa fase include i passaggi per la configurazione dei computer client DirectAccess, del server DirectAccess, dei server dell'infrastruttura e dei server applicazioni e di gestione.  
  
3.  **Verifica della distribuzione**. Questa fase incluse include i passaggi per la verifica della distribuzione DirectAccess.  
  
Per le procedure dettagliate di distribuzione, vedere [Installare e configurare DirectAccess avanzato](../../../remote-access/directaccess/single-server-advanced/Install-and-Configure-Advanced-DirectAccess.md).  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Applicazioni pratiche  
La distribuzione di un server DirectAccess singolo offre:  
  
-   **Accessibilità**. I computer client gestiti che eseguono Windows 10, Windows 8.1, Windows 8 e Windows 7 possono essere configurati come computer client DirectAccess. Questi client possono accedere alle risorse di rete interne tramite DirectAccess ogni volta che sono connessi a Internet, senza dover accedere a una connessione VPN I computer client che non eseguono uno di questi sistemi operativi possono connettersi alla rete interna tramite una VPN.  
  
-   **Facilità di gestione**. I computer client DirectAccess su Internet possono essere gestiti in modalità remota dagli amministratori di Accesso remoto tramite DirectAccess, anche quando i computer client non si trovano sulla rete aziendale interna. Per i computer client che non soddisfano i requisiti aziendali vengono eseguiti il monitoraggio e l'aggiornamento automatici tramite i server di gestione. Sia DirectAccess che la VPN sono gestiti nella stessa console e con lo stesso gruppo di procedure guidate. È possibile gestire uno o più server DirectAccess da una singola Console di gestione Accesso remoto  
  
## <a name="roles-and-features-required-for-this-scenario"></a><a name="BKMK_NEW"></a>Ruoli e funzionalità richiesti per questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per questo scenario:  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|---------|-----------------|  
|Ruolo Accesso remoto|Il ruolo viene installato e disinstallato tramite la console di Server Manager o Windows PowerShell. Questo ruolo comprende sia DirectAccess che Servizio Routing e Accesso remoto (RRAS). Il ruolo Accesso remoto è costituito da due componenti:<br/><br/>1. DirectAccess e VPN RRAS. DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br/>2. routing RRAS. Le funzionalità di routing RRAS sono gestite nella console di routing e accesso remoto legacy.<p>Il ruolo server Accesso remoto dipende dai ruoli e/o dalle funzionalità server seguenti:<br/><br/> -Server Web Internet Information Services (IIS): questa funzionalità è necessaria per configurare il server dei percorsi di rete nel server DirectAccess e il probe Web predefinito.<br/> -Database interno di Windows. Usato per l'accounting locale nel server DirectAccess.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<p>-Viene installato per impostazione predefinita in un server DirectAccess quando è installato il ruolo accesso remoto e supporta l'interfaccia utente della console di gestione remota e i cmdlet di Windows PowerShell.<br />-Può essere installata facoltativamente in un server che non esegue il ruolo server DirectAccess. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<p>La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<p>-Interfaccia utente grafica (GUI) di accesso remoto<br />-Modulo di accesso remoto per Windows PowerShell<p>Le dipendenze includono:<p>-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit (CMAK)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
  
## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  
  
-   Requisiti del server:  
  
    -   Un computer che soddisfi i requisiti hardware per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
    -   Il server deve avere almeno una scheda di rete installata, abilitata e connessa alla rete interna. Quando si usano due schede, una di esse deve essere connessa alla rete aziendale interna e l'altra alla rete esterna (Internet o rete privata).  
  
    -   Se è richiesto Teredo come protocollo di transizione da IPv4 a IPv6, la scheda esterna del server richiede due indirizzi IPv4 pubblici consecutivi. Se è disponibile un solo indirizzo IP, può essere usato solo IP-HTTPS come protocollo di transizione.  
  
    -   Almeno un controller di dominio. Il server DirectAccess e i client DirectAccess devono essere membri di un dominio.  
  
    -   È richiesta un'autorità di certificazione (CA) se non si vogliono usare certificati autofirmati per IP-HTTPS o il server dei percorsi di rete oppure certificati client per l'autenticazione IPsec dei client. In alternativa, è possibile richiedere i certificati da una CA pubblica.  
  
    -   Se il server dei percorsi di rete non si trova nel server DirectAccess, per eseguirlo è richiesto un server Web distinto.  
  
-   Requisiti client  
  
    -   Un computer client deve eseguire Windows 10, Windows 8 o Windows 7.  
  
        > [!NOTE]  
        > I sistemi operativi seguenti possono essere usati come client DirectAccess: Windows 10, Windows Server 2012 R2, Windows Server 2012, Windows 8 Enterprise, Windows 7 Enterprise o Windows 7 Ultimate.  
  
-   Requisii del server di gestione e infrastruttura:  
  
    -   Durane la gestione remota dei computer client DirectAccess, i client avviano le comunicazioni con i server di gestione quali controller di dominio, server System Center Configuration e server Autorità registrazione integrità per i servizi che includono aggiornamenti di Windows e antivirus e conformità dei client di Protezione accesso alla rete . I server necessari devono essere distribuiti prima dell'inizio della distribuzione di Accesso remoto.  
  
    -   Se Accesso remoto richiede la conformità NAP del client, i server NPS e HRS devono essere distribuiti prima dell'inizio della distribuzione di Accesso remoto.  
  
    -   Se si abilita VPN, è necessario un server DHCP per allocare automaticamente gli indirizzi IP ai client VPN, se non si usa un pool di indirizzi statici.  
  
## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisiti software  
Per questo scenario sono presenti numerosi requisiti.  
  
-   Requisiti del server:  
  
    -   Il server DirectAccess deve essere membro di un dominio. Il server può essere distribuito alla periferia della rete interna o dietro un firewall o un altro dispositivo periferico.  
  
    -   Se il server DirectAccess è protetto da un firewall periferico o un dispositivo NAT, il dispositivo deve essere configurato in modo da consentire il traffico da e verso il server DirectAccess.  
  
    -   La persona che distribuisce Accesso remoto sul server deve disporre di autorizzazioni di amministratore locale sul server e di autorizzazioni a livello utente di dominio. L'amministratore deve inoltre disporre di autorizzazioni per gli oggetti Criteri di gruppo utilizzati nella distribuzione di DirectAccess. Per sfruttare le funzionalità che limitano la distribuzione di DirectAccess solo ai computer portatili, sono richieste autorizzazioni per creare un filtro WMI nel controller di dominio.  
  
-   Requisiti dei client di Accesso remoto:  
  
    -   I client DirectAccess devono essere membri di un dominio. I domini contenenti client possono appartenere alla stessa foresta del server DirectAccess oppure disporre di un trust bidirezionale con la foresta del server DirectAccess o con il dominio.  
  
    -   È richiesto un gruppo di sicurezza di Active Directory per contenere i computer che saranno configurati come client DirectAccess. Se un gruppo di sicurezza non è specificato durante la configurazione delle impostazioni del client DirectAccess, per impostazione predefinita l'oggetto Criteri di gruppo del client viene applicato su tutti i computer portatili nel gruppo di sicurezza Computer del dominio.  
  
        > [!NOTE]  
        > Si consiglia di creare un gruppo di sicurezza per ogni dominio che contiene computer client DirectAccess.  
  
        > [!IMPORTANT]  
        > Se è stato abilitato Teredo nella distribuzione DirectAccess e si vuole fornire l'accesso ai client Windows 7, assicurarsi che i client vengano aggiornati a Windows 7 con SP1. I client che utilizzano Windows 7 RTM non saranno in grado di connettersi tramite Teredo. Questi client possono comunque connettersi alla rete aziendale con IP-HTTPS.  
  
## <a name="see-also"></a><a name="BKMK_LINKS"></a>Vedere anche  
Nella tabella seguente vengono forniti i collegamenti a risorse aggiuntive.  
  
|Tipo di contenuto|Riferimenti|  
|--------|-------|  
|**Distribuzione**|[Percorsi di distribuzione di DirectAccess in Windows Server](../../../remote-access/directaccess/DirectAccess-Deployment-Paths-in-Windows-Server.md)<p>[Distribuire un server DirectAccess singolo tramite la procedura guidata di Introduzione](../../../remote-access/directaccess/single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)|  
|**Strumenti e impostazioni**|[Cmdlet di PowerShell per Accesso remoto](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Risorse della community**|[Guida alla sopravvivenza di DirectAccess](https://social.technet.microsoft.com/wiki/contents/articles/23210.directaccess-survival-guide.aspx)<p>[Voci wiki di DirectAccess](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Tecnologie correlate**|[Funzionamento di IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


