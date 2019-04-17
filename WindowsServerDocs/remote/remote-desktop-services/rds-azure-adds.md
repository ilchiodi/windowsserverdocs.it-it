---
title: Servizi di dominio Active Directory Azure e Servizi Desktop remoto
description: Informazioni su come integrare servizi di dominio Active Directory Azure nella distribuzione di RDS.
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
ms.openlocfilehash: e60cf70f1f91ad87046bedf024fe9afc459075b6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1614518"
---
# <a name="integrate-azure-ad-domain-services-with-your-rds-deployment"></a>Integrare servizi di dominio Active Directory Azure con la distribuzione di RDS

È possibile utilizzare [servizi di dominio Active Directory di Azure](/azure/active-directory-domain-services/active-directory-ds-overview) (Azure AD DS) nella distribuzione di Servizi Desktop remoto al posto di Windows Server Active Directory. Azure Active Directory consente di utilizzare l'identità di Azure Active Directory esistenti con carichi di lavoro di Windows classiche.

Con Azure Active Directory è possibile: 
- Creare un ambiente Azure con un dominio locale per le organizzazioni nati in-the cloud. 
- Creare un ambiente isolato Azure con la stessa identità utilizzata per le organizzazioni locali e ambiente online, senza la necessità di creare un sito per sito VPN o ExpressRoute. 

Una volta terminata l'integrazione di Azure Active Directory in distribuzione di Desktop remoto, l'architettura sarà simile al seguente:

![Un diagramma di architettura con RDS con Azure Active Directory](media/aadds-rds.png)

Per visualizzare come questa architettura vengono messi a confronto con altri scenari di distribuzione di RDS, consultare [le architetture di Servizi Desktop remoto](desktop-hosting-logical-architecture.md).

Per ottenere una migliore comprensione di Azure Active Directory, consultare la [Panoramica di Azure Active Directory](/azure/active-directory-domain-services/active-directory-ds-overview) e su [come decidere se Azure Active Directory è adatto per il caso di utilizzo](/azure/active-directory-domain-services/active-directory-ds-comparison).

Utilizzare le informazioni seguenti per distribuire Azure Active Directory con RDS.

## <a name="prerequisites"></a>Prerequisiti

Prima che è possibile raggruppare le identità di Azure Active Directory da utilizzare in una distribuzione di RDS, [configurare Azure Active Directory per salvare per le password per le identità degli utenti](/azure/active-directory-domain-services/active-directory-ds-getting-started-password-sync). Le organizzazioni nati in-the cloud non è necessario apportare modifiche aggiuntive nella propria directory; organizzazioni locali, tuttavia, è necessario consentire gli hash password sincronizzati e archiviati in Azure Active Directory, non può essere ridotta in alcune organizzazioni. Gli utenti dovranno reimpostare le password dopo aver apportato la modifica di configurazione.

## <a name="deploy-azure-ad-ds-and-rds"></a>Distribuzione di RDS e Azure Active Directory 
Utilizzare la procedura seguente per distribuire Azure Active Directory di dominio Active Directory e RDS.

1. Attivare [Azure Active Directory](/azure/active-directory-domain-services/active-directory-ds-getting-started). Si noti che l'articolo collegato esegue le operazioni seguenti:
   - È possibile creare appropriata gruppi di Azure Active Directory per l'amministrazione del dominio.
   - Evidenziare quando potrebbe essere necessario imporre agli utenti di modificare la password in modo che i relativi account possono lavorare con Azure Active Directory.
   
2. Configurazione di RDS. È possibile utilizzare un modello di Azure o distribuire servizi desktop remoto manualmente.
   - Utilizzare il [modello di Active Directory esistente](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/). Verificare che personalizzare quanto segue:
   
      - **Impostazioni**
         - **Gruppo di risorse**: utilizzo del gruppo di risorse di cui si desidera creare le risorse RDS.
         > [!NOTE] 
         > In questo momento deve essere nello stesso gruppo di risorse di cui è presente la rete virtuale a Gestione risorse di Azure.

         - **Prefisso etichetta DNS**: immettere l'URL che si desidera utilizzare per accedere a Web desktop remoto.
         - **Nome di dominio Active Directory**: immettere il nome completo dell'istanza di Azure Active Directory, ad esempio "contoso.onmicrosoft.com" o "contoso.com".
         - **Nome Vnet Active Directory** e **Il nome di Subnet di Active Directory**: immettere gli stessi valori utilizzati durante la creazione, la rete virtuale a Gestione risorse di Azure. Si tratta della subnet a cui si connetteranno le risorse RDS.
         - **Nome utente** e amministratore **Admin Password**: immettere le credenziali per un utente di amministrazione che è un membro del gruppo **Administrators di controller di dominio AAD** in Azure Active Directory.
   
      - **Modello**
         - Rimuovere tutte le proprietà di **dnsServers**: dopo avere selezionato **Modifica modello** pagina modello quickstart Azure, cercare "dnsServers" e rimuovere la proprietà. 

            Ad esempio, prima di rimuovere la proprietà **dnsServers** :
      
            ![Modello di Azure quickstart con proprietà dnsSettings](media/rds-remove-dnssettings-before.png)

            Di seguito è lo stesso file dopo la rimozione della proprietà:

            ![Modello di Azure quickstart con proprietà dnsSettings rimossi](media/rds-remove-dnssettings-after.png)
   
   - [RDS distribuire manualmente](rds-deploy-infrastructure.md). 

