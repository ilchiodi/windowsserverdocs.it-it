---
title: Distribuire la modalità Cache ospitata BranchCache
description: Questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata in computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 326a1f1edfe6cb763a33ebfc8fd5abdd5b6aab3a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Distribuire la modalità Cache ospitata BranchCache

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Guida alla rete Windows Server 2016 Core vengono fornite istruzioni per la pianificazione e distribuzione dei principali componenti necessari per una rete completamente funzionante e un nuovo Active Directory&reg; dominio in una nuova foresta.

Questa guida viene illustrato come compilare nella rete principale, fornendo istruzioni per la distribuzione di BranchCache in modalità cache ospitata in uno o più succursali con un Controller di dominio Read\-Only in cui i computer client sono in esecuzione Windows&reg; 10, Windows 8.1 o Windows 8 e vengono aggiunti al dominio.

>[!IMPORTANT]
>Non utilizzare questa Guida se si prevede di distribuire o hanno già distribuito un server cache ospitata BranchCache che esegue Windows Server 2008 R2. Questa guida fornisce istruzioni per la distribuzione in modalità cache ospitata con un server cache ospitata che esegue Windows Server&reg; 2016, Windows Server 2012 R2 o Windows Server 2012.

Questa guida contiene le sezioni seguenti.

