---
title: Azure Active Directory Domain Services e Servizi Desktop remoto
description: Informazioni su come integrare Azure AD Domain Services in una distribuzione di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: bc70300df8bc8aef78371f4b84fe697bf8e66749
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852984"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integrare Azure AD Domain Services con una distribuzione di Servizi Desktop remoto

È possibile usare [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) in una distribuzione di Servizi Desktop remoto al posto di Windows Server Active Directory. Azure AD Domain Services consente di usare le identità di Azure AD esistenti con carichi di lavoro di Windows classici.

Con Azure AD Domain Services è possibile: 
- Creare un ambiente Azure con un dominio locale per organizzazioni nate nel cloud. 
- Creare un ambiente Azure isolato con le stesse identità usate per l'ambiente locale e online, senza la necessità di creare una VPN da sito a sito o ExpressRoute. 

Al termine dell'integrazione di Azure AD Domain Services nella distribuzione di Desktop remoto, l'architettura avrà un aspetto simile al seguente:

![Diagramma di architettura che mostra Servizi Desktop remoto con Azure AD Domain Services](media/aadds-rds.png)

Per confrontare questa architettura con altri scenari di distribuzione di Servizi Desktop remoto, vedi [Architetture di Servizi Desktop remoto](desktop-hosting-logical-architecture.md).

Per una migliore comprensione di Azure AD Domain Services, leggi [Panoramica di Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) e [Come decidere se Azure AD Domain Services è adatto al proprio caso d'uso](/azure/active-directory-domain-services/active-directory-ds-comparison).

Usa le informazioni seguenti per distribuire Azure AD Domain Services con Servizi Desktop remoto.

## <a name="prerequisites"></a>Prerequisiti

Prima di usare le tue identità di Azure AD in una distribuzione di Servizi Desktop remoto, [configura Azure AD per salvare le password con hash per le identità degli utenti](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Le organizzazioni nate nel cloud non devono apportare altre modifiche nella propria directory. Le organizzazioni locali tuttavia devono consentire la sincronizzazione delle password con hash e l'archiviazione in Azure AD, operazioni che potrebbero non essere consentite per alcune organizzazioni. Gli utenti dovranno reimpostare le password dopo aver apportato questa modifica della configurazione.

## <a name="deploy-azure-ad-ds-and-rds"></a>Distribuire Azure AD Domain Services e Servizi Desktop remoto 
Segui la procedura seguente per distribuire Azure AD Domain Services e Servizi Desktop remoto.

1. Abilita [Azure AD Domain Services](/azure/active-directory-domain-services/active-directory-ds-getting-started). L'articolo accessibile dal collegamento fornisce le informazioni seguenti:
   - Fornisce una procedura dettagliata per creare gruppi Azure AD appropriati per l'amministrazione del dominio.
   - Evidenzia quando potrebbe essere necessario imporre agli utenti di cambiare la password in modo che gli account possano funzionare con Azure AD Domain Services.
   
2. Configura Servizi Desktop remoto. Puoi usare un modello di Azure o distribuire Servizi Desktop remoto manualmente.
   - Usa il [modello di AD esistente](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Personalizza quanto segue:
   
     - **Impostazioni**
       - **Gruppo di risorse**: usa il gruppo di risorse in cui vuoi creare le risorse di Servizi Desktop remoto.
         > [!NOTE] 
         > Attualmente deve essere lo stesso gruppo di risorse in cui è presente la rete virtuale di Azure Resource Manager.

       - **Dns Label Prefix** (Prefisso etichetta DNS): immetti l'URL che deve essere usato dagli utenti per l'accesso al Web Desktop remoto.
       - **Nome di dominio di Active Directory**: immetti il nome completo dell'istanza di Azure AD, ad esempio "contoso.onmicrosoft.com" o "contoso.com".
       - **Ad Vnet Name** (Nome rete virtuale AD) e **Ad Subnet Name** (Nome subnet AD): immetti gli stessi valori usati per creare la rete virtuale di Azure Resource Manager. Si tratta della subnet a cui si connetteranno le risorse di Servizi Desktop remoto.
       - **Nome utente amministratore** e **Password amministratore**: immetti le credenziali di un utente amministratore membro del gruppo **Amministratori di AAD DC** in Azure AD.
   
     - **Modello**
        - Rimuovi tutte le proprietà di **dnsServers**: dopo aver selezionato **Modifica modello** dalla pagina del modello di avvio rapido di Azure, cerca "dnsServers" e rimuovi la proprietà. 

           Ad esempio, prima di rimuovere la proprietà **dnsServers**:
      
           ![Modello di avvio rapido di Azure con la proprietà dnsSettings](media/rds-remove-dnssettings-before.png)

           Ed ecco lo stesso file dopo la rimozione della proprietà:

           ![Modello di avvio rapido di Azure con la proprietà dnsSettings](media/rds-remove-dnssettings-after.png)
   
   - [Distribuisci Servizi Desktop remoto manualmente](rds-deploy-infrastructure.md). 

