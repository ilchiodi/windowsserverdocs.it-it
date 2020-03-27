---
title: Aggiungere DirectAccess a una distribuzione di Accesso remoto (VPN) esistente
description: Questo argomento fa parte della guida aggiungere DirectAccess a una distribuzione di accesso remoto esistente (VPN) per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5db01f7-1ae0-46f2-9be7-8d9e121446b2
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1ad1b823cf48a2c322c7ccab1799c76993b1e9bf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314811"
---
# <a name="add-directaccess-to-an-existing-remote-access-vpn-deployment"></a>Aggiungere DirectAccess a una distribuzione di Accesso remoto (VPN) esistente

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrizione dello scenario  
In questo scenario, un singolo computer che esegue Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 viene configurato come server DirectAccess con le impostazioni consigliate dopo aver già installato e configurato la VPN. Per configurare DirectAccess con le funzionalità aziendali, ad esempio un cluster con bilanciamento del carico, una distribuzione multisito o autenticazione client a due fattori, completare lo scenario descritto in questo argomento per configurare un solo server e quindi configurare lo scenario aziendale come descritto in [distribuire accesso remoto in un'organizzazione](../../ras/Deploy-Remote-Access-in-an-Enterprise.md).  
  
## <a name="in-this-scenario"></a>In questo scenario  
Per configurare un unico server di Accesso remoto è necessaria una serie di passaggi di pianificazione e distribuzione.  
  
### <a name="planning-steps"></a>Passaggi per la pianificazione  
La pianificazione è suddivisa in due fasi:  
  
1.  **Pianificare l'infrastruttura di accesso remoto**  
  
    In questa fase, si descrive la pianificazione richiesta per configurare l'infrastruttura di rete prima di iniziare la distribuzione di Accesso remoto. È inclusa la pianificazione di rete e topologia del server, certificati, Domain Name System (DNS), configurazione di Active Directory e oggetti Criteri di gruppo e il server del percorso di rete DirectAccess.  
  
2.  **Pianificare la distribuzione di accesso remoto**  
  
    In questa fase viene descritta la pianificazione necessaria per preparare la distribuzione di Accesso remoto. È inclusa la pianificazione dei computer client di Accesso remoto, dei requisiti di autenticazione server e client e dei server di infrastruttura.  
  
### <a name="deployment-steps"></a>Fasi di distribuzione  
La distribuzione è suddivisa in tre fasi:  
  
1.  **Configurare l'infrastruttura di accesso remoto**  
  
    In questa fase verranno configurati rete e routing, le impostazioni del firewall (se necessario), certificati, server DNS, le impostazioni di Active Directory e dell'oggetto Criteri di gruppo e il server dei percorsi di rete di DirectAccess.  
  
2.  **Configurare le impostazioni del server di accesso remoto**  
  
    In questa fase si configureranno i computer client di Accesso remoto, il server di Accesso remoto e i server di infrastruttura.  
  
3.  **Verificare la distribuzione**  
  
    In questa fase è necessario verificare il corretto funzionamento della distribuzione.  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Applicazioni pratiche  
La distribuzione di un singolo server di Accesso remoto offre:  
  
-   **Accessibilità**  
  
    I computer client gestiti che eseguono Windows 8 e Windows 7 possono essere configurati come computer client DirectAccess. Questi client possono accedere alle risorse di rete interne con DirectAccess ogni volta che sono connessi a Internet, senza dover accedere a una connessione VPN. I computer client che non eseguono uno di questi sistemi operativi possono connettersi alla rete interna con una VPN. DirectAccess e VPN sono gestiti nella stessa console e con lo stesso gruppo di procedure guidate.  
  
-   **Facilità di gestione**  
  
    I computer client DirectAccess che hanno accesso a Internet possono essere gestiti in modalità remota dagli amministratori di Accesso remoto con DirectAccess, anche quando i computer client non si trovano sulla rete aziendale interna. Per i computer client che non soddisfano i requisiti aziendali vengono eseguiti il monitoraggio e l'aggiornamento automatici tramite i server di gestione.  
  
## <a name="roles-and-features-required-for-this-scenario"></a><a name="BKMK_NEW"></a>Ruoli e funzionalità richiesti per questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per questo scenario:  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|---------|-----------------|  
|Ruolo Accesso remoto|Il ruolo viene installato e disinstallato con la console di Server Manager o Windows PowerShell. Questo ruolo comprende DirectAccess, una funzionalità precedentemente inclusa in Windows Server 2008 R2, e il servizio Routing e Accesso remoto che in precedenza era un servizio ruolo nel ruolo server Servizi di accesso e criteri di rete (NPAS). Il ruolo Accesso remoto è costituito da due componenti:<br /><br />1. DirectAccess e servizio Routing e accesso remoto (RRAS) VPN: gestito nella console di gestione accesso remoto.<br />2. routing RRAS: gestito nella console di routing e accesso remoto.<br /><br />Il ruolo server di Accesso remoto dipende dalle funzionalità server seguenti:<br /><br />-Internet Information Services (IIS) server Web: necessario per configurare il server dei percorsi di rete nel server di accesso remoto e il probe Web predefinito.<br />-Database interno di Windows: usato per l'accounting locale sul server di accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />Per impostazione predefinita, in un server di accesso remoto quando viene installato il ruolo accesso remoto. Supporta l'interfaccia utente della console di gestione remota e i cmdlet di Windows PowerShell.<br />-Installata facoltativamente in un server che non esegue il ruolo del server accesso remoto. In questo caso, viene usata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />-GUI di accesso remoto<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit (CMAK)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
  
## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  
  
**Requisiti del server**  
  
-   Un computer che soddisfi i requisiti hardware per Windows Server 2012.  
  
-   Il server deve avere almeno una scheda di rete installata, abilitata e aggiunta alla rete interna. Quando si usano due schede di rete, una di esse deve essere connessa alla rete aziendale interna e l'altra alla rete esterna (Internet).  
  
-   Se è richiesto Teredo come protocollo di transizione da IPv4 a IPv6, la scheda esterna del server richiede due indirizzi IPv4 pubblici consecutivi. L'Abilitazione guidata DirectAccess non abilita Teredo, neanche se sono presenti due indirizzi IP consecutivi. Se è disponibile un singolo indirizzo IP, usare solo IP-HTTPS come protocollo di transito.  
  
-   Almeno un controller di dominio. Il server di Accesso remoto e il client DirectAccess devono essere membri di un dominio.  
  
-   L'Abilitazione guidata DirectAccess richiede i certificati per IP-HTTPS e il server dei percorsi di rete. Se la connessione VPN SSTP usa già un certificato, questo verrà riutilizzato per IP-HTTPS. Se la connessione VPN SSTP non è stata configurata, è possibile configurare un certificato per IP-HTTPS oppure usare un certificato autofirmato creato automaticamente. Per il server dei percorsi di rete è possibile configurare un certificato oppure usare un certificato autofirmato creato automaticamente.  
  
**Requisiti client**  
  
-   Un computer client deve eseguire Windows 8 o Windows 7.  
  
    > [!NOTE]  
    > Solo i sistemi operativi seguenti possono essere usati come client DirectAccess: Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise e Windows 7 Ultimate.  
  
**Requisiti del server di infrastruttura e gestione**  
  
-   Durante la gestione remota dei computer client DirectAccess, i client avviano le comunicazioni con i server di gestione quali controller di dominio, server System Center Configuration e server Autorità registrazione integrità per i servizi che includono aggiornamenti di Windows e antivirus e conformità dei client di Protezione accesso alla rete . I server necessari devono essere distribuiti prima dell'inizio della distribuzione di Accesso remoto.  
  
-   Se Accesso remoto richiede la conformità NAP del client, i server NPS e HRA devono essere distribuiti prima dell'inizio della distribuzione di Accesso remoto.  
  
-   È necessario un server DNS che esegue Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 con SP2.  
  
## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisiti software  
I requisiti software per questo scenario includono i seguenti.  
  
**Requisiti del server**  
  
-   Il server di Accesso remoto deve essere membro di un dominio. Il server può essere distribuito alla periferia della rete interna o dietro un firewall o un altro dispositivo periferico.  
  
-   Se il server di Accesso remoto è protetto da un firewall periferico o un dispositivo NAT (Network Address Translation), il dispositivo deve essere configurato in modo da consentire il traffico da e verso il server di Accesso remoto.  
  
-   La persona che distribuisce Accesso remoto sul server deve disporre di autorizzazioni di amministratore locale sul server e di autorizzazioni a livello utente di dominio. L'amministratore deve disporre anche di autorizzazioni per gli oggetti Criteri di gruppo usati nella distribuzione di DirectAccess. Per sfruttare le funzionalità che limitano la distribuzione di DirectAccess solo ai computer portatili, sono richieste autorizzazioni per creare un filtro WMI sul controller di dominio.  
  
**Requisiti dei client di accesso remoto**  
  
-   I client DirectAccess devono essere membri di un dominio. I domini contenenti client possono appartenere alla stessa foresta del server di Accesso remoto oppure disporre di un trust bidirezionale con la foresta o il dominio di Accesso remoto.  
  
-   È richiesto un gruppo di sicurezza di Active Directory per contenere i computer che saranno configurati come client DirectAccess. Se un gruppo di sicurezza non è specificato durante la configurazione delle impostazioni del client DirectAccess, per impostazione predefinita l'oggetto Criteri di gruppo del client viene applicato su tutti i computer portatili (compatibili con DirectAccess) nel gruppo di sicurezza Computer del dominio. Solo i sistemi operativi seguenti possono essere usati come client DirectAccess: Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise e Windows 7 Ultimate.  
  
    > [!NOTE]  
    > Si consiglia di creare un gruppo di sicurezza per ogni dominio che contiene computer che saranno configurati come client DirectAccess.  
  

  

