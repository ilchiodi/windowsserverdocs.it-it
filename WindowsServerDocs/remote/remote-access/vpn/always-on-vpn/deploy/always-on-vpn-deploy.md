---
title: Distribuzione VPN Always On per Windows Server e Windows 10
description: È possibile utilizzare questa distribuzione per distribuire connessioni sempre nella rete privata virtuale (VPN) per i dipendenti remoti tramite l'accesso remoto in Windows Server 2016 o versione successiva e i profili VPN Always On per i computer client Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: c435f91ef6f3ff5ffd2661291264b939d5ce4e2a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/21/2018
ms.locfileid: "8981131"
---
# Distribuzione di VPN Always On per Windows Server e Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** accesso remoto](../../../Remote-Access.md)<br>
& #187; [ **Successivo:** informazioni sulle funzionalità VPN Always On e la funzionalità](../../vpn-map-da.md)


VPN Always On offre una soluzione singola, più coerente per accesso remoto e supporta appartenenti al dominio, appartenenti a non di dominio (gruppo di lavoro) o AD – dispositivi aggiunti ad Azure, anche dell'utente i dispositivi.  Con VPN Always On, il tipo di connessione non deve essere esclusivamente utente o del dispositivo, ma può essere una combinazione di entrambi. Ad esempio, si potrebbe abilitare l'autenticazione dei dispositivi per la gestione dei dispositivi remoti e quindi abilitare l'autenticazione utente per la connettività a siti interni aziendali e servizi.



## Prerequisiti

Probabilmente hai le tecnologie distribuite che è possibile utilizzare per distribuire VPN Always On. Diverso dal server controller di dominio/DNS, la distribuzione di VPN Always On richiede un server dei criteri di rete (raggio), un server di autorità di certificazione (CA) e un server di accesso remoto (Routing/VPN). Dopo aver configurata l'infrastruttura, è necessario registrare i client e quindi connettere il client a locale in modo sicuro tramite diverse modifiche di rete.

- Active Directory domain infrastruttura, tra cui uno o più server Domain Name System (DNS). Interni ed esterni aree Domain Name System (DNS) sono obbligatori, che presuppone che l'area interno è un sottodominio delegato della zona esterno (ad esempio, corp.contoso.com e contoso.com).
- Active Directory in base alle infrastruttura a chiave pubblica (PKI) e servizi certificati Active Directory (AD CS).
- Server virtuale o fisica, nuovi o esistenti, per installare il Server di criteri di rete (NPS). Se hai già server dei criteri di rete nella rete, è possibile modificare una configurazione del server dei criteri di rete esistente anziché aggiungere un nuovo server.
- Accesso remoto come server VPN di Gateway RAS con un piccolo sottoinsieme delle funzionalità che supportano le connessioni VPN IKEv2 e routing LAN.
- Rete perimetrale che include due firewall.  Assicurarsi che i firewall consentono il traffico che è necessario per le comunicazioni con VPN sia raggio funzionare correttamente. Per altre informazioni, vedi [Sempre su VPN Panoramica della tecnologia](../always-on-vpn-technology-overview.md).
- Server fisico o macchina virtuale (VM) nella rete perimetrale con due schede di rete Ethernet fisiche per installare l'accesso remoto come server VPN di Gateway RAS. Le macchine virtuali richiedono LAN virtuali (VLAN) per l'host. 
- Appartenenza al gruppo Administrators o equivalente, è il minimo richiesto.
- Leggere la sezione pianificazione di questa guida per garantire che si è pronti per questa distribuzione prima di eseguire la distribuzione.
- Esamina le guide alla distribuzione e progettazione per ognuna delle tecnologie utilizzate. Queste guide possono aiutarti a determinare se gli scenari di distribuzione di fornire i servizi e configurazione che è necessario per la rete dell'organizzazione. Per altre informazioni, vedi [Sempre su VPN Panoramica della tecnologia](../always-on-vpn-technology-overview.md).
- Piattaforma di gestione di tua scelta per la distribuzione della configurazione VPN Always On perché il CSP non è specifico del fornitore.


>[!IMPORTANT]
>Per la distribuzione, non è un requisito che i server di infrastruttura, ad esempio i computer che esegue Active Directory Domain Services, servizi certificati Active Directory e Server dei criteri di rete, sono in esecuzione Windows Server 2016. È possibile utilizzare le versioni precedenti di Windows Server, ad esempio Windows Server 2012 R2, per i server di infrastruttura e il server che esegue l'accesso remoto.
>
>Non tentare di distribuire l'accesso remoto in una macchina virtuale \(VM\) in Microsoft Azure. Utilizza l'accesso remoto in Microsoft Azure non è supportato, inclusi l'accesso remoto VPN e DirectAccess. Per altre informazioni, vedi [il software server Microsoft il supporto per le macchine virtuali di Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="bkmk_about"></a>Su questa distribuzione

Le istruzioni fornite illustrano in dettaglio la distribuzione di accesso remoto come un singolo tenant Gateway RAS VPN per le connessioni VPN point\-to\-sito, usando uno degli scenari indicati di seguito, per i computer client remoto che eseguono Windows 10. Puoi anche trovare istruzioni per la modifica di alcune dell'infrastruttura esistente per la distribuzione. Anche in tutta la distribuzione, puoi trovare i collegamenti per altre informazioni sul processo di connessione VPN, i server da configurare, nodo ProfileXML VPNv2 CSP e altre tecnologie di distribuzione di VPN Always On.

**Scenari di distribuzione di VPN Always On:**

1. Distribuire sempre su VPN solo.
2. Distribuire VPN Always On con accesso condizionale per la connettività VPN con Azure AD.


Per ulteriori informazioni e del flusso di lavoro degli scenari presentati, vedi [Distribuire VPN Always On](always-on-vpn-deploy-deployment.md).


## <a name="bkmk_not"></a>Cosa non viene fornita in questa distribuzione

Questa distribuzione non fornisce le istruzioni per:

- \(AD DS\) di servizi di dominio Active Directory.
- Servizi certificati Active Directory \(AD CS\) e un'infrastruttura a chiave pubblica \(PKI\).
- Dynamic Host Configuration Protocol \(DHCP\). 
- Rete hardware, ad esempio cablaggi Ethernet, firewall, commutatori e hub.
- Altre risorse di rete, ad esempio applicazioni e file server, che gli utenti remoti possono accedere tramite una connessione VPN Always On.
- Connettività Internet o l'accesso condizionale per la connettività Internet con Azure AD. Per ulteriori informazioni, vedi [condizionale accedere in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).




## Passaggi successivi

- [Altre informazioni sulle funzionalità VPN Always On e le funzionalità](../../vpn-map-da.md)

- [Altre informazioni sui miglioramenti apportati VPN Always On](../always-on-vpn-enhancements.md)

- [Scopri alcune delle funzionalità VPN Always On avanzate](always-on-vpn-adv-options.md)

- [Altre informazioni sulla tecnologia di VPN Always On](../always-on-vpn-technology-overview.md)

- [Iniziare a pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-deployment.md)


---
