---
title: Distribuzione VPN Always On per Windows Server e Windows 10
description: È possibile usare questa distribuzione per distribuire Always On connessioni VPN (Virtual Private Network) per i dipendenti remoti usando accesso remoto in Windows Server 2016 o versioni successive e Always On i profili VPN per i computer client Windows 10.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 5ae1a40b-4f10-4ace-8aaf-13f7ab581f4f
ms.localizationpriority: medium
ms.date: 12/20/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: e107ba0d36a1b59d4bc1bb365fb98aa9285677ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860124"
---
# <a name="always-on-vpn-deployment-for-windows-server-and-windows-10"></a>Always On la distribuzione VPN per Windows Server e Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Accesso remoto](../../../Remote-Access.md)<br>
- [Passaggio **successivo:** Informazioni sulle funzionalità e sulle funzionalità di Always On VPN](../../vpn-map-da.md)

Always On VPN fornisce un'unica soluzione coerente per l'accesso remoto e supporta dispositivi aggiunti a un dominio, non aggiunti a un dominio (gruppo di lavoro) o aggiunti a Azure AD, anche dispositivi di proprietà personale. Con VPN Always On, la connessione non deve essere esclusivamente di tipo utente o dispositivo, ma può essere una combinazione di entrambi. È ad esempio possibile abilitare l'autenticazione dei dispositivi per la gestione dei dispositivi remoti e quindi abilitare l'autenticazione utente per la connettività a servizi e siti aziendali interni.

## <a name="prerequisites"></a>Prerequisiti

È probabile che siano state distribuite le tecnologie che è possibile usare per distribuire Always On VPN. Oltre ai server DC/DNS, la distribuzione di Always On VPN richiede un server server dei criteri di rete (RADIUS), un server dell'autorità di certificazione (CA) e un server di accesso remoto (routing/VPN). Una volta configurata l'infrastruttura, è necessario registrare i client e quindi connettere i client al sito locale in modo sicuro tramite diverse modifiche alla rete.

- Active Directory infrastruttura del dominio, inclusi uno o più server DNS (Domain Name System). Sono necessarie entrambe le zone di Domain Name System interno ed esterno (DNS), che presuppone che la zona interna sia un sottodominio delegato della zona esterna, ad esempio corp.contoso.com e contoso.com.
- Infrastruttura a chiave pubblica (PKI) basata su Active Directory e Servizi certificati Active Directory (AD CS).
- Server, virtuale o fisico, esistente o nuovo, per installare Server dei criteri di rete. Se nella rete sono già presenti server NPS, è possibile modificare una configurazione del server dei criteri di rete esistente anziché aggiungere un nuovo server.
- Accesso remoto come server VPN gateway RAS con un piccolo subset di funzionalità che supportano le connessioni VPN IKEv2 e il routing LAN.
- Rete perimetrale che include due firewall.  Assicurarsi che i firewall consentano il corretto funzionamento del traffico necessario per le comunicazioni VPN e RADIUS. Per ulteriori informazioni, vedere [Always on Panoramica della tecnologia VPN](../always-on-vpn-technology-overview.md).
- Server fisico o macchina virtuale (VM) nella rete perimetrale con due schede di rete Ethernet fisiche per installare accesso remoto come server VPN gateway RAS. Le macchine virtuali richiedono la LAN virtuale (VLAN) per l'host. 
- L'appartenenza al gruppo Administrators o a un gruppo equivalente è il requisito minimo necessario.
- Leggere la sezione relativa alla pianificazione di questa guida per assicurarsi di essere pronti per la distribuzione prima di eseguire la distribuzione.
- Esaminare le guide di progettazione e distribuzione per ognuna delle tecnologie utilizzate. Queste guide consentono di determinare se gli scenari di distribuzione forniscono i servizi e la configurazione necessari per la rete dell'organizzazione. Per ulteriori informazioni, vedere [Always on Panoramica della tecnologia VPN](../always-on-vpn-technology-overview.md).
- Piattaforma di gestione preferita per la distribuzione della configurazione VPN Always On perché il CSP non è specifico del fornitore.

>[!IMPORTANT]
>Per questa distribuzione, non è necessario che i server di infrastruttura, ad esempio i computer che eseguono Active Directory Domain Services, Active Directory Servizi certificati e il server dei criteri di rete, eseguano Windows Server 2016. È possibile utilizzare le versioni precedenti di Windows Server, ad esempio Windows Server 2012 R2, per i server di infrastruttura e per il server che esegue accesso remoto.
>
>Non tentare di distribuire accesso remoto in una macchina virtuale (VM) in Microsoft Azure. L'uso di accesso remoto in Microsoft Azure non è supportato, incluse le connessioni VPN di accesso remoto e DirectAccess. Per ulteriori informazioni, vedere [supporto del software server Microsoft per Microsoft Azure macchine virtuali](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

## <a name="about-this-deployment"></a>Informazioni su questa distribuzione

Le istruzioni fornite illustrano come distribuire accesso remoto come gateway RAS VPN a tenant singolo per le connessioni VPN da punto a sito, usando uno degli scenari indicati di seguito, per i computer client remoti che eseguono Windows 10. Sono inoltre disponibili istruzioni per la modifica di alcune infrastrutture esistenti per la distribuzione. In questa distribuzione sono inoltre disponibili collegamenti che consentono di ottenere ulteriori informazioni sul processo di connessione VPN, i server da configurare, il nodo ProfileXML VPNv2 CSP e altre tecnologie per la distribuzione di Always On VPN.

**Scenari di distribuzione di Always On VPN:**

1. Distribuisci solo Always On VPN.
2. Distribuire Always On VPN con accesso condizionale per la connettività VPN usando Azure AD.

Per ulteriori informazioni e flusso di lavoro degli scenari presentati, vedere [Deploy always on VPN](always-on-vpn-deploy-deployment.md).

## <a name="what-isnt-provided-in-this-deployment"></a>Cosa non è disponibile in questa distribuzione

Questa distribuzione non fornisce istruzioni per:

- Active Directory Domain Services (AD DS).
- Active Directory Servizi certificati (AD CS) e un'infrastruttura a chiave pubblica (PKI).
- Dynamic Host Configuration Protocol (DHCP).
- Hardware di rete, ad esempio cablaggio Ethernet, firewall, commutatori e Hub.
- Risorse di rete aggiuntive, ad esempio applicazioni e file server, a cui gli utenti remoti possono accedere tramite una connessione VPN Always On.
- Connettività Internet o accesso condizionale per la connettività Internet con Azure AD. Per informazioni dettagliate, vedere [accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-steps"></a>Passaggi successivi

- [Altre informazioni sulle funzionalità e sulle funzionalità di Always On VPN](../../vpn-map-da.md)

- [Scopri di più sui miglioramenti apportati alla VPN Always On](../always-on-vpn-enhancements.md)

- [Informazioni su alcune delle funzionalità avanzate di Always On VPN](always-on-vpn-adv-options.md)

- [Scopri di più sulla tecnologia VPN Always On](../always-on-vpn-technology-overview.md)

- [Inizia a pianificare la distribuzione di Always On VPN](always-on-vpn-deploy-deployment.md)