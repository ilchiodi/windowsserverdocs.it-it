---
title: Distribuire l'accesso remoto con l'autenticazione OTP
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d0de5f459e31e1dfac40e49cd6cc83de8722df4d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404425"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Distribuire l'accesso remoto con l'autenticazione OTP

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combinano DirectAccess e il servizio Routing e accesso remoto \(RRAS\) VPN in un singolo ruolo accesso remoto.   

## <a name="BKMK_OVER"></a>Descrizione dello scenario  
In questo scenario un server di accesso remoto con DirectAccess abilitato viene configurato per autenticare gli utenti del client DirectAccess con due\-Factor password monouso \(OTP\) Authentication, oltre alle credenziali Active Directory standard.  
  
## <a name="prerequisites"></a>Prerequisiti  
Prima di iniziare a distribuire questo scenario, esaminare l'elenco dei requisiti importanti:  
  
-   [Distribuire un server DirectAccess singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) deve essere distribuito prima di distribuire OTP.  
  
-   I client Windows 7 devono usare DCA 2,0 per supportare OTP.  
  
-   OTP non supporta la modifica del PIN.  
  
-   È necessario distribuire un'infrastruttura a chiave pubblica (PKI).  
  
    Per altre informazioni, vedere: [Mini-modulo della guida al lab di test: Infrastruttura a chiave pubblica di base per Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   La modifica dei criteri all'esterno della console di Gestione DirectAccess o dei cmdlet di Windows PowerShell non è supportata.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Lo scenario di autenticazione OTP include alcuni passaggi:  
  
1.  [Distribuire un server DirectAccess singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). È necessario distribuire un singolo server di accesso remoto prima di configurare OTP. La pianificazione e la distribuzione di un singolo server include la progettazione e la configurazione di una topologia di rete, la pianificazione e la distribuzione di certificati, la configurazione di DNS e Active Directory, la configurazione delle impostazioni del server di Accesso remoto, la distribuzione dei client DirectAccess e la preparazione dei server Intranet.  
  
2.  [Pianificare l'accesso remoto con l'autenticazione OTP](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Oltre alla pianificazione necessaria per un singolo server, OTP richiede la pianificazione per un'autorità di certificazione Microsoft \(CA\) e i modelli di certificato per OTP; e un RADIUS\-server OTP abilitato. La pianificazione potrebbe includere anche un requisito per consentire ai gruppi di sicurezza di esentare utenti specifici dall'autenticazione \(OTP o smart card\) avanzata. Per informazioni sulla configurazione di OTP in un ambiente con foreste a più\-, vedere [configurare una distribuzione a più foreste](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configurare DirectAccess con l'autenticazione OTP](/configure/Configure-RA-with-OTP-Authentication.md). La distribuzione OTP è costituita da una serie di passaggi di configurazione, tra cui la preparazione dell'infrastruttura per l'autenticazione OTP, la configurazione del server OTP, la configurazione delle impostazioni OTP sul server di accesso remoto e l'aggiornamento delle impostazioni del client DirectAccess.  
  
4.  [Risolvere i problemi relativi a una distribuzione OTP] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). Questa sezione sulla risoluzione dei problemi descrive alcuni degli errori più comuni che possono verificarsi quando si distribuisce accesso remoto con l'autenticazione OTP.  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
Aumentare la sicurezza: l'uso di OTP aumenta la sicurezza della distribuzione di DirectAccess. Un utente deve fornire le credenziali OTP per ottenere l'accesso alla rete interna. Un utente fornisce le credenziali OTP tramite le connessioni all'area di lavoro disponibili nelle connessioni di rete sul computer client Windows 10 o Windows 8 oppure tramite DirectAccess Connectivity Assistant \(DCA\) nei computer client che eseguono Windows 7. Il processo di autenticazione OTP funziona nel modo seguente:  
  
1.  Il client DirectAccess immette le credenziali del dominio per accedere ai server dell'infrastruttura DirectAccess \(tramite il tunnel dell'infrastruttura\).  Se non sono disponibili connessioni alla rete interna, a causa di un errore IKE specifico, la connessione all'area di lavoro nel computer client indica all'utente che sono necessarie le credenziali. Nei computer client che eseguono Windows 7, viene visualizzato un pop\-per richiedere le credenziali della smart card.  
  
2.  Una volta immesse, le credenziali OTP vengono inviate tramite SSL al server di accesso remoto, insieme a una richiesta per un breve termine\-certificato di accesso con smart card.  
  
3.  Il server di accesso remoto avvia la convalida delle credenziali OTP con il server OTP basato su RADIUS\-.  
  
4.  Se l'operazione ha esito positivo, il server di Accesso remoto firma la richiesta di certificato usando il rispettivo certificato dell'autorità di registrazione, quindi lo restituisce al computer client DirectAccess.  
  
5.  Il computer client DirectAccess trasmette la richiesta di certificato firmato all'autorità di certificazione e archivia il certificato registrato per l'uso da parte del provider di servizi condivisi Kerberos\/AP.  
  
6.  Usando questo certificato, il computer client esegue in modo trasparente l'autenticazione Kerberos tramite smart card standard.  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità inclusi in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità richiesti per lo scenario.  
  
|Funzionalità\/ruoli|Modalità di supporto dello scenario|  
|---------|-----------------|  
|*Ruolo gestione accesso remoto*|Il ruolo viene installato e disinstallato tramite la console di Server Manager. Questo ruolo comprende sia DirectAccess, che in precedenza era una funzionalità di Windows Server 2008 R2, che servizi routing e accesso remoto che in precedenza era un servizio ruolo in servizi di accesso e criteri di rete \(NPAS\) ruolo server. Il ruolo Accesso remoto è costituito da due componenti:<br /><br />1. DirectAccess e servizi routing e accesso remoto \(RRAS\) VPN: DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br />2. routing RRAS: le funzionalità di routing RRAS sono gestite nella console di routing e accesso remoto legacy.<br /><br />Il ruolo Accesso remoto dipende dalle funzionalità server seguenti:<br /><br />-Internet Information Services \(server Web IIS\): questa funzionalità è necessaria per configurare il server dei percorsi di rete, utilizzare l'autenticazione OTP e configurare il probe Web predefinito.<br />-Windows Database-Used interna per l'accounting locale sul server di accesso remoto.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />-Viene installato per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta l'interfaccia utente della console di gestione remota.<br />-Può essere installata facoltativamente in un server non è in esecuzione il ruolo di server di accesso remoto. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />-Accesso remoto GUI e strumenti da riga di comando<br />-Modulo di accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />-Console Gestione criteri di gruppo<br />-RAS Connection Manager Administration Kit \(CMAK\)<br />-Windows PowerShell 3.0<br />-Infrastruttura e strumenti di gestione grafico|  
  
## <a name="BKMK_HARD"></a>Requisiti hardware  
I requisiti hardware per questo scenario includono i seguenti.  
  
-   Un computer che soddisfi i requisiti hardware per Windows Server 2016 o Windows Server 2012.  
  
-   Per testare lo scenario, è necessario almeno un computer che esegue Windows 10, Windows 8 o Windows 7 configurato come client DirectAccess.  
  
-   Un server OTP che supporta PAP su RADIUS.  
  
-   Un token hardware o software OTP.  
  
## <a name="BKMK_SOFT"></a>Requisiti software  
Per questo scenario sono presenti numerosi requisiti.  
  
1.  Requisiti software per una distribuzione a server singolo. Per ulteriori informazioni, vedere la pagina relativa alla [distribuzione di un server DirectAccess singolo con impostazioni avanzate](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  Oltre ai requisiti software per un singolo server, esistono diversi OTP\-requisiti specifici:  
  
    1.  Autorità di certificazione per l'autenticazione IPsec: in una distribuzione OTP è necessario distribuire DirectAccess usando i certificati dei computer IPsec emessi da un'autorità di certificazione. L'autenticazione IPsec tramite il server di Accesso remoto come proxy Kerberos non è supportata in una distribuzione OTP. È necessaria un'autorità di certificazione interna.  
  
    2.  AUTORITÀ di certificazione per l'autenticazione OTP: una CA globale (Enterprise) Microsoft \(in esecuzione in Windows 2003 Server o versioni successive\) è necessaria per emettere il certificato client OTP. È possibile usare la stessa autorità di certificazione con cui sono stati emessi i certificati per l'autenticazione IPsec. Il server dell'autorità di certificazione devono essere disponibili sul primo tunnel dell'infrastruttura.  
  
    3.  Gruppo di sicurezza: per esentare gli utenti dall'autenticazione avanzata, è necessario un Active Directory gruppo di sicurezza contenente questi utenti.  
  
    4.  Requisiti lato client\-: per i computer client Windows 10 e Windows 8, l'assistente per la connettività di rete \(il servizio\) è usato per rilevare se sono necessarie le credenziali OTP. In tal caso, il gestore multimediale DirectAccess richiede le credenziali.  Il servizio di gestione delle autorità di gestione è incluso nel sistema operativo e non è necessaria alcuna installazione o distribuzione. Per i computer client Windows 7 è necessario DirectAccess Connectivity Assistant \(DCA\) 2,0. disponibile nell' [Area download Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Tieni presente quanto segue:  
  
        1.  L'autenticazione OTP può essere usata in parallelo con smart card e Trusted Platform Module \(TPM\)l'autenticazione basata \-. Se si abilita l'autenticazione OTP nella console di gestione Accesso remoto, verrà abilitato anche l'uso dell'autenticazione tramite smart card.  
  
        2.  Durante la configurazione di accesso remoto, gli utenti in un gruppo di sicurezza specificato possono essere esentati da due\-autenticazione del fattore e quindi eseguire l'autenticazione con il nome utente\/solo password.  
  
        3.  Le modalità tramite OTP, nuovo PIN e codice token successivo non sono supportate.  
  
        4.  In una distribuzione multisito di Accesso remoto le impostazioni OTP sono globali e consentono l'identificazione per tutti i punti di ingresso. Se vengono configurati più server RADIUS o dell'autorità di certificazione per OTP, i server verranno ordinati da ogni server di Accesso remoto in base alla disponibilità e alla prossimità.  
  
        5.  Quando si configura OTP in un ambiente di foresta con più\-di accesso remoto, le autorità di certificazione OTP devono provenire solo dalla foresta delle risorse e la registrazione dei certificati deve essere configurata tra i trust tra foreste. Per altre informazioni, vedere [Servizi certificati Active Directory: Registrazione di certificati tra foreste con Windows Server 2008 R2](https://technet.microsoft.com/library/ff955842.aspx).  
  
        6.  Gli utenti che usano un token OTP di KEY FOB devono inserire il PIN seguito da codice token \(senza separatori\) nella finestra di dialogo OTP di DirectAccess. Gli utenti che usano il token OTP PIN PAD devono inserire solo il codice token nella finestra di dialogo.  
  
        7.  Se WEBDAV è abilitato, non sarà necessario abilitare OTP.  
  
## <a name="KnownIssues"></a>Problemi noti  
Di seguito sono riportati alcuni problemi noti durante la configurazione di uno scenario OTP:  
  
-   Accesso remoto usa un meccanismo di probe per verificare la connettività ai server OTP basati su RADIUS\-. In alcuni casi è possibile che si verifichi un errore sul server OTP. Per evitare questo problema, eseguire le operazioni seguenti nel server OTP:  
  
    -   Creare un account utente corrispondente al nome utente e alla password configurati sul server di Accesso remoto per il meccanismo di tipo probe. Il nome utente non deve definire un utente di Active Directory.  
  
        Per impostazione predefinita, il nome utente sul server di Accesso remoto è DAProbeUser e la password è DAProbePass. Queste impostazioni predefinite possono essere modificate tramite i valori seguenti nel Registro di sistema sul server di Accesso remoto:  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   Se si modifica il certificato radice IPsec in una distribuzione DirectAccess configurata e in esecuzione, OTP smetterà di funzionare. Per risolvere il problema, in ogni server DirectAccess, al prompt di Windows PowerShell, eseguire il comando: `iisreset`  
  
