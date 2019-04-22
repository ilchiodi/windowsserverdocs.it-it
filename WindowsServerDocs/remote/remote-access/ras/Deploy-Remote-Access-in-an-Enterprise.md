---
title: Distribuire Accesso remoto in una grande impresa
description: In questo argomento viene fornita un'introduzione allo scenario DirectAccess in Windows Server 2016 per le aziende.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f110ad945139da3b07b33bbb0adb3e8084743fdb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812242"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>Distribuire Accesso remoto in una grande impresa

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento fornisce un'introduzione allo scenario DirectAccess per l'azienda.  
  
  
> [!IMPORTANT]  
> Per distribuire DirectAccess usando questa Guida, è necessario usare un server DirectAccess che esegue Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Prima di iniziare la distribuzione vedere l'elenco di configurazioni non supportate, problemi noti e prerequisiti  
  
-   [DirectAccess configurazioni non supportate](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-unsupported-configurations)  
  
-   [Problemi noti di DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-known-issues)  
  
-   [Prerequisiti di prerequisiti per la distribuzione DirectAccess)](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/prerequisites-for-deploying-directaccess)  
  
## <a name="BKMK_OVER"></a>Descrizione dello scenario  
L'accesso remoto include diverse funzionalità aziendali, tra cui la distribuzione di più server di accesso remoto in un cluster con carico bilanciato tramite Bilanciamento carico di rete di Windows o un servizio di bilanciamento del carico esterno, la configurazione di una distribuzione multisito con i server di accesso remoto che si trovano in aree geografiche diverse e la distribuzione di DirectAccess mediante l'autenticazione client a due fattori con One Time Password (OTP).  
  
## <a name="in-this-scenario"></a>In questo scenario  
Ogni scenario aziendale viene descritto in un documento che include istruzioni di pianificazione e distribuzione. Per altre informazioni, vedi:  
  
-   [Distribuire accesso remoto in un cluster](cluster/Deploy-Remote-Access-In-Cluster.md)  
  
-   [Distribuire più server di accesso remoto in una distribuzione multisito](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)  
  
-   [Distribuire accesso remoto con autenticazione OTP](otp/Deploy-RA-OTP.md)  
  
-   [Distribuire accesso remoto in un ambiente con più foreste](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)  
  
## <a name="BKMK_APP"></a>Applicazioni pratiche  
Gli scenari aziendali di Accesso remoto forniscono:  
  
-   **Maggiore disponibilità**. Distribuzione di più server di accesso remoto in un cluster consente la scalabilità e aumenta la capacità di velocità effettiva e numero di utenti. Il bilanciamento del carico del cluster fornisce la disponibilità elevata. Se un server nel cluster non riesce, gli utenti remoti possono continuare ad accedere alla rete aziendale interna usando un altro server nel cluster. Il failover è trasparente perché i client si connettono al cluster usando un indirizzo IP virtuale (VIP).  
  
-   **Facilità di gestione**. Un cluster o una distribuzione multisito può essere configurata e gestita come una singola entità tramite la console Gestione accesso remoto in esecuzione in uno dei server cluster. Inoltre, una distribuzione multisito consente agli amministratori di allineare la distribuzione di Accesso remoto ai siti di Active Directory, fornendo un'architettura semplificata. Le impostazioni condivise possono essere impostate facilmente in tutti i server cluster o in tutti i server dei punti di ingresso multisito. Le impostazioni di Accesso remoto possono essere gestite da qualsiasi server nel cluster o nella distribuzione oppure in remoto mediante Strumenti di amministrazione remota del server. Inoltre, l'intero cluster o distribuzione multisito può essere monitorato da una sola Console di gestione Accesso remoto.  
  
-   **Efficienza dei costi**. Una distribuzione multisito di accesso remoto consente alle aziende di distribuire i server di accesso remoto in più siti corrispondenti alle posizioni del client. Ciò fornisce un'esperienza di accesso prevedibile per i client remoti indipendente dalla posizione e riduce costi e larghezza di banda grazie al routing del traffico client su Internet al server di accesso remoto più vicino.  
  
-   **Sicurezza**. Distribuzione di autenticazione client avanzata con una password monouso (OTP) invece della password Active Directory standard garantisce una maggiore sicurezza.  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
La tabella seguente elenca i ruoli e le funzionalità usati nello scenario aziendale.  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|---------|-----------------|  
|Ruolo del server Accesso remoto|Il ruolo viene installato e disinstallato tramite la console di Server Manager. Questo ruolo comprende DirectAccess, una funzionalità precedentemente inclusa in Windows Server 2008 R2, e il servizio Routing e Accesso remoto che in precedenza era un servizio ruolo nel ruolo server Servizi di accesso e criteri di rete (NPAS). Il ruolo Accesso remoto è costituito da due componenti:<br /><br />1.  DirectAccess e Routing e accesso remoto (RRAS) VPN DirectAccess e VPN vengono gestiti insieme nella console di gestione accesso remoto.<br />2.  Funzionalità di routing di Routing RRAS routing vengono gestite nella console di Routing e accesso remoto legacy.<br /><br />Il ruolo server Accesso remoto dipende dalle funzionalità server seguenti:<br /><br />-Internet Information Services (IIS) - questa funzionalità è necessaria per configurare il probe web predefinito e server del percorso di rete.<br />-Funzionalità Console Gestione Criteri gruppo - funzionalità richiesta da DirectAccess per creare e gestire gli oggetti Criteri di gruppo (GPO) in Active Directory e deve essere installato come funzionalità obbligatoria per il ruolo del server.|  
|Funzionalità Strumenti di Gestione Accesso remoto|Questa funzionalità viene installata come segue:<br /><br />-Viene installato per impostazione predefinita in un server di accesso remoto quando è installato il ruolo Accesso remoto e supporta l'interfaccia utente della console di gestione remota.<br />-Può essere installata facoltativamente in un server non è in esecuzione il ruolo di server di accesso remoto. In questo caso viene utilizzata per la gestione remota di un computer di Accesso remoto che esegue DirectAccess e VPN.<br /><br />La funzionalità Strumenti di Gestione Accesso remoto è costituita dai seguenti elementi:<br /><br />1.  Interfaccia utente grafica di Accesso remoto e strumenti da riga di comando<br />2.  Modulo Accesso remoto per Windows PowerShell<br /><br />Le dipendenze includono:<br /><br />1.  Console Gestione Criteri di gruppo<br />2.  Connection Manager Administration Kit (CMAK) RAS<br />3.  Windows PowerShell 3.0<br />4.  Strumenti di gestione e infrastruttura grafica|  
|Windows NLB|Questa funzionalità consente il bilanciamento del carico di più server di accesso remoto.|  
  

  


