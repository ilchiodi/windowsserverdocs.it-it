---
title: Distribuire VPN Always On
description: In questo argomento vengono fornite istruzioni dettagliate per la distribuzione VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6849d8547885b10fb32a133f09a82b6f133a535e
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749654"
---
# <a name="deploy-always-on-vpn"></a>Distribuire VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Scopri la VPN Always On funzionalità avanzate](always-on-vpn-adv-options.md)
- [**prossimo:** Passaggio 1. Iniziare a pianificare la distribuzione VPN Always On](always-on-vpn-deploy-planning.md)

In questa sezione illustra il flusso di lavoro per la distribuzione di connessioni VPN Always On per i computer remoti client per Windows 10 aggiunti a un dominio. Se si desidera **configurare l'accesso condizionale** per ottimizzare il modo in cui gli utenti VPN accedono alle risorse, vedere [accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Per altre informazioni sull'accesso condizionale per la connettività VPN con Azure AD, vedere [accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 

Il diagramma seguente illustra il processo del flusso di lavoro per i diversi scenari durante la distribuzione VPN Always On:

![Diagramma di flusso del flusso di lavoro di distribuzione VPN Always On](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)

>[!IMPORTANT]
>Per questa distribuzione, non è un requisito che i server di infrastruttura, ad esempio i computer che eseguono servizi di dominio Active Directory, servizi certificati Active Directory e Server dei criteri di rete, siano in esecuzione Windows Server 2016. È possibile usare le versioni precedenti di Windows Server, ad esempio Windows Server 2012 R2, per il server di infrastruttura e per il server che esegue l'accesso remoto.

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[Passaggio 1. Pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md)

In questo passaggio, si inizia a pianificare e preparare la distribuzione VPN Always On. Prima di installare il ruolo server Accesso remoto nel computer si intende utilizzare come server VPN. Dopo la corretta pianificazione, è possibile distribuire VPN Always On e, facoltativamente, configurare l'accesso condizionale per la connettività VPN con Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[Passaggio 2. Configurare l'infrastruttura del server VPN Always On](vpn-deploy-server-infrastructure.md)

In questo passaggio installare e configurare i componenti lato server necessari per supportare la connessione VPN. I componenti lato server è inclusa la configurazione di infrastruttura a chiave pubblica per distribuire i certificati usati da utenti, il server VPN e il server NPS.  È anche possibile configurare RRAS per supportare connessioni IKEv2 e il server dei criteri di rete per eseguire l'autorizzazione per le connessioni VPN.

Per configurare l'infrastruttura di server, è necessario eseguire le attività seguenti:

- **In un server configurato con Active Directory Domain Services:** Abilitare la registrazione automatica dei certificati in Criteri di gruppo per utenti e computer, creare il gruppo di utenti di VPN, il gruppo di server VPN e il gruppo di server dei criteri di rete e aggiungere membri a ogni gruppo.
- **Nel Server Active Directory certificato autorità di certificazione:** Creare i modelli di certificato di autenticazione dell'utente, l'autenticazione Server VPN e l'autenticazione del Server dei criteri di rete.
- **Nei client Windows 10 aggiunti a un dominio:** Registrare e convalidare i certificati utente.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[Passaggio 3. Configurare il server di accesso remoto per VPN Always On](vpn-deploy-ras.md)

In questo passaggio si configura accesso remoto VPN per consentire le connessioni VPN IKEv2, negare le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio degli indirizzi IP per la connessione client VPN autorizzati.

Per configurare server di accesso remoto, è necessario eseguire le attività seguenti:

- Registrare e convalidare il certificato del server VPN
- Installare e configurare accesso remoto VPN

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[Passaggio 4. Installare e configurare il server dei criteri di rete](vpn-deploy-nps.md)

In questo passaggio per installare Server dei criteri di rete (NPS), usando Windows PowerShell o il Server Manager Aggiungi ruoli e funzionalità guidata. È anche possibile configurare criteri di rete per gestire tutti l'autenticazione, autorizzazione e compiti di contabilità per la richiesta di connessione ricevuto dal server VPN.

Per configurare criteri di rete, è necessario eseguire le attività seguenti:

- Registrare il Server NPS in Active Directory
- Configurare l'Accounting per il Server NPS RADIUS
- Aggiungere il Server VPN come Client RADIUS in Criteri di rete
- Configurare criteri di rete in Criteri di rete
- Registrazione automatica del certificato del Server dei criteri di rete

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[Passaggio 5. Configurare DNS e le impostazioni del Firewall per AlwaysOn in una VPN](vpn-deploy-dns-firewall.md)

In questo passaggio si configurare DNS e del Firewall delle impostazioni. Quando si connettono i client VPN remoti, usano gli stessi server DNS utilizzati dai client interni, che consente loro di risolvere i nomi esattamente come il resto della workstation di interno. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[Passaggio 6. Configurare le connessioni VPN Always On dei client Windows 10](vpn-deploy-client-vpn-connections.md)

In questo passaggio si configura il computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. È possibile utilizzare diverse tecnologie per configurare i client VPN di Windows 10, tra cui Windows PowerShell, System Center Configuration Manager e Intune. Tutte e tre richiedono un profilo VPN XML per configurare le impostazioni VPN appropriate.

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[Passaggio 7. (Facoltativo) Configurare l'accesso condizionale per la connettività VPN](../../ad-ca-vpn-connectivity-windows10.md)

In questo passaggio facoltativo, è possibile ottimizzare VPN modo in cui gli utenti accedono alle risorse. Con accesso condizionale di Azure AD per la connettività VPN, è possibile proteggere le connessioni VPN. Accesso condizionale è un motore di valutazione basata su criteri che consente di creare regole di accesso per qualsiasi Azure AD applicazione connessa. Per altre informazioni, vedere [accesso condizionale di Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-step"></a>Passaggio successivo

[Passaggio 1. Pianificare la distribuzione VPN Always On](always-on-vpn-deploy-planning.md): Prima di installare il ruolo server Accesso remoto nel computer si intende utilizzare come server VPN. Dopo la corretta pianificazione, è possibile distribuire VPN Always On e, facoltativamente, configurare l'accesso condizionale per la connettività VPN con Azure AD.  
