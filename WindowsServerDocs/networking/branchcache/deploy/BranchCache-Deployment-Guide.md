---
title: Guida alla distribuzione di BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: lizross
author: eross-msft
ms.openlocfilehash: df6287f35c3fbf397df3b4d61812db3b14f3ce38
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318510"
---
# <a name="branchcache-deployment-guide"></a>Guida alla distribuzione di BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questa guida per informazioni su come distribuire BranchCache in Windows Server 2016.  
  
Oltre a questo argomento, questa guida contiene le sezioni seguenti.  
  
-   [Scelta di una progettazione di BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Distribuire BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Cenni preliminari sulla distribuzione di BranchCache

BranchCache è una tecnologia di ottimizzazione della larghezza di banda di rete WAN WAN inclusa in alcune edizioni di Windows Server 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012, Windows Server&reg; 2008 R2 e correlati i sistemi operativi client Windows.  
  
Per ottimizzare la larghezza di banda della rete WAN, BranchCache copia il contenuto dai server di contenuti della sede principale e lo memorizza nella cache presso le filiali, consentendo ai computer client delle filiali di accedere al contenuto localmente anziché tramite la rete WAN.  
  
Nelle filiali, il contenuto viene memorizzato nella cache sul server che eseguono la funzionalità BranchCache di Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 - o, se non sono server disponibili nella succursale, il contenuto viene memorizzato nella cache nei computer client che eseguono Windows 10&reg;, Windows&reg; 8.1, Windows 8 o Windows 7&reg; .  
  
Dopo che un client richiede e riceve il contenuto dal Data Center principale di office o cloud e il contenuto viene memorizzato nella cache nella succursale, altri computer della stessa succursale possono ottenere il contenuto localmente anziché contattando il server di contenuti tramite il collegamento WAN.  
  
**Vantaggi della distribuzione di BranchCache**  
  
File di cache BranchCache, web e il contenuto dell'applicazione degli uffici di filiale, consentendo ai computer client accedere ai dati tramite la rete locale (LAN) anziché accedere al contenuto su connessioni WAN lente.  
  
BranchCache consente di ridurre sia il traffico WAN e il tempo necessario per gli utenti delle succursali aprire i file nella rete.  BranchCache offre sempre gli utenti con i dati più recenti e consente di proteggere la sicurezza del contenuto mediante la crittografia cache sul server cache ospitata e sui computer client.  
  
### <a name="what-this-guide-provides"></a>Informazioni contenute in questa guida  
Questa guida di distribuzione consente di distribuire BranchCache nelle modalità seguenti:  
  
-   Modalità Cache distribuita. In questa modalità, computer client della filiale scaricare contenuto dal server di contenuti nella sede centrale o cloud e poi memorizza nella cache il contenuto per gli altri computer della succursale stessa. La modalità Cache distribuita non richiede un computer server nella filiale.  
  
-   Modalità Cache ospitata. In questa modalità, branch office download dei computer client contenuti dal server di contenuti nella sede centrale o cloud e un server cache ospitata recupera il contenuto dai client. Il server cache ospitata memorizza quindi nella cache il contenuto per altri computer client.  
  
In questa guida vengono inoltre fornite istruzioni sulla distribuzione di tre tipi di server di contenuti. Sui server di contenuti è disponibile il contenuto di origine scaricato dai computer client della filiale ed è necessario uno o più server di contenuti per distribuire BranchCache in entrambe le modalità. Di seguito sono elencati i tipi di server di contenuti:  
  
-   **Server del contenuto basato su server Web**. Questi server di contenuti inviano contenuto ai computer client con BranchCache utilizzando i protocolli HTTP e HTTPS. Questi server di contenuti devono essere in esecuzione versioni di Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 che supportano BranchCache e in cui è installata la funzionalità BranchCache.  
  
-   **Server applicazioni basati su BITS**. Questi server di contenuti inviano contenuto ai computer client con BranchCache utilizzando il Servizio trasferimento intelligente in background (BITS). Questi server di contenuti devono essere in esecuzione versioni di Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 che supportano BranchCache e in cui è installata la funzionalità BranchCache.  
  
-   **File server di contenuti basati su server**. Questi server di contenuti devono essere in esecuzione versioni di Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 che supportano BranchCache e dopo che il File Services è installato il ruolo server. È inoltre necessario che sia installato e configurato il servizio ruolo **BranchCache per file di rete** del servizio ruolo Servizi file. Questi server di contenuti inviano contenuto ai computer client con BranchCache utilizzando il protocollo SMB (Server Message Block).  
  
Per ulteriori informazioni, vedere [versioni del sistema operativo per BranchCache](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Requisiti di distribuzione di BranchCache

Di seguito sono i requisiti per la distribuzione di BranchCache utilizzando questa Guida.  
  
-   **Server di contenuti Web e file** deve essere in esecuzione uno dei seguenti sistemi operativi per fornire la funzionalità BranchCache: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2. Windows 8 e versioni successive client continua a visualizzare i vantaggi di BranchCache quando si accede a contenuto nei server che eseguono Windows Server 2008 R2, tuttavia non sono in grado di rendere utilizzo della nuova chunking e hashing tecnologie in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012.  
  
-   **I computer client** deve essere in esecuzione Windows 10, Windows 8.1 o Windows 8 per rendere utilizzare il modello di distribuzione più recente suddivisione in blocchi e di hashing miglioramenti introdotti con Windows Server 2012.  
  
-   **Server cache ospitata** deve essere in esecuzione Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 per utilizzare i miglioramenti di distribuzione e scalabilità delle funzionalità descritte in questo documento.  Un computer che esegue uno di questi sistemi operativi che è stato configurato come un server cache ospitata può continuare a servire i computer client che eseguono Windows 7, ma a tale scopo, deve essere fornito con un certificato adatto per la sicurezza TLS (Transport Layer), come descritto nella Windows Server 2008 R2 e Windows 7 [Guida alla distribuzione di BranchCache](https://technet.microsoft.com/library/ee649232.aspx).  
  
-   **Un dominio Active Directory** è necessaria per sfruttare i vantaggi di criteri di gruppo e il rilevamento automatico della cache ospitata, ma un dominio non è necessario utilizzare BranchCache.  È possibile configurare singoli computer utilizzando Windows PowerShell. Inoltre, non è necessario che i controller di dominio eseguono Windows Server 2012 o versione successiva per utilizzare nuove impostazioni dei criteri di gruppo BranchCache; è possibile importare i modelli amministrativi di BranchCache nel controller di dominio che eseguono sistemi operativi precedenti, o è possibile creare oggetti Criteri di gruppo in modalità remota in altri computer che eseguono Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 o Windows Server 2012.

-   **Siti di Active Directory** vengono utilizzati per limitare l'ambito del server cache ospitata che vengono individuati automaticamente.  Per rilevare automaticamente un server cache ospitata, i computer client e server devono appartenere allo stesso sito. BranchCache è progettato per avere un impatto minimo sul client e server e non impone requisiti hardware aggiuntivi oltre a quelli necessari per eseguire i rispettivi sistemi operativi.  

**Documentazione e cronologia di BranchCache**

BranchCache è stato introdotto in Windows 7&reg; e Windows Server&reg; 2008 R2 ed è stato migliorato in Windows Server 2012, Windows 8 e versioni successive.

> [!NOTE]
> Se si distribuisce BranchCache in sistemi operativi diversi da Windows Server 2016, sono disponibili le seguenti risorse di documentazione.
> 
> - Per informazioni su BranchCache in Windows 8, Windows 8.1, Windows Server 2012 e Windows Server 2012 R2, vedere [Panoramica di BranchCache](https://technet.microsoft.com/library/hh831696.aspx).  
> - Per informazioni su BranchCache in Windows 7 e Windows Server 2008 R2, vedere  [BranchCache per Windows Server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx).  
  


