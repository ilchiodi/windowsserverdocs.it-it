---
title: Distribuire l'accesso remoto con l'autenticazione OTP
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto con autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecb4503c6f8cbc08f2175a33a41929491e4a2b7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811992"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Distribuire l'accesso remoto con l'autenticazione OTP

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combina DirectAccess e Routing e accesso remoto \(RRAS\) VPN in un unico ruolo Accesso remoto.   

## <a name="BKMK_OVER"></a>Descrizione dello scenario  
In questo scenario un accesso remoto server con DirectAccess abilitato viene configurato per autenticare gli utenti di client DirectAccess con due\-password monouso factor \(OTP\) l'autenticazione, oltre a attivo standard Credenziali di directory.  
  
## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  
  
-   [Distribuire un DirectAccess Server singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) devono essere distribuiti prima della distribuzione OTP.  
  
-   I client di Windows 7 devono usare DCA 2.0 per supportare OTP.  
  
-   OTP non supporta la modifica del PIN.  
  
-   È necessario distribuire un'infrastruttura a chiave pubblica (PKI).  
  
    Per altre informazioni, vedi: [Mini-modulo della Guida al Lab di test: Base PKI per Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   La modifica dei criteri all'esterno di console Gestione DirectAccess o i cmdlet di Windows PowerShell non è supportata.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Lo scenario di autenticazione OTP include alcuni passaggi:  
  
1.  [Distribuire un Server DirectAccess singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un singolo server di accesso remoto deve essere distribuito prima di configurare OTP. La pianificazione e la distribuzione di un singolo server include la progettazione e la configurazione di una topologia di rete, la pianificazione e la distribuzione di certificati, la configurazione di DNS e Active Directory, la configurazione delle impostazioni del server di Accesso remoto, la distribuzione dei client DirectAccess e la preparazione dei server Intranet.  
  
2.  [Pianificare l'accesso remoto con l'autenticazione OTP](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Oltre alla pianificazione necessaria per un singolo server, OTP richiede la pianificazione per un'autorità di certificazione Microsoft \(autorità di certificazione\) e i modelli di certificato per OTP e un raggio\-server OTP abilitato. Pianificazione può includere anche un requisito per i gruppi di sicurezza esentino utenti specifici da strong \(OTP o smart card\) l'autenticazione. Per informazioni sulla configurazione di OTP in un multi\-ambiente della foresta, vedere [configurare una distribuzione a più foreste](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configurare DirectAccess con l'autenticazione OTP](/configure/Configure-RA-with-OTP-Authentication.md). Distribuzione OTP è costituita da un numero di passaggi di configurazione, inclusa la preparazione dell'infrastruttura per l'autenticazione OTP, la configurazione del server OTP, la configurazione delle impostazioni OTP sul server di accesso remoto e l'aggiornamento delle impostazioni client DirectAccess.  
  
4.  [Troubleshoot an OTP Deployment] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). In questa sezione sulla risoluzione dei problemi illustra alcuni degli errori più comuni che possono verificarsi quando si distribuisce accesso remoto con autenticazione OTP.  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
Aumento della protezione utilizzando OTP aumenta la sicurezza della distribuzione di DirectAccess. Un utente deve fornire le credenziali OTP per ottenere l'accesso alla rete interna. Un utente fornisce le credenziali OTP tramite le connessioni all'area di lavoro disponibili nelle connessioni di rete nel computer client Windows 10 o Windows 8 o tramite DirectAccess Connectivity Assistant \(DCA\) nei computer client che eseguono Windows 7. Il processo di autenticazione OTP funziona nel modo seguente:  
  
1.  Il client DirectAccess immette le credenziali di dominio per accedere ai server dell'infrastruttura DirectAccess \(attraverso il tunnel dell'infrastruttura\).  Se non sono disponibili connessioni alla rete interna, a causa di un errore IKE specifico, la connessione all'area di lavoro nel computer client indica all'utente che sono necessarie le credenziali. Nei computer client che eseguono Windows 7, un pop\-che richiede credenziali della smart card viene visualizzata.  
  
2.  Dopo avere immesso le credenziali OTP, vengono inviati tramite SSL al server di accesso remoto, insieme a una richiesta per un breve\-certificato di termine della smart card per l'accesso.  
  
3.  Il server di accesso remoto avvia la convalida delle credenziali OTP con raggio\-server OTP basato su.  
  
4.  Se l'operazione ha esito positivo, il server di Accesso remoto firma la richiesta di certificato usando il rispettivo certificato dell'autorità di registrazione, quindi lo restituisce al computer client DirectAccess.  
  
5.  Il computer client DirectAccess inoltra la richiesta di certificato firmato all'autorità di certificazione e archivia il certificato registrato per l'uso da SSP Kerberos\/Asia Pacifico.  
  
6.  Usando questo certificato, il computer client esegue in modo trasparente l'autenticazione Kerberos tramite smart card standard.  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per lo scenario.  
  
|Ruolo\/funzionalità|Modalità di supporto dello scenario|  
|---------|-----------------|  
|*Ruolo Gestione accesso remoto*|Il ruolo viene installato e disinstallato tramite la console di Server Manager. Questo ruolo comprende DirectAccess, che in precedenza era una funzionalità in Windows Server 2008 R2 e servizio Routing e i servizi di accesso remoto che in precedenza era un servizio ruolo in base ai criteri di rete e servizi di accesso \(NPAS\) server ruolo. Il ruolo Accesso remoto è costituito da due componenti:<br /><br />1.  DirectAccess e Routing e accesso remoto \(RRAS\) VPN DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br />2.  Funzionalità di routing di Routing RRAS routing vengono gestite nella console di Routing e accesso remoto legacy.<br /><br />Il ruolo Accesso remoto dipende dalle funzionalità server seguenti:<br /><br />-Internet Information Services \(IIS\) Server Web - questa funzionalità è necessaria per configurare il server dei percorsi di rete, utilizzare l'autenticazione OTP e configurare il probe web predefinito.<br />-Windows Database-Used interna per l'accounting locale sul server di accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />-Viene installato per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta l'interfaccia utente della console di gestione remota.<br />-Può essere installata facoltativamente in un server non è in esecuzione il ruolo di server di accesso remoto. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />-Accesso remoto GUI e strumenti da riga di comando<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit \(Connection Manager Administration Kit\)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  
  
-   Un computer che soddisfi i requisiti hardware per Windows Server 2016 o Windows Server 2012.  
  
-   Per testare lo scenario, è necessario almeno un computer che eseguono Windows 10, Windows 8 o Windows 7 configurato come un client DirectAccess.  
  
-   Un server OTP che supporta PAP su RADIUS.  
  
-   Un token hardware o software OTP.  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
Per questo scenario sono presenti numerosi requisiti.  
  
1.  Requisiti software per una distribuzione a server singolo. Per altre informazioni, vedere [distribuire un DirectAccess Server singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  Oltre ai requisiti software per un singolo server esistono una serie di OTP\-requisiti specifici:  
  
    1.  Autorità di certificazione per l'autenticazione IPsec una distribuzione OTP di che DirectAccess deve essere distribuito tramite IPsec macchine certificati emessi da un'autorità di certificazione. L'autenticazione IPsec tramite il server di Accesso remoto come proxy Kerberos non è supportata in una distribuzione OTP. È necessaria un'autorità di certificazione interna.  
  
    2.  Autorità di certificazione per OTP l'autenticazione A Microsoft CA dell'organizzazione \(in esecuzione su Windows 2003 Server o in un secondo momento\) è necessaria per l'emissione del certificato client OTP. È possibile usare la stessa autorità di certificazione con cui sono stati emessi i certificati per l'autenticazione IPsec. Il server dell'autorità di certificazione devono essere disponibili sul primo tunnel dell'infrastruttura.  
  
    3.  Sicurezza gruppo: per esentare gli utenti dall'autenticazione avanzata, è necessario un gruppo di sicurezza di Active Directory contenente questi utenti.  
  
    4.  Client\-requisiti lato-per Windows 10 e Windows 8 client computer, l'Assistente connettività di rete \(Assistente compilazione NATIVA\) servizio viene usato per rilevare se sono necessarie le credenziali OTP. In tal caso, gestione supporti DirectAccess richiederà le credenziali.  Assistente connettività di rete è incluso nel sistema operativo ed è necessaria alcuna installazione o la distribuzione. Per i computer client Windows 7, DirectAccess Connectivity Assistant \(DCA\) 2.0 è obbligatorio. disponibile nell' [Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Tenere presente quanto segue:  
  
        1.  L'autenticazione OTP può essere usata in parallelo con smart card e Trusted Platform Module \(TPM\)\-l'autenticazione basata su. Se si abilita l'autenticazione OTP nella console di gestione Accesso remoto, verrà abilitato anche l'uso dell'autenticazione tramite smart card.  
  
        2.  Durante gli utenti di configurazione di accesso remoto in una sicurezza specificata gruppo può essere esente da due\-autenticazione a due fattori e quindi eseguire l'autenticazione con nome utente\/solo la password.  
  
        3.  Le modalità tramite OTP, nuovo PIN e codice token successivo non sono supportate.  
  
        4.  In una distribuzione multisito di Accesso remoto le impostazioni OTP sono globali e consentono l'identificazione per tutti i punti di ingresso. Se vengono configurati più server RADIUS o dell'autorità di certificazione per OTP, i server verranno ordinati da ogni server di Accesso remoto in base alla disponibilità e alla prossimità.  
  
        5.  Quando si configura OTP in un accesso remoto più\-foresta ambiente, le autorità di certificazione OTP devono essere da solo la foresta delle risorse e registrazione del certificato deve essere configurata tra trust tra foreste. Per altre informazioni, vedere [Servizi certificati Active Directory: Registrazione certificato tra foreste con Windows Server 2008 R2](https://technet.microsoft.com/library/ff955842.aspx).  
  
        6.  Gli utenti che utilizzano un token OTP KEY FOB devono inserire il PIN seguito dal codice token \(senza separatori\) nella finestra di dialogo OTP di DirectAccess. Gli utenti che usano il token OTP PIN PAD devono inserire solo il codice token nella finestra di dialogo.  
  
        7.  Se WEBDAV è abilitato, non sarà necessario abilitare OTP.  
  
## <a name="KnownIssues"></a>Problemi noti  
Di seguito sono riportati alcuni problemi noti durante la configurazione di uno scenario OTP:  
  
-   Accesso remoto Usa un meccanismo di probe per verificare la connettività a RADIUS\-server OTP basati su. In alcuni casi è possibile che si verifichi un errore sul server OTP. Per evitare questo problema, eseguire le operazioni seguenti nel server OTP:  
  
    -   Creare un account utente corrispondente al nome utente e alla password configurati sul server di Accesso remoto per il meccanismo di tipo probe. Il nome utente non deve definire un utente di Active Directory.  
  
        Per impostazione predefinita, il nome utente sul server di Accesso remoto è DAProbeUser e la password è DAProbePass. Queste impostazioni predefinite possono essere modificate tramite i valori seguenti nel Registro di sistema sul server di Accesso remoto:  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   Se si modifica il certificato radice IPsec in una distribuzione DirectAccess configurata e in esecuzione, OTP smetterà di funzionare. Per risolvere questo problema, in ogni server DirectAccess, al prompt di Windows PowerShell, eseguire il comando: `iisreset`  
  
