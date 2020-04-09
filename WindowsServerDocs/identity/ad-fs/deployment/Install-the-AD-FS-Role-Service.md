---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Installare il servizio ruolo ADFS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 942d34cbfcd988820a54a4f85583ce4b424a415f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855394"
---
# <a name="install-the-ad-fs-role-service"></a>Installare il servizio ruolo ADFS

È possibile utilizzare la procedura seguente per installare il servizio ruolo AD FS in un computer che esegue Windows Server 2012 R2 per diventare il primo server federativo in una Federazione server farm o un server federativo in una server farm federativa esistente.  
  
L'appartenenza a **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Per installare il ruolo server ADFS mediante l'Aggiunta guidata ruoli e funzionalità  
  
1.  Aprire Gestione server. Per aprire Server Manager, fare clic su **Server Manager** nella schermata **Start** oppure **Server Manager** nella barra delle applicazioni sul desktop. Nella scheda **Avvio rapido** del **riquadro iniziale** nella pagina **Dashboard** fare clic su **Aggiungi ruoli e funzionalità**. In alternativa, è possibile scegliere **Aggiungi ruoli e funzionalità** dal menu **Gestione**.  
  
2.  Nella pagina **Prima di iniziare** fare clic su **Avanti**.  
  
3.  Nella pagina **Selezione tipo di installazione** fare clic su **\-basata su ruoli o su\-installazione basata su funzionalità**, quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, verificare che il computer di destinazione sia selezionato e quindi fare clic su **Avanti**.  
  
5.  Nella pagina **Selezione ruoli server** fare clic su **Active Directory Federation Services**, quindi su **Avanti**.  
  
6.  Nella pagina **Selezione funzionalità** fare clic su **Avanti**. I prerequisiti richiesti sono preselezionati. Non è necessario selezionare altre funzionalità.  
  
7.  Nella pagina **Active Directory Servizio federativo \(AD FS\)** fare clic su **Avanti**.  
  
8.  Dopo aver verificato le informazioni nella pagina **Conferma selezioni per l'installazione**, fare clic su **Installa**.  
  
9. Nella pagina **Stato installazione** verificare che tutti gli elementi siano installati correttamente e quindi fare clic su **Chiudi**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Per installare il ruolo server ADFS mediante Windows PowerShell  
  
1.  Nel computer che si desidera configurare come server federativo aprire la finestra di comando di Windows PowerShell, quindi eseguire il comando seguente: `Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Vedi anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

