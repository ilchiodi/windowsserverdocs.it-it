---
title: Azure Active Directory Domain Services e Servizi Desktop remoto
description: Descrive come integrare Azure AD Domain Services alla distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/02/2017
ms.tgt_pltfrm: na
ms.topic: article
author: christianmontoya
ms.localizationpriority: medium
ms.openlocfilehash: 8b1baf642ffa3c8e8a0a2cfc70d2f49b58f208b3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446587"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integrare Azure AD Domain Services con la distribuzione di Servizi Desktop remoto

È possibile usare [Azure Active Directory Domain Services](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) nella distribuzione di Servizi Desktop remoto al posto di Windows Server Active Directory. Azure Active Directory Domain Services consente di usare le identità di Azure AD esistenti con i carichi di lavoro di Windows classici.

Con Azure Active Directory Domain Services è possibile: 
- Creare un ambiente Azure con un dominio locale per le organizzazioni nato-in-the-cloud. 
- Creare un ambiente di Azure isolato con le stesse identità usata per le origini locali e ambiente online, senza la necessità di creare una VPN site-to-site o ExpressRoute. 

Al termine l'integrazione di Azure Active Directory Domain Services alla distribuzione di Desktop remoto, l'architettura avrà un aspetto simile al seguente:

![Un diagramma dell'architettura con Servizi Desktop remoto con Azure Active Directory Domain Services](media/aadds-rds.png)

Per verificare come questa architettura con altri scenari di distribuzione di servizi desktop remoto, estrarre [architetture di Servizi Desktop remoto](desktop-hosting-logical-architecture.md).

Per ottenere una migliore comprensione di Azure Active Directory Domain Services, consultare il [Panoramica di Azure AD DS](/azure/active-directory-domain-services/active-directory-ds-overview) e [come decidere se Azure Active Directory Domain Services è più adatta al proprio caso d'uso](/azure/active-directory-domain-services/active-directory-ds-comparison).

Usare le informazioni seguenti per distribuire Azure Active Directory Domain Services con Servizi Desktop remoto.

## <a name="prerequisites"></a>Prerequisiti

Prima di connettere le identità da Azure AD da usare in una distribuzione di servizi desktop remoto [configurare Azure AD per salvare le password con hash per le identità degli utenti](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Nati-in-the-cloud alle organizzazioni non è necessario apportare altre modifiche nella propria directory; le organizzazioni in locale, tuttavia, necessario consentire gli hash delle password sincronizzate e archiviate in Azure AD, che potrebbe non essere consentito per alcune organizzazioni. Gli utenti dovranno reimpostare la password dopo aver apportato questa modifica della configurazione.

## <a name="deploy-azure-ad-ds-and-rds"></a>Distribuire servizi desktop remoto e Azure Active Directory Domain Services 
Usare la procedura seguente per distribuire Azure Active Directory e servizi desktop remoto.

1. Abilitare [Azure Active Directory Domain Services](/azure/active-directory-domain-services/active-directory-ds-getting-started). Si noti che l'articolo collegato esegue le operazioni seguenti:
   - Procedura dettagliata per creare l'oggetto appropriato gruppi Azure AD per l'amministrazione del dominio.
   - Evidenzia quando potrebbe essere necessario imporre agli utenti di modificare la password in modo che gli account personali possono lavorare con Azure Active Directory Domain Services.
   
2. Configurare Servizi Desktop remoto. È possibile usare un modello di Azure o distribuire servizi desktop remoto manualmente.
   - Usare la [Existing AD modello](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Assicurarsi di personalizzare quanto segue:
   
     - **Impostazioni**
       - **Gruppo di risorse**: Usare il gruppo di risorse in cui si desidera creare le risorse di servizi desktop remoto.
         > [!NOTE] 
         > Attualmente, deve essere stesso gruppo di risorse in cui è presente la rete virtuale di Azure resource manager.

       - **Prefisso con etichetta DNS**: Immettere l'URL che si desidera che gli utenti da usare per accesso Web desktop remoto.
       - **Nome di dominio ad**: Immettere il nome completo dell'istanza di Azure AD, ad esempio, "contoso.onmicrosoft.com" o "contoso.com".
       - **Nome della rete virtuale di Active Directory** e **nome Subnet Ad**: Immettere gli stessi valori che è utilizzato per creare la rete virtuale di Azure resource manager. Si tratta della subnet a cui dovranno connettersi le risorse di servizi desktop remoto.
       - **Nome utente amministratore** e **Admin Password**: Immettere le credenziali per un utente amministratore che è un membro del **AAD DC Administrators** gruppo in Azure AD.
   
     - **modello**
        - Rimuovere tutte le proprietà del **dnsServers**: dopo aver selezionato **modifica modello** dalla pagina del modello di avvio rapido di Azure, cercare "dnsServers" e rimuovere la proprietà. 

           Ad esempio, prima di rimuovere il **dnsServers** proprietà:
      
           ![Modello di avvio rapido di Azure con la proprietà dnsSettings](media/rds-remove-dnssettings-before.png)

           Ed ecco lo stesso file dopo la rimozione della proprietà:

           ![Modello di avvio rapido di Azure con la proprietà dnsSettings rimosso](media/rds-remove-dnssettings-after.png)
   
   - [Distribuire servizi desktop remoto manualmente](rds-deploy-infrastructure.md). 

