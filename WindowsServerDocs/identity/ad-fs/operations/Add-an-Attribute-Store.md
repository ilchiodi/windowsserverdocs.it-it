---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Aggiungere un archivio attributi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 103ee707c88f4e88b231a833f739cf75b6503e18
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190102"
---
# <a name="add-an-attribute-store"></a>Aggiungere un archivio attributi


Gli account utente e account del computer che richiedono l'accesso a una risorsa protetta da Active Directory Federation Services \(ADFS\) vengono archiviati in un archivio di attributi, ad esempio Active Directory Domain Services \(Active Directory Domain Services \). Il motore di rilascio delle attestazioni utilizza gli archivi di attributi per raccogliere dati che sono necessari emettere attestazioni. Dati da archivi di attributi viene quindi proiettati come attestazioni.  
  
È possibile utilizzare la procedura seguente per aggiungere un archivio attributi per il servizio federativo.  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Per aggiungere un archivio attributi  
  
1.  Aprire **Gestione ADFS**.  
  
2.  In **azioni** fare clic su **aggiungere un archivio attributi**.  

![aggiungere l'archivio di attributi](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  Nel **aggiungere un archivio attributi** finestra di dialogo, configurare le seguenti proprietà per l'archivio di attributi che si desidera aggiungere:  
  
    -   In **nome**, digitare il nome che si desidera utilizzare per identificare l'archivio di attributi.  
  
    -   Nelle **tipo di archivio attributo**, selezionare un tipo di archivio attributo supportato tra **Active Directory**, **LDAP**, oppure **SQL**.  
  
    -   In **stringa di connessione**, se è stato selezionato un protocollo di accesso di Directory di Lightweight \(LDAP\) store o un Structured Query Language \(SQL\) archivio, immettere la stringa utilizzata per stabilire una connessione all'archivio di attributi. Per gli archivi di attributi di Active Directory, non è necessaria; nessuna stringa di connessione di conseguenza, questo campo è disabilitato.  
  
        > [!NOTE]  
        > Per impostazione predefinita, ADFS crea automaticamente un archivio attributi di Active Directory.  
 
![aggiungere l'archivio di attributi](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Fare clic su **OK**.  
  
## <a name="additional-references"></a>Altri riferimenti  

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[Il ruolo degli archivi di attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
