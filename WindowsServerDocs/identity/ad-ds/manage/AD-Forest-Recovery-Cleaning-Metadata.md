---
title: Ripristino della foresta di Active Directory-pulizia dei metadati dei controller di dominio rimossi
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: b9ba00939ccb2ee747501733fb9654edb4c8132e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824254"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Ripristino della foresta di Active Directory-pulizia dei metadati di controller di dominio scrivibili rimossi

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

La pulizia dei metadati rimuove Active Directory dati che identificano un controller di dominio nel sistema di replica.  

Utilizzare la procedura seguente per eliminare gli oggetti controller di dominio per i controller di dominio che si prevede di aggiungere di nuovo alla rete tramite la reinstallazione di servizi di dominio Active Directory.  
  
Se si utilizza la versione di Active Directory utenti e computer o Active Directory siti e servizi inclusi Strumenti di amministrazione remota del server (strumenti di amministrazione remota del server), la pulizia dei metadati viene eseguita automaticamente quando si elimina un oggetto controller di dominio.  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>Eliminazione di un controller di dominio utilizzando Active Directory utenti e computer

Quando si usa la versione di Active Directory utenti e computer o Centro di amministrazione di Active Directory in Strumenti di amministrazione remota del server (strumenti di amministrazione remota del server), la pulizia dei metadati viene eseguita automaticamente quando si elimina l'oggetto DC. Anche l'oggetto server e l'oggetto computer vengono eliminati automaticamente.  

In alternativa, è inoltre possibile utilizzare Active Directory siti e servizi in strumenti di amministrazione remota del server per eliminare un oggetto DC. Se si usano Active Directory siti e servizi, è necessario eliminare l'oggetto server associato e l'oggetto Impostazioni NTDS prima di poter eliminare l'oggetto DC.  

Per informazioni sull'installazione di strumenti di amministrazione remota del server, vedere l'articolo [strumenti di amministrazione remota del server](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
  
La procedura seguente è identica per i controller di dominio che eseguono Windows Server 2016, 2012, 2008 R2 o 2008. Il controller di dominio di destinazione dell'operazione di pulizia dei metadati può eseguire qualsiasi versione di Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Per eliminare un oggetto controller di dominio utilizzando Active Directory utenti e computer in strumenti di amministrazione remota del server  
  
1. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Utenti e computer di Active Directory**.  
2. Nell'albero della console fare doppio clic sul contenitore di dominio, quindi fare doppio clic sull'unità organizzativa **Domain Controllers** (OU).  
3. Nel riquadro dei dettagli fare clic con il pulsante destro del mouse sul controller di dominio che si desidera eliminare e quindi scegliere **Elimina**.
   ![Elimina](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. Fare clic su **Sì** per confermare l'eliminazione. Selezionare il **controller di dominio è in modo permanente offline e non può più essere abbassato tramite la casella di controllo installazione guidata di Active Directory Domain Services (Dcpromo)** e fare clic su **Elimina**.  
5. Se il controller di dominio è un server di catalogo globale, fare clic su **Sì** per confermare l'eliminazione.  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
