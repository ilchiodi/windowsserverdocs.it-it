---
title: Gestire SharePoint Online in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 282f3634-6de6-4691-803c-df6c3c16660d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f6f0a29759fa8bdb098576cdb32a71d8568fb465
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852724"
---
# <a name="manage-sharepoint-online-in-windows-server-essentials"></a>Gestire SharePoint Online in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile gestire le raccolte e i siti del team di SharePoint Online dal dashboard, senza accedere a Office 365, se si integra Office 365 con il server di Windows Server Essentials. È possibile ottenere le raccolte e i siti del team di SharePoint Online con qualsiasi piano aziendale di Office 365. [Ecco come integrare Office 365 con il server.](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
 Come bonus, gli utenti potranno usare l'app My Server 2012 R2 per accedere ai file nelle raccolte di SharePoint Online da qualsiasi luogo usando il dispositivo mobile o Windows Phone. [Dove è disponibile l'app My Server?](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
 Mai usato SharePoint? [Ecco cosa è possibile fare](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
## <a name="where-on-the-dashboard-will-i-manage-my-libraries-and-team-sites"></a>Da quale posizione del dashboard si gestiscono le raccolte e i siti del team?  
 Si userà la nuova scheda **SharePoint Online** , che viene aggiunta all'area di **archiviazione** del dashboard quando si integra Office 365 con il server per gestire le risorse di SharePoint Online.  

  
## <a name="what-can-i-manage-from-the-dashboard"></a>Cosa è possibile gestire dal dashboard?  
  
### <a name="manage-your-online-libraries"></a>Gestire le raccolte online  
   
|-|-|  
| Aggiungere una libreria | Nella scheda **raccolte di SharePoint** usare **Aggiungi una raccolta**. Sarà possibile effettuare tutte le normali scelte:<br /><br /> -Scegliere un sito del team e il tipo di libreria.<br />-Decidere se usare il controllo della versione.<br />-Assegnare le autorizzazioni di accesso.<br /><br /> **Suggerimento:** Per individuare le autorizzazioni del sito del team ereditate dalla libreria se non si assegnano le autorizzazioni, usare **Visualizza autorizzazioni sito**. |  
| Aprire una raccolta | Per usare il contenuto della libreria, è necessario aprirlo in Office 365. Basta selezionare la raccolta e fare clic su **Apri la raccolta**. Ciò che si può fare con il contenuto dipende dalle credenziali usate per accedere a SharePoint Online. |  
| Modificare i controlli della versione o le autorizzazioni di accesso | È possibile utilizzare **Visualizza le proprietà della libreria** per visualizzare o modificare i controlli della versione o le autorizzazioni di accesso per la raccolta. |  
| Eliminare una libreria | **Avviso:** Prima di eliminare una raccolta di SharePoint Online, assicurarsi di salvare tutti i file che si desidera conservare in un altro percorso. Quando si elimina una raccolta da SharePoint, tutto viene eliminato definitivamente. Non c'è modo di recuperare nulla.<br /><br /> Dopo aver verificato che la libreria non archivia alcuna operazione necessaria in un secondo momento, selezionare la libreria e fare clic su **Elimina la raccolta**. |  
  
### <a name="manage-your-team-sites"></a>Gestire i siti del team  
 
|-|-|  
| Gestire i siti del team di SharePoint | L'azione **Gestisci siti del team** consente di accedere a Office 365 e di gestire i siti del team di SharePoint Online. Le operazioni che è possibile eseguire in Office 365 verranno determinate dall'account online con cui si accede.<br /><br /> Quando si chiude Office 365 e si torna al dashboard, fare clic su **Aggiorna** per visualizzare le modifiche. | Visualizzare o modificare le autorizzazioni del sito del team | Poiché una libreria eredita le autorizzazioni dal sito del team per impostazione predefinita, è utile avere accesso facile al sito del team. Per visualizzare o modificare le autorizzazioni per un sito del team, selezionare il sito del team o una delle relative librerie, quindi fare clic su **Visualizza autorizzazioni sito**.<br /><br /> **Suggerimento:** Serve aiuto per i punti di le autorizzazioni del sito del team di SharePoint? Le autorizzazioni del sito del team includono un utile collegamento [Ulteriori informazioni](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924) .  
  
## <a name="tips"></a>Suggerimenti  
  
-   **Fare clic su Aggiorna per visualizzare le modifiche più recenti apportate al portale di Office 365.** È necessario aggiornare la visualizzazione dopo aver aperto Office 365 per gestire SharePoint Online. Se si tiene il dashboard aperto per lunghi periodi, fare clic su **Aggiorna** per essere certi di vedere le ultime modifiche.  
  
-   **Le operazioni che è possibile eseguire in SharePoint Online variano a seconda che si stia lavorando al dashboard o in Office 365.** Nel dashboard le modifiche a SharePoint Online vengono apportate usando l'account amministratore per l'integrazione con Office 365. Tuttavia, quando si accede a Office 365 dal dashboard, le autorizzazioni di accesso per l'account online usato determinano quali operazioni è possibile eseguire.  
  
     Per trovare l'account amministratore per l'integrazione con Office 365, aprire la scheda **office 365** nel dashboard.  
  
## <a name="other-things-you-might-want-to-do"></a>Altre operazioni possibili  
  
-   [Usare un'app My Server per lavorare con le raccolte di SharePoint Online da qualsiasi posizione](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
-   [Altre informazioni sull'assegnazione di autorizzazioni per i siti del team SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924)  
  
-   [Scopri cosa puoi fare con le funzionalità di SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
-   [Esaminare i piani aziendali di Office 365 disponibili](https://office.microsoft.com/business/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_what-is-office-365-for_Text)  
  
-   [Integrare Office 365 con il server](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gestire altri Servizi online Microsoft dal dashboard](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
