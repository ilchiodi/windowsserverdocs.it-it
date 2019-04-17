---
title: Distribuire VPN Always On
description: Questo argomento fornisce istruzioni dettagliate per la distribuzione di VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067326"
---
# Distribuire VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #0171; [ **Precedente:** conoscere la VPN Always On funzionalità avanzate](always-on-vpn-adv-options.md)<br>
& #0187; [ **Successivo:** passaggio 1. Iniziare a pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md)

In questa sezione scoprirai il flusso di lavoro per la distribuzione di connessioni VPN Always On per i computer remoti client per Windows 10 aggiunti al dominio. Se si desidera **configurare l'accesso condizionale** ottimizzare il modo in cui gli utenti della VPN accedono alle risorse, vedi [l'accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Per ulteriori informazioni sull'accesso condizionale per la connettività VPN con Azure AD, vedi [l'accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 


Il diagramma seguente illustra il processo del flusso di lavoro per i diversi scenari durante la distribuzione di VPN Always On: 

_Fai clic per ingrandire l'immagine_.

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![miniatura](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>Per la distribuzione, non è un requisito che il server di infrastruttura, ad esempio i computer che eseguono Server dei criteri di rete, servizi certificati Active Directory e Active Directory Domain Services sono in esecuzione Windows Server 2016. È possibile utilizzare le versioni precedenti di Windows Server, ad esempio Windows Server 2012 R2, per i server di infrastruttura e il server che esegue l'accesso remoto.

## [Passaggio 1. Pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md)

In questo passaggio, iniziare a pianificare e preparare la distribuzione di VPN Always On. Prima di installare il ruolo del server Accesso remoto nel computer di cui che si intende sull'utilizzo come server VPN. Dopo la pianificazione corretta, puoi distribuire VPN Always On e facoltativamente configurare l'accesso condizionale per la connettività VPN con Azure AD.

## [Passaggio 2. Configurare l'infrastruttura del server VPN Always On](vpn-deploy-server-infrastructure.md)

In questo passaggio, installare e configurare i componenti sul lato server necessari per supportare la connessione VPN. I componenti sul lato server includono la configurazione dell'infrastruttura PKI per distribuire i certificati usati da utenti, il server VPN e il server dei criteri di rete.  Anche configurare RRAS per supportare le connessioni IKEv2 e il server dei criteri di rete per eseguire l'autorizzazione per le connessioni VPN.

Per configurare l'infrastruttura di server, è necessario eseguire le attività seguenti:
- **In un server configurato con servizi di dominio Active Directory:** Abilitare la registrazione automatica dei certificati in Criteri di gruppo per utenti e computer, creare il gruppo di utenti VPN, il gruppo di server VPN e il gruppo di server dei criteri di rete e aggiungere i membri per ogni gruppo.
- **Su un Server di certificati CA di Active Directory:** Creare modelli di certificato di autenticazione utente, l'autenticazione Server VPN e autenticazione Server dei criteri di rete.
- **Nei client aggiunto al dominio di Windows 10:** Eseguire la registrazione e la convalida dei certificati utente.

## [Passaggio 3. Configurare il server di accesso remoto per VPN Always On](vpn-deploy-ras.md)

In questo passaggio, configurare l'accesso remoto VPN per consentire le connessioni VPN IKEv2, Nega le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio di indirizzi IP per la connessione client VPN autorizzati.

Per configurare RAS, è necessario eseguire le attività seguenti:
- Eseguire la registrazione e convalidare il certificato del server VPN
- Installare e configurare l'accesso remoto VPN

## [Passaggio 4. Installare e configurare il Server dei criteri di rete](vpn-deploy-nps.md)

In questo passaggio, installare Server dei criteri di rete (NPS) usando Windows PowerShell o il Server Manager Aggiungi ruoli e Features Wizard. Anche configurare dei criteri di rete per gestire tutti i autenticazione, autorizzazione e compiti contabilità per la richiesta di connessione che riceve dal server VPN.

Per configurare criteri di rete, è necessario eseguire le attività seguenti:
- Registrare il Server dei criteri di rete in Active Directory
- Configurare RADIUS Accounting del server dei criteri di rete
- Aggiungere il Server VPN come Client RADIUS in Criteri di rete
- Configurare i criteri di rete in Criteri di rete
- Registrazione automatica certificato del Server dei criteri di rete

## [Passaggio 5. Configurare DNS e impostazioni del Firewall per sempre in VPN](vpn-deploy-dns-firewall.md)

In questo passaggio, configurare DNS e Firewall impostazioni. Quando i client VPN remoti si connettono, usano gli stessi server DNS che usano i client interni, consentendo loro di risolvere i nomi nello stesso modo nel resto della tua workstation interne. 

## [Passaggio 6. Configurare le connessioni VPN Always On dei client Windows 10](vpn-deploy-client-vpn-connections.md)

In questo passaggio, configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. È possibile utilizzare varie tecnologie per configurare i client VPN di Windows 10, incluso Windows PowerShell, System Center Configuration Manager e Intune. Tutti e tre richiedono un profilo VPN di XML per configurare le impostazioni VPN appropriate. 

## [Passaggio 7. (Facoltativo) Configurare l'accesso condizionale per la connettività VPN](../../ad-ca-vpn-connectivity-windows10.md) 
In questo passaggio facoltativo, puoi ottimizzare VPN autorizzato come gli utenti di accedere alle risorse. Con l'accesso condizionale di Azure AD per la connettività VPN, puoi proteggere le connessioni VPN. Accesso condizionale è un motore di valutazione basato su criteri che consente di creare regole di accesso per qualsiasi Azure AD applicazione connessa. Per altre informazioni, vedi [l'accesso condizionale di Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).


## Passaggio successivo
[Passaggio 1. Pianificare la distribuzione di VPN Always On](always-on-vpn-deploy-planning.md): prima di installare il ruolo del server Accesso remoto nel computer di cui si intende sull'utilizzo come server VPN. Dopo la pianificazione corretta, puoi distribuire VPN Always On e facoltativamente configurare l'accesso condizionale per la connettività VPN con Azure AD.  



---