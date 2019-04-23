---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Installare il servizio ruolo ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831172"
---
# <a name="install-the-ad-fs-role-service"></a>Installare il servizio ruolo ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2

È possibile utilizzare la procedura seguente per installare il servizio ruolo ADFS in un computer che esegue Windows Server 2012 R2 per diventare il primo server federativo in una server farm federativa o un server federativo in una server farm federativa esistente.  
  
L'appartenenza a **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Per installare il ruolo di server ADFS mediante l'aggiunta guidata ruoli e funzionalità  
  
1.  Aprire Server Manager. Per aprire Server Manager, fare clic su **Server Manager** nel **avviare** dello schermo, o **Gestione Server** nella barra delle applicazioni sul desktop. Nella scheda **Avvio rapido** del **riquadro iniziale** nella pagina **Dashboard** fare clic su **Aggiungi ruoli e funzionalità**. In alternativa, è possibile scegliere **Aggiungi ruoli e funzionalità** dal menu **Gestione**.  
  
2.  Nella pagina **Prima di iniziare**, fare clic su **Avanti**.  
  
3.  Nel **Selezione tipo di installazione** pagina, fare clic su **ruolo\-o basata su funzionalità\-installazione basata su**e quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Selezione server di destinazione** fare clic su **Selezionare un server dal pool di server**, verificare che il computer di destinazione sia selezionato e quindi fare clic su **Avanti**.  
  
5.  Nella pagina **Selezione ruoli server** fare clic su **Active Directory Federation Services**e quindi scegliere **Avanti**.  
  
6.  Nella pagina **Selezione funzionalità** fare clic sul pulsante **Avanti**. I prerequisiti richiesti sono preselezionati automaticamente. Non è necessario selezionare altre funzionalità.  
  
7.  Nel **Active Directory Federation Services \(ADFS\)**  fare clic su **Next**.  
  
8.  Dopo aver verificato le informazioni nella **Conferma selezioni per l'installazione** pagina, fare clic su **installare**.  
  
9. Nella pagina **Stato installazione** verificare che tutti gli elementi siano installati correttamente e quindi fare clic su **Chiudi**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Per installare il ruolo di server ADFS mediante Windows PowerShell  
  
1.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell e quindi eseguire il comando seguente: `Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