- [Prerequisiti per l'utilizzo di questa Guida](#bkmk_pre)

- [Informazioni sulla Guida](#bkmk_about)

- [Cosa non fornisce questa Guida](#bkmk_not)

- [Panoramiche delle tecnologie](#bkmk_tech)

- [Panoramica della distribuzione in modalità Cache ospitata di BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Modalità Cache ospitata BranchCache pianificazione della distribuzione](3-Bc-Hcm-Plan.md)

- [Distribuzione in modalità Cache ospitata di BranchCache](4-Bc-Hcm-Deployment.md)

- [Risorse aggiuntive](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Prerequisiti per l'utilizzo di questa Guida

Si tratta di una guida complementare alla Guida alla rete Windows Server 2016 Core. Per distribuire BranchCache in modalità cache ospitata con questa Guida, è innanzitutto necessario eseguire le operazioni seguenti.

- Distribuire una rete core nella sede principale usando la Guida alla rete Core o già sono le tecnologie fornite nella Guida alla rete Core installato e funzioni correttamente nella rete. Queste tecnologie includono TCP/IP v4, DHCP, servizi di dominio Active Directory \(AD DS\) e DNS.

    > [!NOTE]
    > Windows Server 2016 [Guida alla rete Core](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) è disponibile nella libreria tecnica di Windows Server 2016.  

- Distribuire server di contenuti BranchCache che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 nella sede principale o in un data center cloud. Per informazioni su come distribuire server di contenuti BranchCache, vedere [risorse aggiuntive](11-Bc-Hcm-additional-resources.md).

- Stabilire \(WAN\) connessioni di rete tra la succursale, la sede principale e, se appropriato, le risorse Cloud tramite private virtuale rete \(VPN\), DirectAccess o un altro metodo di connessione.

- Distribuire i computer client nella filiale che eseguono uno dei seguenti sistemi operativi, che forniscono BranchCache con supporto per servizio trasferimento intelligente in Background (BITS), Hyper Text HTTP (Transfer Protocol) e Server Message Block (SMB).
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 Enterprise

>[!NOTE]
>Nei seguenti sistemi operativi, BranchCache non supporta la funzionalità HTTP e SMB, ma supporta la funzionalità BranchCache BITS.
>     - Windows 10 Pro, BITS supporta solo
>     - Windows 8.1 Pro, BITS supporta solo
>     - Windows 8 Pro, BITS supporta solo

## <a name="bkmk_about"></a>Informazioni sulla Guida

Questa guida è progettata per gli amministratori di rete e di sistema che hanno seguito le istruzioni nella Guida alla rete Core di Windows Server 2016 o Guida alla rete Core di Windows Server 2012 per distribuire una rete core, o per coloro che hanno distribuito in precedenza le tecnologie incluse nella Guida alla rete Core, tra cui \(AD DS\) servizi di dominio Active Directory, Domain Name Service \(DNS\), Dynamic Host Configuration Protocol \(DHCP\) e TCP/IP v4.

È consigliabile consultare le guide alla progettazione e distribuzione per ognuna delle tecnologie utilizzate in questo scenario di distribuzione. Queste guide consentono di determinare se questo scenario di distribuzione fornisce i servizi e le impostazioni di configurazione per la rete dell'organizzazione.

## <a name="bkmk_not"></a>Cosa non fornisce questa Guida

Questa Guida non fornisce informazioni concettuali su BranchCache, incluse le informazioni sulle modalità di BranchCache.  

Questa Guida non fornisce informazioni su come distribuire connessioni WAN o altre tecnologie nella succursale, ad esempio DHCP, un controller di dominio o un server VPN.

Inoltre, questa Guida non fornisce indicazioni sull'hardware che è necessario utilizzare quando si distribuisce un server cache ospitata. È possibile eseguire altre applicazioni e servizi nel server cache ospitata, tuttavia è necessario eseguire tale operazione, in base a carico di lavoro, funzionalità hardware e dimensioni delle succursali, se si desidera installare server cache ospitata di BranchCache in un computer specifico e quanto spazio su disco da allocare per la cache.  
Questa guida vengono fornite istruzioni per configurare i computer che eseguono Windows 7. Se si dispone di computer client che eseguono Windows 7 nelle succursali, è necessario configurarli con procedure diverse da quelle fornite in questa guida per i computer client che eseguono Windows 10, Windows 8.1 e Windows 8.
  
Inoltre, se si dispone di computer che eseguono Windows 7, è necessario configurare il server cache ospitata con un certificato server rilasciato da un'autorità di certificazione attendibili i computer client. \ (Se tutti i computer client sono in esecuzione Windows 10, Windows 8.1 o Windows 8, non necessaria configurare il server cache ospitata con un certificato server. \) 
> [!IMPORTANT]
> Se i server cache ospitata esegue Windows Server 2008 R2, utilizzare Windows Server 2008 R2 [Guida alla distribuzione di BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) invece di questa Guida alla distribuzione di BranchCache in modalità cache ospitata. Applicare le impostazioni di criteri di gruppo che sono descritti nella Guida a tutti i client BranchCache che eseguono versioni di Windows da Windows 7 a Windows 10. I computer che eseguono Windows Server 2008 R2 non possono essere configurati tramite i passaggi in questa Guida.

## <a name="bkmk_tech"></a>Panoramiche delle tecnologie

Questa guida complementare BranchCache è la sola tecnologia che è necessario installare e configurare. È necessario eseguire i comandi BranchCache di Windows PowerShell nel server di contenuti, ad esempio Web e file server, tuttavia non è necessario modificare o riconfigurare il server di contenuti in altro modo. Inoltre, è necessario configurare i computer client utilizzando criteri di gruppo nei controller di dominio che eseguono servizi di dominio Active Directory in Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

### <a name="branchcache"></a>BranchCache

BranchCache è una tecnologia di ottimizzazione (WAN) della larghezza di banda di rete WAN inclusa in alcune edizioni dei sistemi operativi Windows Server 2016 e Windows 10, nonché in alcune edizioni di Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7.

Per ottimizzare la larghezza di banda WAN quando gli utenti accedono al contenuto nei server remoti, BranchCache Scarica i contenuti di richiesta client dalla sede principale o server contenuto cloud ospitati e memorizza nella cache il contenuto in succursali, consentendo di altri computer client nelle filiali di accedere al contenuto stesso localmente anziché tramite la rete WAN.

Quando si distribuisce BranchCache in modalità cache ospitata, è necessario configurare i computer client nella succursale come client in modalità cache ospitata e quindi è necessario distribuire un server cache ospitata nella succursale. Questa guida viene illustrato come distribuire il server cache ospitata con prehashed e precaricato contenuto dal Web e file server di contenuti basati su server.

### <a name="group-policy"></a>Criteri di gruppo

Criteri di gruppo in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 sono un'infrastruttura utilizzata per distribuire e applicare uno o più configurazioni desiderate o le impostazioni di criteri a un set di accesso degli utenti e computer all'interno di un ambiente Active Directory. 

Questa infrastruttura è costituita da un motore criteri di gruppo e da più estensioni lato connessione tra client \(CSEs\) che sono responsabili della lettura delle impostazioni dei criteri nei computer client di destinazione.

Criteri di gruppo viene utilizzato in questo scenario per configurare computer client membri del dominio con la modalità cache ospitata di BranchCache.

Per continuare con questa Guida, vedere [ospitato Cache modalità di distribuzione Panoramica di BranchCache](2-Bc-Hcm-Deploy-Overview.md).
