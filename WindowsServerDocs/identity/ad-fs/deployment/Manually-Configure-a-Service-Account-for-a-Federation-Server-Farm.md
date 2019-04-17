---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurare manualmente un Account del servizio per una Server Farm federativa
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5b5a8d198f93772903ea9b0a2b4b01075799bf0f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurare manualmente un Account del servizio per una Server Farm federativa

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si intende configurare un ambiente di server farm federativa in Active Directory Federation Services \(AD FS\), è necessario creare e configurare un account di servizio dedicato in servizi di dominio Active Directory \(AD DS\) dove risiederà la farm. È quindi possibile configurare ciascun server federativo della farm per utilizzare questo account. Quando si desidera consentire ai computer client nella rete aziendale per l'autenticazione dei server federativi in una farm ADFS utilizzando l'autenticazione integrata di Windows, è necessario completare le seguenti attività all'interno dell'organizzazione.  
  
> [!NOTE]  
> È necessario eseguire le attività di questa procedura solo una volta per l'intera server farm federativa. In seguito, quando si crea un server federativo utilizzando Configurazione guidata Server federativo AD FS, è necessario specificare lo stesso account nel **Account del servizio** pagina della procedura guidata in ogni server federativo nella farm.  
  
#### <a name="create-a-dedicated-service-account"></a>Creare un account di servizio dedicato  
  
1.  Creare un account dall'utente o del servizio dedicato nella foresta Active Directory che si trova nell'organizzazione del provider di identità. Questo account è necessario per il protocollo di autenticazione Kerberos di lavorare in uno scenario di farm e per consentire l'autenticazione pass\-through in ogni server federativo. Utilizzare questo account solo per gli scopi della farm di server federativo.  
  
2.  Modificare le proprietà dell'account utente e selezionare il **Nessuna scadenza Password** casella di controllo. Questa azione assicura che la funzione dell'account del servizio non venga interrotta in seguito a requisiti di modifica password di dominio.  
  
    > [!NOTE]  
    > Utilizzando l'account servizio di rete per questo account dedicato genererà errori casuali quando verrà tentato l'accesso tramite autenticazione integrata di Windows, perché i ticket Kerberos non verranno convalidati da un server a un altro.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Per impostare il nome SPN dell'account del servizio  
  
1.  Poiché l'identità del pool di applicazioni per l'AppPool di ADFS di Active Directory è in esecuzione come account di servizio o dall'utente di dominio, è necessario configurare \(SPN\) il nome dell'entità servizio per l'account nel dominio con lo strumento della riga di comando Setspn.exe. Setspn.exe viene installato per impostazione predefinita nei computer che eseguono Windows Server 2008. Eseguire il comando seguente in un computer appartenente allo stesso dominio in cui risiede l'account servizio o dall'utente:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Ad esempio, in uno scenario in cui tutti i server federativi sono nel cluster sotto il nome host Domain Name System \(DNS\) fs.fabrikam.com e il nome dell'account del servizio assegnato all'AppPool di ADFS di Active Directory è denominato adfs2farm, digitare il comando seguente e quindi premere INVIO:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    È necessario completare questa attività una sola volta per questo account.  
  
2.  Dopo avere modificato l'identità dell'AppPool di AD FS per l'account del servizio, impostare il \(ACLs\) gli elenchi di controllo accesso nel database di SQL Server per consentire l'accesso in lettura a questo nuovo account in modo che l'AppPool di AD FS può leggere i dati dei criteri.  
  

