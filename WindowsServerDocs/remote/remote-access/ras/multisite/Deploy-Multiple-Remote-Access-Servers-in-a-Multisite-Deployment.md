---
title: Distribuire più server di accesso remoto in una distribuzione multisito
description: Questo argomento fa parte della Guida alla distribuzione di più server di accesso remoto in una distribuzione multisito di Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3091c770fb9b207d82deaa571970bfeb44d17ee3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875882"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>Distribuire più server di accesso remoto in una distribuzione multisito

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combina DirectAccess e servizio di accesso remoto (RAS) VPN in un unico ruolo Accesso remoto. Accesso remoto può essere distribuito in molti scenari aziendali. In questa panoramica fornisce un'introduzione allo scenario enterprise per la distribuzione di server di accesso remoto in una configurazione multisito.  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
In una distribuzione multisito due o più server di accesso remoto o cluster di server vengono distribuiti e configurati come punti di ingresso diversi in un'unica posizione o in posizioni geografiche diverse. Distribuzione di più punti di ingresso in un'unica posizione consente di ridondanza dei server o per l'allineamento del server di accesso remoto con l'architettura di rete esistente. Per posizione geografica assicura un utilizzo efficiente delle risorse, come i computer client remoti possono connettersi alle risorse di rete interna usando un punto di ingresso più vicino. Il traffico attraverso una distribuzione multisito può essere distribuito e bilanciato con un servizio di bilanciamento del carico globale esterno.  
  
Una distribuzione multisito supporta computer client che eseguono Windows 10, Windows 8 o Windows 7. Identificare i computer client che eseguono Windows 8 o Windows 10 automaticamente un punto di ingresso, o l'utente può selezionare manualmente un punto di ingresso. L'assegnazione automatica si verifica nell'ordine di priorità seguente:  
  
1.  Utilizzare un punto di ingresso selezionato manualmente dall'utente.  
  
2.  Utilizzare un punto di ingresso identificato da un servizio di bilanciamento del carico globale esterno se uno è stato distribuito.  
  
3.  Utilizzare il punto di ingresso più vicino identificato dal computer client utilizzando un meccanismo di verifica automatica.  
  
Supporto per i client che eseguono Windows 7 deve essere attivato manualmente in ogni punto di ingresso e selezione di un punto di ingresso da questi client non è supportata.  
  
## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  
  
-   [Distribuire un DirectAccess Server singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) devono essere distribuiti prima di una distribuzione multisita.  
  
-   I client Windows 7 si connetteranno sempre a un sito specifico. Non saranno in grado di connettersi al sito più vicino in base alla posizione del client (a differenza dei client di Windows 10, 8 o 8.1).  
  
-   La modifica dei criteri all'esterno della console di gestione DirectAccess o con cmdlet di PowerShell non è supportata.  
  
