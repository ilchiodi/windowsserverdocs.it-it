---
title: Ripristino della foresta Active Directory - pulizia dei metadati di controller di dominio rimossi
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adds
ms.openlocfilehash: b71cab51a362a96ab6071e5eed3cf31c4421041c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843042"
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>Ripristino della foresta Active Directory - pulizia dei metadati di controller di dominio scrivibili

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Pulizia dei metadati rimuove i dati di Active Directory che identifica un controller di dominio per il sistema di replica.  

Usare la procedura seguente per eliminare gli oggetti controller di dominio per i controller di dominio che si intende aggiungere alla rete tramite la reinstallazione di Active Directory Domain Services.  
  
Se si usa la versione di Active Directory Users and Computers o siti di Active Directory e servizi che è inclusi strumenti di amministrazione remota Server (RSAT), pulizia dei metadati viene eseguita automaticamente quando si elimina un oggetto controller di dominio.  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>L'eliminazione di un controller di dominio con Active Directory Users and Computers

Quando si usa la versione di Active Directory Users e computer o centro di amministrazione di Active Directory in remoto Server Administration strumenti (RSAT), pulizia dei metadati viene eseguita automaticamente quando si elimina l'oggetto controller di dominio. L'oggetto server e l'oggetto computer vengono anche eliminati automaticamente.  

In alternativa, è possibile anche usare Active Directory Sites and Services in amministrazione remota del server per eliminare un oggetto controller di dominio. Se si usa Active Directory Sites and Services, è necessario eliminare l'oggetto server associato e un oggetto impostazioni NTDS prima di poter eliminare l'oggetto controller di dominio.  

Per informazioni sull'installazione di amministrazione remota del server, vedere l'articolo [strumenti di amministrazione remota del Server](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).
  
La procedura seguente è lo stesso per i controller di dominio che eseguono entrambi Windows Server 2016, 2012, 2008 R2 o 2008. Il controller di dominio di destinazione dell'operazione di pulizia dei metadati può eseguire qualsiasi versione di Windows Server.  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>Per eliminare un oggetto controller di dominio usando Active Directory Users and Computers in amministrazione remota del server  
  
1. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Utenti e computer di Active Directory**.  
2. Nell'albero della console, fare doppio clic sul contenitore di dominio e quindi fare doppio clic il **controller di dominio** unità organizzativa (OU).  
3. Nel riquadro dei dettagli, fare doppio clic su controller di dominio che si desidera eliminare e quindi fare clic su **Elimina**.
   ![Elimina](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4. Fare clic su **Sì** per confermare l'eliminazione. Selezionare il **questo Controller di dominio è definitivamente offline e non può non è più essere abbassata di livello con l'Active Directory Domain Services installazione guidata (DCPROMO)** casella di controllo e fare clic su **eliminare**.  
5. Se il controller di dominio ha un server di catalogo globale, fare clic **Sì** verificare che l'eliminazione.  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
