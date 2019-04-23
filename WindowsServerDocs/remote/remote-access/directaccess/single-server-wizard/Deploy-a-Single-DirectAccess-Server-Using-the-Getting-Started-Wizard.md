---
title: Distribuire un server DirectAccess singolo usando Attività iniziali guidate
description: Questo argomento fa parte della Guida di distribuire un Server DirectAccess singolo con l'introduzione avvio procedura guidata per Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb0cf464-0668-40f8-8222-feb6bae6d3d5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d2bab7abd8acd958ccb0b0c1d64bc4245b61be36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854072"
---
# <a name="deploy-a-single-directaccess-server-using-the-getting-started-wizard"></a>Distribuire un server DirectAccess singolo usando Attività iniziali guidate

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento fornisce un'introduzione allo scenario DirectAccess in cui viene usato un server DirectAccess singolo e consente di distribuire DirectAccess in pochi, semplici passaggi.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Prima di iniziare la distribuzione vedere l'elenco di configurazioni non supportate, problemi noti e prerequisiti  
È possibile utilizzare gli argomenti seguenti per esaminare i prerequisiti e altre informazioni prima di distribuire DirectAccess.  
  
-   [DirectAccess configurazioni non supportate](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Prerequisiti per la distribuzione di DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
In questo scenario, un singolo computer che esegue Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, è configurato come server DirectAccess con le impostazioni predefinite in pochi passaggi della procedura guidata semplice, senza dover configurare tali impostazioni dell'infrastruttura come un'autorità di certificazione (CA) o i gruppi di sicurezza di Active Directory.  
  
> [!NOTE]  
> Per configurare una distribuzione avanzata con impostazioni personalizzate, vedere [Deploy a Single DirectAccess Server with Advanced Settings](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
  
## <a name="in-this-scenario"></a>In questo scenario  
Per configurare un server DirectAccess di base, sono necessari diversi passaggi di pianificazione e distribuzione.  
  
### <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  
  
-   Windows Firewall deve essere abilitato in tutti i profili  
  
-   Questo scenario è supportato solo quando i computer client sono in esecuzione Windows 10, Windows 8.1 o Windows 8.  
  
-   La tecnologia ISATAP non è supportata nella rete aziendale. Se si usa la tecnologia ISATAP, rimuoverla e usare la connettività IPv6 nativa.  
  
-   Non è necessaria un'infrastruttura a chiave pubblica (PKI).  
  
-   Non supportato per la distribuzione dell'autenticazione a due fattori. Le credenziali di dominio sono necessarie per l'autenticazione.  
  
-   Distribuisce automaticamente DirectAccess a tutti i computer portatili nel dominio corrente.  
  
-   Il traffico a Internet non viene trasmesso nel tunnel DirectAccess. La configurazione del tunneling forzato non è supportata.  
  
-   Il server DirectAccess è il server dei percorsi di rete.  
  
-   La Protezione accesso alla rete (NAP) non è supportata.  
  
-   La modifica dei criteri all'esterno della console di gestione DirectAccess o con cmdlet di PowerShell non è supportata.  
  
-   Per la distribuzione multisito, ora o in futuro, innanzitutto [distribuire un DirectAccess Server singolo con impostazioni avanzate](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
### <a name="planning-steps"></a>Passaggi di pianificazione  
La pianificazione è suddivisa in due fasi:  
  
1.  Pianificare l'infrastruttura DirectAccess. In questa fase viene descritta la pianificazione necessaria per impostare l'infrastruttura di rete prima di iniziare la distribuzione DirectAccess. Include la pianificazione della rete e della topologia del server e del server dei percorsi di rete di DirectAccess.  
  
2.  Pianificazione della distribuzione di DirectAccess. In questa fase vengono descritti i passaggi di pianificazione necessari per la preparazione della distribuzione di DirectAccess. Include la pianificazione per i computer client DirectAccess, i requisiti per l'autenticazione client e server, le impostazioni VPN, i server dell'infrastruttura e i server applicazioni e di gestione.  
  
Per passaggi di pianificazione dettagliati, vedere [pianificare una distribuzione avanzata DirectAccess](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
### <a name="deployment-steps"></a>Fasi di distribuzione  
La distribuzione è suddivisa in tre fasi:  
  
1.  La fase di questa infrastruttura DirectAccess la configurazione include la configurazione di rete e routing, delle impostazioni del firewall se necessario, configurare i certificati, server DNS, le impostazioni di Active Directory e oggetti Criteri di gruppo e il percorso di rete di DirectAccess Server.  
  
2.  Configurazione delle impostazioni server DirectAccess. Questa fase include i passaggi per la configurazione dei computer client DirectAccess, del server DirectAccess, dei server dell'infrastruttura e dei server applicazioni e di gestione.  
  
3.  Verifica della distribuzione. Questa fase include i passaggi per verificare che la distribuzione funzioni come richiesto.  
  
Per informazioni dettagliate sui passaggi per la distribuzione, vedere [Install and Configure Basic DirectAccess](../../../remote-access/directaccess/single-server-wizard/Install-and-Configure-Basic-DirectAccess.md).  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
La distribuzione di un singolo server di Accesso remoto offre:  
  
-   Facilità di accesso. È possibile configurare i computer client gestiti che eseguono Windows 10, Windows 8.1, Windows 8 o Windows 7, come i client DirectAccess. Questi client possono accedere alle risorse di rete interne tramite DirectAccess ogni volta che sono connessi a Internet, senza dover accedere a una connessione VPN I computer client che non eseguono uno di questi sistemi operativi possono connettersi alla rete interna usando le tradizionali connessioni VPN.  
  
-   Facilità di gestione. I computer client DirectAccess su Internet possono essere gestiti in modalità remota dagli amministratori di Accesso remoto tramite DirectAccess, anche quando i computer client non si trovano sulla rete aziendale interna. Per i computer client che non soddisfano i requisiti aziendali vengono eseguiti il monitoraggio e l'aggiornamento automatici tramite i server di gestione. Sia DirectAccess che la VPN sono gestiti nella stessa console e con lo stesso gruppo di procedure guidate. Inoltre, è possibile gestire uno o più server di Accesso remoto da una singola console di gestione di Accesso remoto  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per lo scenario.  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|---------|-----------------|  
|Ruolo Accesso remoto|Il ruolo viene installato e disinstallato tramite la console di Server Manager o Windows PowerShell. Questo ruolo comprende DirectAccess, una funzionalità precedentemente inclusa in Windows Server 2008 R2, e il servizio Routing e Accesso remoto che in precedenza era un servizio ruolo nel ruolo server Servizi di accesso e criteri di rete (NPAS). Il ruolo Accesso remoto è costituito da due componenti:<br /><br />1.  DirectAccess e Routing e accesso remoto VPN (RRAS) di servizi. DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br />2.  RRAS Routing. Le funzionalità di routing RRAS sono gestite nella console di Routing e accesso remoto legacy.<br /><br />Il ruolo del server di Accesso remoto dipende dai ruoli/funzionalità del server seguenti:<br /><br />-Internet Information Services (IIS) Web Server - questa funzionalità è necessaria configurare il server dei percorsi rete sul server di accesso remoto e del probe web predefinito.<br />-Database interno di Windows. Usato per l'accounting locale sul server di Accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />-Viene installato per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta l'interfaccia utente della console di gestione remota e i cmdlet di Windows PowerShell.<br />-Può essere installata facoltativamente in un server non è in esecuzione il ruolo di server di accesso remoto. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />-Accesso remoto GUI<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit (CMAK)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  
  
-   Requisiti del server:  
  
    -   Un computer che soddisfi i requisiti hardware per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
    -   Il server deve avere almeno una scheda di rete installata, abilitata e connessa alla rete interna. Quando si usano due schede, una di esse deve essere connessa alla rete aziendale interna e l'altra alla rete esterna (Internet o rete privata).  
  
    -   Almeno un controller di dominio. Il server di Accesso remoto e il client DirectAccess devono essere membri di un dominio.  
  
-   Requisiti client  
  
    -   Un computer client deve essere in esecuzione Windows 10, Windows 8.1 o Windows 8.  
  
        > [!IMPORTANT]  
        > Se alcuni o tutti i computer client sono in esecuzione Windows 7, è necessario utilizzare la configurazione guidata avanzata. Attività iniziali installazione guidate descritta in questo documento non supporta i computer client che eseguono Windows 7. Visualizzare [distribuire un DirectAccess Server singolo con impostazioni avanzate](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) per istruzioni su come usare i client Windows 7 con DirectAccess.  
  
        > [!NOTE]  
        > Solo i seguenti sistemi operativi possono essere usati come client DirectAccess: Windows 10 Enterprise, Windows 8.1 Enterprise, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8 Enterprise, Windows Server 2008 R2, Windows 7 Enterprise e Ultimate di Windows 7.  
  
-   Requisii del server di gestione e infrastruttura:  
  
    -   Se si abilita VPN e non è configurato un pool di indirizzi IP statico, è necessario distribuire un server DHCP per allocare automaticamente gli indirizzi IP ai client VPN.  
  
-   Un server DNS che esegue Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 SP2 o Windows Server 2008 R2 è obbligatorio.  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
Per questo scenario sono presenti numerosi requisiti.  
  
-   Requisiti del server:  
  
    -   Il server di Accesso remoto deve essere membro di un dominio. Il server può essere distribuito alla periferia della rete interna o dietro un firewall o un altro dispositivo periferico.  
  
    -   Se il server di Accesso remoto di trova dietro un firewall periferico o un dispositivo NAT, il dispositivo deve essere configurato in modo da consentire il traffico da e verso il server di Accesso remoto.  
  
    -   La persona che distribuisce Accesso remoto sul server deve disporre di autorizzazioni di amministratore locale sul server e di autorizzazioni a livello utente di dominio. L'amministratore deve inoltre disporre di autorizzazioni per gli oggetti Criteri di gruppo utilizzati nella distribuzione di DirectAccess. Per sfruttare le funzionalità che limitano la distribuzione di DirectAccess solo ai computer portatili, sono richieste autorizzazioni per creare un filtro WMI nel controller di dominio.  
  
-   Requisiti dei client di Accesso remoto:  
  
    -   I client DirectAccess devono essere membri di un dominio. I domini che contengono client possono appartenere alla stessa foresta del server di Accesso remoto oppure avere un trust bidirezionale con la foresta di server di Accesso remoto.  
  
    -   È richiesto un gruppo di sicurezza di Active Directory per contenere i computer che saranno configurati come client DirectAccess. Se un gruppo di sicurezza non è specificato durante la configurazione delle impostazioni del client DirectAccess, per impostazione predefinita l'oggetto Criteri di gruppo del client viene applicato su tutti i computer portatili nel gruppo di sicurezza Computer del dominio. Solo i seguenti sistemi operativi possono essere usati come client DirectAccess:  Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 Enterprise e Ultimate di Windows 7.  
  
## <a name="BKMK_LINKS"></a>Vedere anche  
Nella tabella seguente vengono forniti i collegamenti a risorse aggiuntive.  
  
|Tipo di contenuto|Riferimenti|  
|--------|-------|  
|**Accesso remoto su TechNet**|[TechCenter di accesso remoto](https://technet.microsoft.com/network/bb545655.aspx)|  
|**Strumenti e impostazioni**|[Cmdlet di PowerShell per Accesso remoto](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Risorse della community**|[Voci di DirectAccess Wiki](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Tecnologie correlate**|[Funzionamento di IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


