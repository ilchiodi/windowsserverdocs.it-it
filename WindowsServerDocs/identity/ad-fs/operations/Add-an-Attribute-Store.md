---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Aggiungere un archivio attributi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-an-attribute-store"></a>Aggiungere un archivio attributi

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Account utente e computer che richiedono l'accesso a una risorsa protetta da Active Directory Federation Services \(AD FS\) vengono archiviati in un archivio di attributi, ad esempio \(AD DS\) servizi di dominio Active Directory. Il motore di rilascio delle attestazioni utilizza gli archivi di attributi per raccogliere dati che sono necessari emettere attestazioni. Dati da archivi di attributi viene quindi proiettati come attestazioni.  
  
È possibile utilizzare la procedura seguente per aggiungere un archivio di attributi per il servizio federativo.  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Per aggiungere un archivio di attributi  
  
1.  Apri **gestione di ADFS**.  
  
2.  In **azioni** fare clic su **aggiungere un archivio attributi**.  

![Aggiungere l'archivio di attributi](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  Nel **aggiungere un archivio attributi** finestra di dialogo casella, configurare le seguenti proprietà per l'archivio di attributi che si desidera aggiungere:  
  
    -   In **nome visualizzato**, digitare il nome che si desidera utilizzare per identificare l'archivio di attributi.  
  
    -   In **tipo di archivio attributo**, selezionare un tipo di archivio attributo supportato tra **Active Directory**, **LDAP**, o **SQL**.  
  
    -   In **stringa di connessione**, se è stato selezionato un archivio di Lightweight Directory Access Protocol \(LDAP\) o un archivio \(SQL\) Structured Query Language, immettere la stringa utilizzata per stabilire una connessione all'archivio di attributi. Per gli archivi di attributi Active Directory, non è necessaria; nessuna stringa di connessione Pertanto, questo campo è disabilitato.  
  
        > [!NOTE]  
        > ADFS crea automaticamente un archivio di attributi di Active Directory, per impostazione predefinita.  
 
![Aggiungere l'archivio di attributi](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Fare clic su **OK**.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  

[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md)
  
[Il ruolo degli archivi attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
