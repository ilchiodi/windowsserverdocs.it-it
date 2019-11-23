---
title: Distribuire la modalità Cache ospitata BranchCache
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49e74132dba2909b7e5b639c95ef50064cf23e8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356374"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Distribuire la modalità Cache ospitata BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La Guida alla rete di Windows Server 2016 Core vengono fornite istruzioni per la pianificazione e distribuzione dei componenti di base necessari per una rete completamente funzionante e un nuovo Active Directory&reg; dominio in una nuova foresta.

In questa guida viene illustrato come compilare nella rete principale, fornendo istruzioni per la distribuzione di BranchCache in modalità cache ospitata in uno o più succursali con una lettura\-solo i Controller di dominio in cui i computer client sono in esecuzione Windows&reg; 10, Windows 8.1 o Windows 8 e vengono aggiunti al dominio.

>[!IMPORTANT]
>Non utilizzare questa Guida se si prevede di distribuire o hanno già distribuito un server cache ospitata BranchCache che esegue Windows Server 2008 R2. Questa guida vengono fornite istruzioni per la distribuzione in modalità cache ospitata con un server cache ospitata che esegue Windows Server&reg; 2016, Windows Server 2012 R2 o Windows Server 2012.

Questa guida contiene le sezioni seguenti.

- [Prerequisiti per l'uso di questa guida](#bkmk_pre)

- [Informazioni sulla guida](#bkmk_about)

- [Informazioni non contenute in questa guida](#bkmk_not)

- [Panoramica sulla tecnologia](#bkmk_tech)

- [Panoramica della distribuzione in modalità cache ospitata di BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Pianificazione della distribuzione in modalità cache ospitata di BranchCache](3-Bc-Hcm-Plan.md)

- [Distribuzione in modalità cache ospitata di BranchCache](4-Bc-Hcm-Deployment.md)

- [Risorse aggiuntive](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Prerequisiti per l'uso di questa guida

Si tratta di una guida complementare alla Guida di Windows Server 2016 Core Network. Per distribuire BranchCache in modalità cache ospitata con questa Guida, è prima di tutto necessario eseguire questa procedura.

- Distribuire una rete core nella sede principale usando la Guida alla rete core o avere già installate e funzionanti nella rete le tecnologie fornite nella Guida alla rete core. Queste tecnologie includono TCP\/IP v4, DHCP, servizi di dominio Active Directory \(AD DS\), e DNS.

    > [!NOTE]
    > Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) è disponibile nella libreria tecnica di Windows Server 2016.  

- Distribuire server di contenuti BranchCache che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 nel proprio ufficio principale o in un data center cloud. Per informazioni su come distribuire server di contenuti BranchCache, vedere [risorse aggiuntive](11-Bc-Hcm-additional-resources.md).

- Stabilire la rete WAN \(WAN\) le connessioni tra le succursale, la sede principale e, se appropriato, le risorse Cloud, tramite una rete privata virtuale \(VPN\), DirectAccess, o un altro metodo di connessione.

- Distribuire nei computer client nella filiale che eseguono uno dei seguenti sistemi operativi, che forniscono BranchCache con supporto per servizio trasferimento intelligente in Background (BITS), Hyper Text Transfer Protocol (HTTP) e Server Message Block (SMB).
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 Enterprise

> [!NOTE]
> Nei seguenti sistemi operativi, BranchCache non supporta la funzionalità HTTP e SMB, ma supporta la funzionalità BranchCache BITS.
>     - Windows 10 Pro, solo supporto BITS
>     - Windows 8.1 Pro, solo supporto BITS
>     - Windows 8 Pro, solo supporto BITS

## <a name="bkmk_about"></a>Informazioni su questa guida

Questa guida è destinata agli amministratori di rete e di sistema che hanno seguito le istruzioni riportate nella Guida alla rete core di Windows Server 2016 o nella Guida alla rete core di Windows Server 2012 per distribuire una rete core o per coloro che hanno distribuito in precedenza le tecnologie incluse nella Guida alla rete core, tra cui Active Directory Domain Services \(AD DS\), Domain Name Service \(DNS\), Dynamic Host Configuration Protocol \(DHCP e TCP\)\/

È consigliabile vedere le guide alla progettazione e alla distribuzione per ognuna delle tecnologie usate in questo scenario di distribuzione. Queste guide consentono di determinare se questo scenario di distribuzione fornisce la configurazione e i servizi necessari per la rete dell'organizzazione.

## <a name="bkmk_not"></a>Descrizione non fornita da questa guida

Questa Guida non fornisce informazioni concettuali su BranchCache, incluse le informazioni sulle modalità e le funzionalità di BranchCache.  

In questa Guida non fornisce informazioni sulla distribuzione di connessioni WAN o altre tecnologie nella succursale, ad esempio un server DHCP, controller di dominio di sola lettura o VPN.

Questa Guida non fornisce inoltre indicazioni sull'hardware che è necessario usare quando si distribuisce un server cache ospitata. È possibile eseguire altre applicazioni e servizi nel server cache ospitata, ma è necessario determinare, in base a carico di lavoro, funzionalità hardware e dimensioni delle succursali, se si vuole installare il server cache ospitata BranchCache in un computer specifico e quanto spazio su disco allocare per la cache.  
In questa guida vengono fornite istruzioni per configurare i computer che eseguono Windows 7. Se si dispone di computer client che eseguono Windows 7 nelle succursali, è necessario configurarli tramite le procedure che sono diverse da quelli forniti in questa guida per i computer client che eseguono Windows 10, Windows 8.1 e Windows 8.
  
Inoltre, se si dispone di computer che eseguono Windows 7, è necessario configurare il server cache ospitata con un certificato server emesso da un'autorità di certificazione attendibili i computer client. \(se tutti i computer client eseguono Windows 10, Windows 8.1 o Windows 8, non è necessario configurare il server cache ospitata con un certificato del server.\) 
> [!IMPORTANT]
> Se i server cache ospitata esegue Windows Server 2008 R2, utilizzare Windows Server 2008 R2 [Guida alla distribuzione di BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) invece di questa Guida alla distribuzione di BranchCache in modalità cache ospitata. Applicare le impostazioni di criteri di gruppo che sono descritti nella Guida a tutti i client di BranchCache che eseguono versioni di Windows da Windows 7 per Windows 10. Attenersi alla procedura riportata in questa Guida non è possibile configurare i computer che eseguono Windows Server 2008 R2.

## <a name="bkmk_tech"></a>Panoramica sulla tecnologia

In questa guida complementare BranchCache è la sola tecnologia che è necessario installare e configurare. Si devono eseguire i comandi BranchCache di Windows PowerShell nei server di contenuti, ad esempio server Web e file server, ma non è necessario modificare o riconfigurare i server di contenuti in altro modo. Inoltre, è necessario configurare i computer client utilizzando criteri di gruppo nei controller di dominio che eseguono servizi di dominio Active Directory in Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

### <a name="branchcache"></a>BranchCache

BranchCache è una tecnologia di ottimizzazione (WAN) della larghezza di banda di rete WAN inclusa in alcune edizioni dei sistemi operativi Windows 10 e Windows Server 2016, nonché in alcune edizioni di Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7.

Per ottimizzare la larghezza di banda WAN quando gli utenti di accedere ai contenuti nei server remoti, BranchCache Scarica i contenuti di richiesta del client dalla sede principale o server contenuto cloud ospitati e memorizza nella cache il contenuto degli uffici di filiale, consentendo di altri computer client nelle filiali di accedere al contenuto stesso localmente anziché tramite la rete WAN.

Quando si distribuisce BranchCache in modalità cache ospitata, è necessario configurare i computer client nella succursale come client in modalità cache ospitata e quindi distribuire un server cache ospitata nella succursale. In questa guida viene illustrato come distribuire il server cache ospitata con prehashed e precaricato contenuto dal server Web e file\-basato su server di contenuti.

### <a name="group-policy"></a>Criteri di gruppo

Criteri di gruppo in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 sono un'infrastruttura utilizzata per distribuire e applicare uno o più configurazioni desiderate o le impostazioni di criteri a un insieme di computer all'interno di un ambiente Active Directory e gli utenti di destinazione. 

Questa infrastruttura è costituita da più client e un motore di criteri di gruppo\-estensioni lato \(CSE\) che sono responsabili per la lettura delle impostazioni dei criteri nei computer client di destinazione.

Criteri di gruppo viene usato in questo scenario per configurare computer client membri del dominio con la modalità cache ospitata di BranchCache.

Per continuare con questa Guida, vedere [ospitato Cache modalità di distribuzione Panoramica di BranchCache](2-Bc-Hcm-Deploy-Overview.md).
