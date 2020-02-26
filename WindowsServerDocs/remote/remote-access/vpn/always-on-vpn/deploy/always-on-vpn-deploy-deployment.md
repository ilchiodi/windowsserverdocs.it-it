---
title: Distribuire VPN Always On
description: Questo argomento fornisce istruzioni dettagliate per la distribuzione di Always On VPN in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8383aea81e91f52473fd564abf315423abfa83b1
ms.sourcegitcommit: 9bc7a0478d72944f714f8041fa4506e0d1ed0366
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/25/2020
ms.locfileid: "77607074"
---
# <a name="deploy-always-on-vpn"></a>Distribuire VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Informazioni sulle funzionalità avanzate di Always On VPN](always-on-vpn-adv-options.md)
- [Passaggio **successivo:** Passaggio 1. Iniziare a pianificare la distribuzione di Always On VPN](always-on-vpn-deploy-planning.md)

Questa sezione illustra il flusso di lavoro per la distribuzione di Always On connessioni VPN per computer client Windows 10 aggiunti a un dominio remoto. Se si vuole **configurare l'accesso condizionale** per ottimizzare il modo in cui gli utenti VPN accedono alle risorse, vedere [accesso condizionale per la connettività VPN con Azure ad](../../ad-ca-vpn-connectivity-windows10.md). Per altre informazioni sull'accesso condizionale per la connettività VPN con Azure AD, vedere [accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 

Il diagramma seguente illustra il processo del flusso di lavoro per i diversi scenari durante la distribuzione di Always On VPN:

[diagramma di flusso ![del flusso di lavoro di distribuzione VPN Always On](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png)

> [!IMPORTANT]
> Per questa distribuzione, non è necessario che i server di infrastruttura, ad esempio i computer che eseguono Active Directory Domain Services, Active Directory Servizi certificati e il server dei criteri di rete, eseguano Windows Server 2016. È possibile utilizzare le versioni precedenti di Windows Server, ad esempio Windows Server 2012 R2, per i server di infrastruttura e per il server che esegue accesso remoto.

## <a name="step-1-plan-the-always-on-vpn-deployment"></a>[Passaggio 1. Pianificare la distribuzione di Always On VPN](always-on-vpn-deploy-planning.md)

In questo passaggio si inizia a pianificare e preparare la distribuzione di Always On VPN. Prima di installare il ruolo del server accesso remoto nel computer che si sta pianificando di utilizzare come server VPN. Dopo la pianificazione corretta, è possibile distribuire Always On VPN e, facoltativamente, configurare l'accesso condizionale per la connettività VPN usando Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructure"></a>[Passaggio 2. Configurare l'infrastruttura del server VPN Always On](vpn-deploy-server-infrastructure.md)

In questo passaggio si installano e configurano i componenti lato server necessari per supportare la VPN. I componenti lato server includono la configurazione dell'infrastruttura a chiave pubblica per distribuire i certificati utilizzati dagli utenti, dal server VPN e dal server dei criteri di rete.  È inoltre possibile configurare RRAS per supportare le connessioni IKEv2 e il server NPS per eseguire l'autorizzazione per le connessioni VPN.

Per configurare l'infrastruttura server di, è necessario eseguire le attività seguenti:

- **In un server configurato con Active Directory Domain Services:** Abilitare la registrazione automatica dei certificati in Criteri di gruppo sia per computer che per utenti, creare il gruppo utenti VPN, il gruppo server VPN e il gruppo Server dei criteri di rete e aggiungere membri a ogni gruppo.
- **In un Active Directory CA del server di certificazione:** Creare i modelli di certificato di autenticazione utente, autenticazione server VPN e server dei criteri di rete.
- **Nei client Windows 10 aggiunti a un dominio:** Registrare e convalidare i certificati utente.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>[Passaggio 3. Configurare il server di accesso remoto per la VPN Always On](vpn-deploy-ras.md)

In questo passaggio viene configurata la VPN di accesso remoto per consentire le connessioni VPN IKEv2, negare le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio degli indirizzi IP per la connessione dei client VPN autorizzati.

Per configurare RAS, è necessario eseguire le attività seguenti:

- Registrare e convalidare il certificato del server VPN
- Installare e configurare VPN di accesso remoto

## <a name="step-4-install-and-configure-the-nps-server"></a>[Passaggio 4. Installare e configurare il server NPS](vpn-deploy-nps.md)

In questo passaggio viene installato Server dei criteri di rete utilizzando Windows PowerShell o l'Server Manager aggiunta guidata ruoli e funzionalità. Viene inoltre configurato il server dei criteri di rete per gestire tutti i compiti di autenticazione, autorizzazione e accounting per la richiesta di connessione ricevuta dal server VPN.

Per configurare NPS, è necessario eseguire le attività seguenti:

- Registrare il server NPS in Active Directory
- Configurare l'accounting RADIUS per il server NPS
- Aggiungere il server VPN come client RADIUS in server dei criteri di rete
- Configurare i criteri di rete nel server dei criteri di rete
- Registrare automaticamente il certificato del server NPS

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpn"></a>[Passaggio 5. Configurare le impostazioni di DNS e firewall per la VPN Always On](vpn-deploy-dns-firewall.md)

In questo passaggio vengono configurate le impostazioni di DNS e del firewall. Quando i client VPN remoti si connettono, usano gli stessi server DNS usati dai client interni, che consentono loro di risolvere i nomi in modo analogo alle altre workstation interne. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>[Passaggio 6. Configurare le connessioni VPN Always On client Windows 10](vpn-deploy-client-vpn-connections.md)

In questo passaggio si configureranno i computer client Windows 10 in modo che comunichino con tale infrastruttura con una connessione VPN. È possibile usare diverse tecnologie per configurare i client VPN di Windows 10, tra cui Windows PowerShell, Microsoft endpoint Configuration Manager e Intune. Per configurare le impostazioni VPN appropriate, tutte e tre richiedono un profilo VPN XML.

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivity"></a>[Passaggio 7. Opzionale Configurare l'accesso condizionale per la connettività VPN](../../ad-ca-vpn-connectivity-windows10.md)

In questo passaggio facoltativo è possibile ottimizzare il modo in cui gli utenti VPN autorizzati accedono alle risorse. Con Azure AD l'accesso condizionale per la connettività VPN, è possibile proteggere le connessioni VPN. L'accesso condizionale è un motore di valutazione basato su criteri che consente di creare regole di accesso per qualsiasi applicazione Azure AD connessa. Per ulteriori informazioni, vedere [Azure Active Directory (Azure ad) accesso condizionale](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-step"></a>Passaggio successivo

[Passaggio 1. Pianificare la distribuzione di Always On VPN](always-on-vpn-deploy-planning.md): prima di installare il ruolo del server accesso remoto nel computer che si sta pianificando di usare come server VPN. Dopo la pianificazione corretta, è possibile distribuire Always On VPN e, facoltativamente, configurare l'accesso condizionale per la connettività VPN usando Azure AD.  
