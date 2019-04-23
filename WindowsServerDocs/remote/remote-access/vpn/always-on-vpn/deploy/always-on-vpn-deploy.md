---
title: Distribuzione VPN Always On per Windows Server e Windows 10
description: È possibile usare questa distribuzione per distribuire le connessioni sempre nella rete privata virtuale (VPN) per i dipendenti remoti usando i profili VPN Always On e accesso remoto in Windows Server 2016 o versioni successive per i computer client Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb60bdc6d6f3ff074f04827aa95c9e8e8abf35b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859622"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Distribuzione VPN Always On per Windows Server e Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Precedente:** Accesso remoto](../../../Remote-Access.md)<br>
&#187; [**Next:** Scopri le funzionalità VPN Always On](../../vpn-map-da.md)


VPN Always On fornisce un'unica soluzione coerente per l'accesso remoto e supporta appartenenti a un dominio, aggiunto non di dominio (gruppo di lavoro) o Active Directory-dispositivi aggiunti ad Azure, anche i dispositivi personali.  Con VPN Always On, il tipo di connessione non deve essere esclusivamente utente o dispositivo, ma può essere una combinazione di entrambi. Ad esempio, è possibile abilitare l'autenticazione del dispositivo per la gestione dei dispositivi remoti e quindi abilitare l'autenticazione utente per la connettività a servizi e siti aziendali interne.



## <a name="prerequisites"></a>Prerequisiti

È probabile che si hanno le tecnologie distribuite che è possibile usare per la distribuzione VPN Always On. Diverso da server controller di dominio/DNS, la distribuzione VPN Always On richiede un server dei criteri di rete (RADIUS), un server di autorità di certificazione (CA) e un server di accesso remoto (servizio Routing/VPN). Dopo aver configurato l'infrastruttura, è necessario registrare i client e quindi connettere i client per on-premises in modo sicuro tramite diverse modifiche di rete.

- Active Directory domain infrastruttura, inclusi uno o più server di sistema DNS (Domain Name). Le zone di sistema DNS (Domain Name) interne ed esterne sono necessari, che presuppone che l'area interna è un sottodominio delegato della zona esterno (ad esempio, corp.contoso.com e contoso.com).
- Active Directory-based infrastruttura a chiave pubblica (PKI) e servizi certificati Active Directory (AD CS).
- Server, virtuale o fisico, esistente o nuovo, per installare Server dei criteri di rete (NPS). Se si dispone già di server dei criteri di rete nella rete, è possibile modificare una configurazione di server dei criteri di rete esistente anziché aggiungere un nuovo server.
- Accesso remoto come server VPN Gateway RAS con un piccolo subset di funzionalità che supportano le connessioni VPN IKEv2 e routing LAN.
- Rete perimetrale che include due firewall.  Assicurarsi che i firewall consentano il traffico che è necessario per le comunicazioni di VPN sia RADIUS funzionare correttamente. Per altre informazioni, vedere [sempre su VPN Cenni preliminari sulla tecnologia](../always-on-vpn-technology-overview.md).
- Server fisico o macchina virtuale (VM) nella rete perimetrale con due schede di rete Ethernet fisiche per installare accesso remoto come server VPN Gateway RAS. Le macchine virtuali richiedono LAN virtuale (VLAN) per l'host. 
- Appartenenza al gruppo Administrators o equivalente è il requisito minimo necessario.
- Leggere la sezione pianificazione di questa guida per assicurarsi che siano preparati per la distribuzione prima di eseguire la distribuzione.
- Esaminare le guide di progettazione e distribuzione per ognuna delle tecnologie usate. Queste guide consentono di determinare se gli scenari di distribuzione forniscono i servizi e configurazione necessarie per la rete dell'organizzazione. Per altre informazioni, vedere [sempre su VPN Cenni preliminari sulla tecnologia](../always-on-vpn-technology-overview.md).
- Piattaforma di gestione preferito per distribuire la configurazione VPN Always On in quanto il CSP non è specifico del fornitore.


>[!IMPORTANT]
>Per questa distribuzione, non è un requisito che i server di infrastruttura, ad esempio i computer che eseguono servizi di dominio Active Directory, servizi certificati Active Directory e Server dei criteri di rete, siano in esecuzione Windows Server 2016. È possibile usare le versioni precedenti di Windows Server, ad esempio Windows Server 2012 R2, per il server di infrastruttura e per il server che esegue l'accesso remoto.
>
>Non tentare di distribuire accesso remoto in una macchina virtuale \(VM\) in Microsoft Azure. Uso di accesso remoto in Microsoft Azure non è supportato, inclusi l'accesso remoto VPN e DirectAccess. Per altre informazioni, vedere [supporto di software server Microsoft per le macchine virtuali di Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).


## <a name="bkmk_about"></a>Su questa distribuzione

Le istruzioni fornite consentono di eseguire la distribuzione di accesso remoto come un singolo tenant VPN RAS Gateway per il punto\-a\-le connessioni VPN, usando uno degli scenari indicati di seguito, per i computer client remoti che eseguono Windows del sito 10. È inoltre possibile trovare istruzioni per la modifica di alcune dell'infrastruttura esistente per la distribuzione. Anche in questa distribuzione, è trovare i collegamenti che consentono di approfondire il processo di connessione VPN, server da configurare, nodo ProfileXML VPNv2 CSP e altre tecnologie per la distribuzione VPN Always On.

**Scenari di distribuzione VPN Always On:**

1. Distribuire sempre in una VPN solo.
2. Distribuire VPN Always On con accesso condizionale per la connettività VPN con Azure AD.


Per altre informazioni e degli scenari presentati del flusso di lavoro, vedere [distribuire VPN Always On](always-on-vpn-deploy-deployment.md).


## <a name="bkmk_not"></a>Che cosa non viene fornita in questa distribuzione

Questa distribuzione non vengono fornite istruzioni per:

- Servizi di dominio Active Directory \(Active Directory Domain Services\).
- Servizi certificati Active Directory \(Servizi certificati Active Directory\) e un'infrastruttura a chiave pubblica \(PKI\).
- Dynamic Host Configuration Protocol \(DHCP\). 
- Hardware, ad esempio cavi Ethernet, i firewall, commutatori e hub di rete.
- Risorse di rete aggiuntive, ad esempio applicazioni e file server, che gli utenti remoti possono accedere tramite una connessione VPN Always On.
- Connettività Internet o l'accesso condizionale per la connettività Internet usando Azure AD. Per informazioni dettagliate, vedere [accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).




## <a name="next-steps"></a>Passaggi successivi

- [Scopri le funzionalità VPN Always On](../../vpn-map-da.md)

- [Altre informazioni sui miglioramenti VPN Always On](../always-on-vpn-enhancements.md)

- [Informazioni su alcune delle funzionalità avanzate VPN Always On](always-on-vpn-adv-options.md)

- [Altre informazioni relative alle tecnologie VPN Always On](../always-on-vpn-technology-overview.md)

- [Iniziare a pianificare la distribuzione VPN Always On](always-on-vpn-deploy-deployment.md)


---