-   È necessario distribuire un'infrastruttura a chiave pubblica (PKI).  
  
    Per altre informazioni, vedi: [Mini-modulo della Guida al Lab di test: Base PKI per Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   Rete aziendale deve essere abilitata a IPv6. Se si usa la tecnologia ISATAP, rimuoverla e usare la connettività IPv6 nativa.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Lo scenario di distribuzione multisito include una serie di passaggi:  
  
1. [Distribuire un server DirectAccess singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Prima di configurare una distribuzione multisito, è necessario distribuire un singolo server di accesso remoto con impostazioni avanzate.  
  
2. [Pianificare una distribuzione Multisita](plan/Plan-a-Multisite-Deployment.md). Per compilare un numero di ulteriori informazioni sulla pianificazione di una distribuzione multisito da un singolo server passaggi sono obbligatori, inclusa l'osservanza prerequisiti multisiti e pianificazione di gruppi di sicurezza di Active Directory, oggetti Criteri di gruppo (GPO), DNS e le impostazioni del client.  
  
3. [Configurare una distribuzione multisito](configure/Configure-a-Multisite-Deployment.md). Si tratta di un numero di passaggi di configurazione, inclusa la preparazione dell'infrastruttura di Active Directory, configurazione del server di accesso remoto esistente e l'aggiunta di più server di accesso remoto come punti di ingresso alla distribuzione multisito.  
  
4. [Risolvere i problemi relativi a una distribuzione multisito](troubleshoot/Troubleshoot-a-Multisite-Deployment.md). In questa sezione sulla risoluzione dei problemi descritti un numero di errori più comuni che possono verificarsi durante la distribuzione di accesso remoto in una distribuzione multisito.
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
Una distribuzione multisito fornisce quanto segue:  
  
-   Distribuzione multisita migliorati A prestazioni consente ai client computer che accedono a risorse interne tramite accesso remoto per connettersi utilizzando il punto di ingresso più vicino e più adatto. Client di accedere alle risorse interne in modo efficiente e la velocità dei client che Internet richieste indirizzate tramite DirectAccess è stata migliorata. Il traffico tra punti di ingresso può essere bilanciato utilizza un bilanciamento del carico globale esterno.  
  
-   Facilità di gestione multisito consente agli amministratori allineare la distribuzione di accesso remoto a una distribuzione di siti di Active Directory, offre un'architettura semplificata. Impostazioni condivise possono essere impostate con facilità tra cluster o server del punto di ingresso. Impostazioni di accesso remoto possono essere gestite da uno qualsiasi dei server nella distribuzione o in modalità remota utilizzando strumenti di amministrazione remota Server (RSAT). Inoltre, l'intera distribuzione multisito può essere monitorata da un'unica console di gestione accesso remoto.  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente sono elencati i ruoli e funzionalità in questo scenario.  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|---------|-----------------|  
|Ruolo Accesso remoto|Il ruolo viene installato e disinstallato tramite la console di Server Manager. Comprende DirectAccess, una funzionalità precedentemente inclusa in Windows Server 2008 R2, e il servizio Routing e Accesso remoto (RRAS) che in precedenza era un servizio ruolo nel ruolo del server Servizi di accesso e criteri di rete (NPAS). Il ruolo Accesso remoto è costituito da due componenti:<br /><br />-DirectAccess e Routing e accesso remoto (RRAS) VPN DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br />Funzionalità di routing di Routing RRAS routing e accesso REMOTO vengono gestite nella console di Routing e accesso remoto legacy.<br /><br />Le dipendenze sono le seguenti:<br /><br />-Internet Information Services (IIS) Web Server - questa funzionalità è necessaria configurare il server dei percorsi di rete e probe web predefinito.<br />-Windows Database-Used interna per l'accounting locale sul server di accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />-Viene installato per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta l'interfaccia utente della console di gestione remota.<br />-Può essere installata facoltativamente in un server non è in esecuzione il ruolo di server di accesso remoto. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />-Accesso remoto GUI e strumenti da riga di comando<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit (CMAK)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  
  
-   Almeno due computer di accesso remoto di raccogliere in una distribuzione multisito.   
  
-   Per testare lo scenario, è necessario almeno un computer che eseguono Windows 8 e configurato come client DirectAccess. Per testare lo scenario per i client che eseguono Windows 7 è richiesto almeno un computer che eseguono Windows 7.  
  
-   Per bilanciare il carico tra server del punto di ingresso, è necessario un bilanciamento del carico globale esterno di terze parti.  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
I requisiti software per questo scenario includono i seguenti.  
  
-   Requisiti software per una distribuzione a server singolo.  
  
-   Oltre ai requisiti software per un singolo server esistono una serie di requisiti multisite-specifici:  
  
    -   Autenticazione IPsec requisiti In che una distribuzione multisito DirectAccess deve essere distribuita utilizzando l'autenticazione del certificato computer IPsec. L'opzione per eseguire l'autenticazione IPsec utilizza il server di accesso remoto come un proxy Kerberos non è supportata. Una CA interna è necessario per distribuire i certificati IPsec.  
  
    -   Rete e IP-HTTPS percorso server requisiti-certificati necessari per il server dei percorsi di rete e IP-HTTPS devono essere emesso da un'autorità di certificazione. L'opzione per utilizzare i certificati automaticamente emesso e firmati dal server di accesso remoto non è supportata. Certificati possono essere rilasciati da una CA interna o da una CA esterna di terze parti.  
  
    -   Requisiti di Active Directory-è richiesto almeno un sito di Active Directory. Server di accesso remoto deve trovarsi nel sito. Per accelerare l'aggiornamento, è consigliabile che ogni sito dispone di un controller di dominio scrivibile, anche se ciò non è obbligatorio.  
  
    -   Sicurezza gruppo requisiti sono i seguenti:  
  
        -   Un singolo gruppo di sicurezza è necessario per tutti i computer client Windows 8 da tutti i domini. È consigliabile creare un gruppo di protezione di questi client per ogni dominio.  
  
        -   Un gruppo di sicurezza contenente i computer Windows 7 è necessario per ogni punto di ingresso configurato per supportare i client Windows 7. È consigliabile disporre di un gruppo di sicurezza univoco per ogni punto di ingresso in ogni dominio.  
  
        -   I computer non dovranno essere inclusi in più di un gruppo di sicurezza che comprende client DirectAccess. Se i client sono inclusi in più gruppi, la risoluzione dei nomi per le richieste dei client non funzionerà come previsto.  
  
    -   Oggetto Criteri di gruppo requisiti-oggetti Criteri di gruppo può essere creato manualmente prima di configurare accesso remoto o creato automaticamente durante la distribuzione di accesso remoto. Requisiti sono i seguenti:  
  
        -   Un oggetto Criteri di gruppo client univoco è necessaria per ogni dominio.  
  
        -   Un oggetto Criteri di gruppo di server è necessario per ogni punto di ingresso, nel dominio in cui si trova il punto di ingresso. Se più punti di ingresso si trovano nello stesso dominio, verrà server più oggetti Criteri di gruppo (uno per ogni punto di ingresso) nel dominio.  
  
        -   Un GPO univoco per il client Windows 7 è richiesto per ogni punto di ingresso abilitata per il supporto di client Windows 7, per ogni dominio.  
  
## <a name="KnownIssues"></a>Problemi noti  
Di seguito è problemi noti durante la configurazione di uno scenario multisito:  
  
-   **Più punti di ingresso nella stessa subnet IPv4**. L'aggiunta di più punti di ingresso nella stessa subnet IPv4 si otterrà un messaggio di conflitto di indirizzo IP e l'indirizzo DNS64 per il punto di ingresso non verrà configurato come previsto. Questo problema si verifica quando IPv6 non è stato distribuito sulle interfacce del server nella rete aziendale interne. Per evitare questo problema, eseguire il seguente comando di Windows PowerShell in tutti i server di accesso remoto correnti e futuri:  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   Se l'indirizzo pubblico specificato per i client DirectAccess di connettersi al server di accesso remoto dispone di un suffisso della risoluzione dei NOMI, DirectAccess potrebbe non funzionare come previsto. Assicurarsi che la risoluzione dei NOMI disponga di un'esenzione per il nome pubblico. In una distribuzione multisito, per i nomi di tutti i punti di ingresso pubblici devono essere aggiunte le esenzioni. Si noti che se forzare il tunneling sono attivata vengono aggiunti automaticamente tali esenzioni. Questi vengono rimossi se il tunneling forzato è disabilitato.  
  
-   Quando si utilizza il cmdlet Windows PowerShell **Disable-DAMultiSite**, i parametri WhatIf and Confirm non hanno effetto e multisito verrà disabilitata e verrà rimosso oggetti Criteri di gruppo di Windows 7.  
  
-   Quando i client Windows 7 mediante DCA in una distribuzione multisito vengono aggiornati a Windows 8, l'Assistente connettività di rete non funzionerà. Questo problema può essere risolto prima l'aggiornamento client modificando i GPO di Windows 7 tramite i cmdlet di Windows PowerShell seguenti:  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    Nel caso in cui il client è già stato aggiornato, quindi spostare il computer client al gruppo di sicurezza di Windows 8.  
  
-   Quando si modificano le impostazioni di controller di dominio utilizzando il cmdlet Windows PowerShell **Set-DAEntryPointDC**, se il parametro ComputerName specificato è un server di accesso remoto in un punto di ingresso diverso dall'ultimo aggiunto alla distribuzione multisito, verrà visualizzato un avviso che indica che il server specificato non verrà aggiornato fino al successivo aggiornamento di criteri. Il server effettivo che non è stato aggiornato può essere visualizzato utilizzando il **lo stato di configurazione** nel **DASHBOARD** del **Console Gestione accesso remoto**. In questo modo non problemi funzionali, tuttavia, è possibile eseguire **gpupdate /force** nei server che non è stato aggiornato per ottenere lo stato di configurazione aggiornato immediatamente.  
  
-   Quando la funzionalità multisito viene distribuita in una rete di aziendale solo IPv4, la modifica interna prefisso IPv6 anche modifiche DNS64 l'indirizzo di rete, ma non aggiorna l'indirizzo sulle regole del firewall che consentono alle query DNS per il servizio DNS64. Per risolvere questo problema, eseguire i comandi di Windows PowerShell seguenti dopo la modifica del prefisso IPv6 della rete interna:  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   Se DirectAccess è stata distribuita un'infrastruttura ISATAP esistente non presente, quando si rimuove un punto di ingresso di un host ISATAP, l'indirizzo IPv6 del servizio DNS64 verrà rimossa da indirizzi server DNS di tutti i suffissi DNS nella tabella NRPT.  
  
    Per risolvere questo problema, nel **configurazione Server di infrastruttura** procedura guidata di **DNS** pagina, rimuovere i suffissi DNS che sono stati modificati e aggiungerli nuovamente con gli indirizzi di server DNS corretti, fare clic su **rileva** sul **indirizzi Server DNS** la finestra di dialogo.  
  


