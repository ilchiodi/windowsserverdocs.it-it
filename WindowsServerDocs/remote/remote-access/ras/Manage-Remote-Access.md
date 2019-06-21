---
title: Gestire l'accesso remoto
description: In questo argomento vengono fornite informazioni su come gestire l'accesso remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1459819a-b1b6-4800-8770-4a85d02c7a2b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3b2c251f99be455ec11e3ea3ef25ca14c8399de2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282975"
---
# <a name="manage-remote-access"></a>Gestire l'accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Lo scenario di distribuzione della gestione remota dei client DirectAccess usa DirectAccess per gestire i client tramite Internet. Questa sezione illustra lo scenario, compresi fasi, ruoli, funzionalità e collegamenti a risorse aggiuntive.  
  
Windows Server 2016 e Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) VPN in un unico ruolo Accesso remoto.   
  
> [!NOTE]  
> Oltre a questo argomento, sono disponibili i seguenti argomenti sulla gestione di Accesso remoto.  
>   
> -   [Usare il monitoraggio e l'accounting di Accesso remoto](monitoring-and-accounting/Use-Remote-Access-Monitoring-and-Accounting.md)  
> -   [Gestire i client DirectAccess in remoto](manage-remote-clients/Manage-DirectAccess-Clients-Remotely.md)  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
Poiché i computer client DirectAccess vengono connessi alla Intranet ogni volta che vengono connessi a Internet, indipendentemente dal fatto che l'utente abbia eseguito l'accesso al computer, possono essere gestiti come risorse Intranet e mantenuti aggiornati con modifiche ai Criteri di gruppo, aggiornamenti del sistema operativo, aggiornamenti antimalware e altre modifiche organizzative.  
  
In alcuni casi, i computer o i server Intranet devono avviare connessioni ai client DirectAccess. I tecnici del supporto possono ad esempio usare le connessioni Desktop remoto per connettersi ai client DirectAccess remoti e risolverne i problemi. Questo scenario consente di mantenere la soluzione di accesso remoto esistente per la connettività degli utenti, usando DirectAccess per la gestione remota.  
  
DirectAccess fornisce una configurazione che supporta la gestione remota dei client DirectAccess. È possibile usare un'opzione di distribuzione guidata che limita la creazione dei criteri solo a quelli necessari per la gestione remota dei computer client.  
  
> [!NOTE]  
> In questa distribuzione le opzioni di configurazione a livello utente, come l'imposizione del tunneling, l'integrazione di Protezione accesso alla rete e l'autenticazione a due fattori, non sono disponibili.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Lo scenario di distribuzione della gestione remota dei client DirectAccess include i passaggi seguenti per la pianificazione e la configurazione.  
  
### <a name="plan-the-deployment"></a>Pianificare la distribuzione  
Per la pianificazione di questo scenario sono previsti solo alcuni requisiti per i computer e la rete. Essi includono:  
  
-   **Topologia di rete e di server**: con DirectAccess è possibile collocare il server di Accesso remoto alla periferia della Intranet o dietro un dispositivo NAT (Network Address Translation) o un firewall.  
  
-   **Server dei percorsi di rete DirectAccess**: Il server dei percorsi di rete viene usato dai client DirectAccess per determinare se si trovano sulla rete interna. Il server dei percorsi di rete può essere installato nei server DirectAccess o in un altro server.  
  
-   **Client DirectAccess**: Stabilire quali computer gestiti saranno configurati come client DirectAccess.  
  
### <a name="configure-the-deployment"></a>Configurare la distribuzione  
La configurazione della distribuzione prevede alcuni passaggi. tra cui:  
  
1.  **Configurare l'infrastruttura**: Configurare le impostazioni DNS, aggiungere i computer client e server a un dominio, se necessario, e configurare i gruppi di sicurezza di Active Directory.  
  
    In questo scenario di distribuzione, gli oggetti Criteri di gruppo vengono creati automaticamente tramite Accesso remoto. Per le opzioni avanzate per oggetto Criteri di gruppo di certificati, vedere [distribuzione avanzata di accesso remoto](assetId:///3475e527-541f-4a34-b940-18d481ac59f6).  
  
2.  **Configurare le impostazioni di rete e del server di Accesso remoto**: configurare le schede di rete, gli indirizzi IP e il routing.  
  
3.  **Configurare le impostazioni dei certificati**: In questo scenario di distribuzione, attività iniziali guidate consente di creare certificati autofirmati, in modo che non è necessario configurare l'infrastruttura di certificati avanzata.  
  
4.  **Configurare il server dei percorsi di rete**:  in questo scenario il server dei percorsi di rete viene installato nel server di Accesso remoto.  
  
5.  **Pianificare i server di gestione DirectAccess**: gli amministratori possono gestire in remoto i computer client DirectAccess che si trovano all'esterno delle rete aziendale usando Internet. I server di gestione includono computer usati durante la gestione remota dei client, ad esempio i server di aggiornamento.  
  
6.  **Configurare il server di Accesso remoto**: installare il ruolo Accesso remoto ed eseguire Attività iniziali guidate di DirectAccess per configurare DirectAccess.  
  
7.  **Verificare la distribuzione**: testare un client per verificare che sia in grado di connettersi alla rete interna e a Internet con DirectAccess.  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
La distribuzione di un singolo server di Accesso remoto per la gestione dei client DirectAccess offre quanto indicato di seguito:  
  
-   **Accessibilità**: Gestito client computer che eseguono Windows 8 o Windows 7 possono essere configurati come computer client DirectAccess. Questi client possono accedere alle risorse di rete interne con DirectAccess ogni volta che sono connessi a Internet, senza dover accedere a una connessione VPN. I computer client che non eseguono uno di questi sistemi operativi possono connettersi alla rete interna tramite una VPN. DirectAccess e VPN sono gestiti nella stessa console e con lo stesso gruppo di procedure guidate.  
  
-   **Facilità di gestione**: i computer client DirectAccess connessi a Internet possono essere gestiti in modalità remota dagli amministratori di Accesso remoto con DirectAccess, anche quando i computer client non si trovano sulla rete aziendale interna. Per i computer client che non soddisfano i requisiti aziendali vengono eseguiti il monitoraggio e l'aggiornamento automatici tramite i server di gestione. È possibile gestire uno o più server di Accesso remoto da una singola Console di gestione Accesso remoto.  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per lo scenario.  
  
|Ruolo o funzionalità|Modalità di supporto dello scenario|  
|----------|-----------------|  
|*Ruolo Accesso remoto*|Questo ruolo viene installato e disinstallato con la console di Server Manager o Windows PowerShell. Questo ruolo comprende DirectAccess, una funzionalità precedentemente inclusa in Windows Server 2008 R2, e il servizio Routing e Accesso remoto che in precedenza era un servizio ruolo nel ruolo server Servizi di accesso e criteri di rete (NPAS). Il ruolo Accesso remoto è costituito da due componenti:<br /><br />1.  VPN DirectAccess e Servizio Routing e Accesso remoto (RRAS): DirectAccess e VPN vengono gestiti nella console Gestione Accesso remoto.<br />2.  RRAS: le funzionalità vengono gestite nella console di routing e Accesso remoto.<br /><br />Il ruolo del server Accesso remoto dipende dalle funzionalità seguenti:<br /><br />-Il Server web (IIS): necessaria per configurare il server dei percorsi di rete e un probe Web predefinito.<br />: Database interno di Windows: Usato per l'accounting locale sul server di Accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />: Per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta la gestione remota della console dell'interfaccia utente.<br />-Come opzione in un server che non è in esecuzione il ruolo server Accesso remoto. In questo caso, viene usata per la gestione remota di un server di Accesso remoto.<br /><br />Questa funzionalità include gli elementi seguenti:<br /><br />-Accesso remoto GUI e strumenti da riga di comando<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit (CMAK)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  
  
### <a name="server-requirements"></a>Requisiti del server  
  
-   Un computer che soddisfi i requisiti hardware per Windows Server 2016. Per altre informazioni, vedere Windows Server 2016 [requisiti di sistema](https://technet.microsoft.com/windows-server-docs/get-started/system-requirements-and-installation).  
  
-   Il server deve avere almeno una scheda di rete installata e abilitata. Dovrà essere presente una sola scheda connessa alla rete aziendale interna e una sola scheda connesso alla rete esterna (Internet).  
  
-   Se è richiesto Teredo come protocollo di transizione da IPv6 a IPv4, la scheda esterna del server richiede due indirizzi IPv4 pubblici consecutivi. Se è disponibile una singola scheda di rete, come protocollo di transito potrà essere usato solo IP-HTTPS.  
  
-   Almeno un controller di dominio. I server di Accesso remoto e i client DirectAccess devono essere membri di un dominio.  
  
-   È richiesta un'autorità di certificazione nel server se non si vogliono usare certificati autofirmati per IP-HTTPS o il server dei percorsi di rete oppure se si vogliono usare certificati client per l'autenticazione IPsec dei client.  
  
### <a name="client-requirements"></a>Requisiti client  
  
-   Un computer client deve essere in esecuzione Windows 10 o Windows 8 o Windows 7.  
  
### <a name="infrastructure-and-management-server-requirements"></a>Requisiti del server di gestione e infrastruttura  
  
-   Durante la gestione remota dei computer client DirectAccess, i client avviano le comunicazioni con i server di gestione quali controller di dominio, server System Center Configuration e server Autorità registrazione integrità. Questi server forniscono servizi che includono aggiornamenti di Windows e antivirus e conformità dei client di Protezione accesso alla rete. Distribuire i server necessari prima dell'inizio della distribuzione di Accesso remoto.  
  
-   È necessario un server DNS che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008 con SP2.  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
I requisiti software per questo scenario includono i seguenti.  
  
### <a name="server-requirements"></a>Requisiti del server  
  
-   Il server di Accesso remoto deve essere membro di un dominio. Il server può essere distribuito alla periferia della rete interna o dietro un firewall o un altro dispositivo periferico.  
  
-   Se il server di Accesso remoto di trova dietro un firewall periferico o un dispositivo NAT, il dispositivo deve essere configurato in modo da consentire il traffico da e verso il server di Accesso remoto.  
  
-   Gli amministratori che distribuiscono un server di Accesso remoto devono avere le autorizzazioni di amministratore locale nel server e le autorizzazioni a livello utente di dominio. L'amministratore deve avere anche le autorizzazioni per gli oggetti Criteri di gruppo usati nella distribuzione di DirectAccess. Per sfruttare le funzionalità che limitano la distribuzione di DirectAccess solo ai computer portatili, sono necessarie le autorizzazioni di amministratore per il dominio nel controller di dominio per creare un filtro WMI.  
  
-   Se il server del percorso di rete non si trova nel server di Accesso remoto, per eseguirlo è richiesto un server distinto.  
  
### <a name="remote-access-client-requirements"></a>Requisiti dei client di Accesso remoto  
  
-   I client DirectAccess devono essere membri di un dominio. I domini contenenti client possono appartenere alla stessa foresta del server di Accesso remoto oppure disporre di un trust bidirezionale con la foresta o il dominio di Accesso remoto.  
  
-   È richiesto un gruppo di sicurezza di Active Directory per contenere i computer che saranno configurati come client DirectAccess. I computer non dovranno essere inclusi in più di un gruppo di sicurezza che comprende client DirectAccess. Se i client sono inclusi in più gruppi, la risoluzione dei nomi per le richieste dei client non funzionerà come previsto.  
  

