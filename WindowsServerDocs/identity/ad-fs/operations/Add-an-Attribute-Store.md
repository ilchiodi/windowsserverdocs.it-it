---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Aggiungere un archivio attributi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0f5c9d3b0f856ab72a16930ddb5c50686d747ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358390"
---
# <a name="add-an-attribute-store"></a>Aggiungere un archivio attributi


Gli account utente e i computer che richiedono l'accesso a una risorsa protetta da Active Directory Federation Services \(AD FS @ no__t-1 vengono archiviati in un archivio di attributi, ad esempio Active Directory Domain Services \(AD DS @ no__t-3. Il motore di rilascio delle attestazioni utilizza gli archivi di attributi per raccogliere dati che sono necessari emettere attestazioni. Dati da archivi di attributi viene quindi proiettati come attestazioni.  
  
È possibile utilizzare la procedura seguente per aggiungere un archivio attributi per il servizio federativo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Per aggiungere un archivio attributi  
  
1.  Aprire **Gestione ADFS**.  
  
2.  In **azioni** fare clic su **aggiungere un archivio attributi**.  

![aggiungere l'archivio di attributi](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. Nel **aggiungere un archivio attributi** finestra di dialogo, configurare le seguenti proprietà per l'archivio di attributi che si desidera aggiungere:  
  
   -   In **nome**, digitare il nome che si desidera utilizzare per identificare l'archivio di attributi.  
  
   -   In **il tipo di archivio di attributi**, selezionare un tipo di archivio attributo supportato tra **Active Directory**, **LDAP**, o **SQL**.  
  
   -   In **stringa di connessione**, se è stato selezionato un protocollo di accesso di Directory di Lightweight \(LDAP\) store o un Structured Query Language \(SQL\) archivio, immettere la stringa utilizzata per stabilire una connessione all'archivio di attributi. Per gli archivi di attributi di Active Directory, non è necessaria; nessuna stringa di connessione Pertanto, questo campo è disabilitato.  
  
       > [!NOTE]  
       > ADFS crea automaticamente un archivio di attributi di Active Directory, per impostazione predefinita.  
 
![aggiungere l'archivio di attributi](media/Add-an-Attribute-Store/addstore2.PNG) 

4. Fare clic su **OK**.  
  
## <a name="additional-references"></a>Altri riferimenti  

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[Ruolo degli archivi di attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
