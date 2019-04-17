---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Installare il servizio ruolo ADFS di Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-ad-fs-role-service"></a>Installare il servizio ruolo ADFS di Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2

È possibile utilizzare la procedura seguente per installare il servizio ruolo ADFS in un computer che esegue Windows Server 2012 R2 per il primo server federativo in una server farm federativa o un server federativo in una server farm federativa esistente.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Per installare il ruolo server AD FS tramite l'aggiunta guidata ruoli e funzionalità  
  
1.  Aprire Server Manager. Per aprire Server Manager, fare clic su **Server Manager** nel **Start** schermata o **Server Manager** sulla barra delle applicazioni sul desktop. Nel **avvio rapido** scheda della finestra di **iniziale** riquadro il **Dashboard** pagina, fare clic su **Aggiungi ruoli e funzionalità**. In alternativa, è possibile fare clic su **Aggiungi ruoli e funzionalità** nel **Gestisci** menu.  
  
2.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
3.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione installazione basata su ruoli o basata su**, quindi fare clic su **Avanti**.  
  
4.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, verificare che il computer di destinazione sia selezionata e quindi fare clic su **Avanti**.  
  
5.  Nel **Selezione ruoli server** pagina, fare clic su **Active Directory Federation Services**, quindi fare clic su **Avanti**.  
  
6.  Nel **selezionare le funzionalità** pagina, fare clic su **Avanti**. I prerequisiti necessari preselezionati per te. Non è necessario selezionare le altre funzionalità.  
  
7.  Nel **Active Directory Federation Services \(AD FS\)** pagina, fare clic su **Avanti**.  
  
8.  Dopo aver verificato le informazioni sul **Conferma selezioni per l'installazione** pagina, fare clic su **installare**.  
  
9. Nel **lo stato dell'installazione** pagina, verificare che tutto sia stato installato correttamente e quindi fare clic su **Chiudi**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Per installare il ruolo server AD FS tramite Windows PowerShell  
  
1.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell e quindi eseguire il comando seguente: `Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

